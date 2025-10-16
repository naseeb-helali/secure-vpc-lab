# Phase 1: Bastion Host (Public Subnet)

## Objective
Provision a hardened entry point (SSH jump host) in the public subnet to administratively access private resources without exposing them to the internet.

## Why this matters? 
- Bastion hosts are designed to withstand attacks and must be deployed in **public subnets**.
- Prefer **Elastic IP** and **restrict source IPs** in Security Groups/NACLs to the minimum required.
- High availability setups using Auto Scaling are possible; **private subnets still require NAT** for outbound internet access.

## Instance Specs (lab-friendly)
- AMI: Amazon Linux 2 (or preferred Linux)
- Type: t2.micro (small to minimize cost)
- Subnet: `public-a` (from Step 02)
- Public IP: enabled (or attach an Elastic IP)
- Security Group: `sg-bastion` (see below)
- Key Pair: required (SSH)

## Security Group — sg-bastion
- **Inbound**: SSH (22) from a fixed, trusted IP only.
- **Outbound**: allow all (default).

> Keep inbound minimal. Do **not** open SSH to `0.0.0.0/0`. Source restriction is explicitly recommended.

## Console Instructions
1. Create/Import **Key Pair**.
2. Create **Security Group** `sg-bastion` with SSH(22) allowed only from your fixed IP.
3. Launch **EC2** in **public-a**:
   - Enable public IP assignment (or allocate/attach **Elastic IP** after launch — recommended on.
   - Attach `sg-bastion` and your key pair.
4. (Optional) Allocate & attach **Elastic IP** to the Bastion.
5. Validate SSH:
   ```bash
   ssh -i ~/.ssh/<key>.pem ec2-user@<BASTION_PUBLIC_IP>
