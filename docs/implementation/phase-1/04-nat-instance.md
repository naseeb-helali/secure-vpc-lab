# Phase 1: NAT Instance (Public Subnet) + Private Egress Routing

## Objective
Provide secure outbound internet access for private subnets using a **NAT Instance** (lab-friendly and low-cost), without exposing private instances to inbound internet.

## Why this design
- NAT instances reside in **public subnets**; private route tables send default traffic to the NAT instance; **Source/Destination Check must be disabled** so the instance can forward traffic.
- This keeps private EC2s unreachable from the internet while allowing outbound updates (OS packages, language/runtime deps).
- In production you would typically use **NAT Gateway** (managed/HA, but billed per hour + per GB). For **Phase 1 (lab)** we choose a **NAT Instance** to minimize cost.

## Prerequisites
- VPC, subnets, IGW, and route tables from Step 02.
- Bastion host from Step 03 (for administrative access to private EC2s).

## Security Group (sg-nat)
- **Inbound**: none required from the internet for NAT functionality.
- **Outbound**: allow all (default).
- Administration should be performed via the Bastion, not from the internet.

## Launch the NAT Instance
- AMI: **Amazon Linux 2**
- Type: **t2.micro**
- Subnet: `public-a`
- Public IPv4: **Enabled** (required for NAT egress)
- Security Group: `sg-nat`
- Key Pair: optional (administration via Bastion recommended)
- **User Data** (enable IP forwarding and MASQUERADE):

```bash
#!/bin/bash
set -euxo pipefail

# Enable IPv4 forwarding
sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf

# Persist and configure NAT for the private CIDR (adjust if different)
PRIVATE_CIDR="10.0.0.0/16"

# Install iptables if needed (Amazon Linux 2)
yum -y install iptables-services || true

# Flush and set basic iptables rules
iptables -t nat -F
iptables -F

# MASQUERADE outbound traffic from private CIDR to the instance's public interface
iptables -t nat -A POSTROUTING -s ${PRIVATE_CIDR} -j MASQUERADE

# Allow forwarding
iptables -A FORWARD -s ${PRIVATE_CIDR} -j ACCEPT
iptables -A FORWARD -d ${PRIVATE_CIDR} -m state --state ESTABLISHED,RELATED -j ACCEPT

# Save rules
service iptables save || true
systemctl enable iptables || true
systemctl restart iptables || true
