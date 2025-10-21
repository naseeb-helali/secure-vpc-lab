Secure, Cost-Optimized AWS VPC Lab (Phase 1)

🎯 Objective

Build a secure and cost-efficient AWS VPC lab that demonstrates: network isolation (Public/Private), secure outbound via NAT Instance, private access to S3 via Gateway Endpoint, secure management via Bastion, and monitoring via VPC Flow Logs.

🧭 Scope (Phase 1)

VPC + Subnets across multiple AZs.

Bastion Host in Public Subnet with restricted source.

NAT Instance instead of NAT Gateway to reduce cost.

S3 Gateway Endpoint for access from private networks without Internet.

VPC Flow Logs to CloudWatch or S3.


🧩 Why These Choices?

NAT Gateway incurs hourly and per-GB costs, so we use a NAT Instance for the lab.

NAT Instance: in Public Subnet + default route from Private to it + disable Source/Dest Check.


Gateway Endpoint (S3) is free and avoids traffic through NAT.

Bastion Host for secure management with restricted source.

Flow Logs to capture Source/Dest IP/Port and ACCEPT/REJECT status.


🧱 Architecture (TL;DR)

Public: Bastion + NAT Instance

Private: App EC2

S3 Gateway Endpoint, Flow Logs at VPC level.


🧪 Quick Verification

SSH → Bastion → SSH → EC2 inside Private.

Package updates from Private go through NAT Instance.

aws s3 ls from Private goes through S3 Gateway Endpoint (without NAT).

Review Flow Logs for ACCEPT/REJECT results.


🧹 Teardown

Refer to runbooks/teardown.md to dismantle resources and reduce cost.

🧱 Architecture Overview

Network Structure

Public Subnet (10.0.1.0/24) — Hosts Bastion & NAT Instance.

Private Subnet (10.0.2.0/24) — Hosts Application EC2.

S3 Gateway Endpoint — Internal access to S3 (free).

VPC Flow Logs — Sent to CloudWatch or S3 for visibility.


Routing Logic

Route Table	Destination	Target

Public RT	0.0.0.0/0	Internet Gateway
Private RT	0.0.0.0/0	NAT Instance
Private RT	pl-aws-s3	Gateway Endpoint


Subnet Plan

Name	CIDR	AZ	Purpose

public-a	10.0.1.0/24	AZ-A	Bastion, NAT instance
private-a	10.0.2.0/24	AZ-A	Application EC2


Security Highlights

Bastion accessible only from fixed IP via SSH(22).

Private EC2 accessible only from Bastion.

NAT Instance restricted (outbound only).


Trade-offs

NAT Instance chosen for cost efficiency.

Flow Logs enabled with short retention for cost control.


✅ Sanity Checklist

[ ] VPC created (10.0.0.0/16)

[ ] Public Subnet (10.0.1.0/24) linked to IGW

[ ] Private Subnet (10.0.2.0/24) linked to NAT Instance

[ ] S3 Gateway Endpoint associated with private RT

[ ] Bastion SG: SSH only from fixed IP

[ ] App SG: SSH only from Bastion

[ ] Flow Logs enabled (CloudWatch or S3)
