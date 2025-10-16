# Network Setup (VPC, Subnets, Route Tables)

## Objective
Stand up the base network: one VPC, one public subnet for internet-facing utilities (Bastion/NAT instance), and one private subnet for workloads. Prepare route tables for correct isolation and future egress via a NAT instance.

## Target Topology
- **VPC**: `10.0.0.0/16`
- **Public-A**: `10.0.1.0/24` (AZ-A) — will host Bastion and the NAT instance
- **Private-A**: `10.0.2.0/24` (AZ-A) — will host the app EC2

## Console Instructions
1. **Create VPC**  
   - VPC name: `vpc-secure-lab`  
   - IPv4 CIDR: `10.0.0.0/16`

2. **Create Subnets**  
   - `public-a` → `10.0.1.0/24` (AZ-A)  
   - `private-a` → `10.0.2.0/24` (AZ-A)

3. **Create and Attach Internet Gateway**  
   - Create IGW → Attach to `vpc-secure-lab`.

4. **Create Route Tables & Associations**  
   - `rtb-public-a` → associate with `public-a` → add route: `0.0.0.0/0 → IGW`  
   - `rtb-private-a` → associate with `private-a` → **no internet route yet** (local only).

5. **Tagging (optional)**  
   - `Project=secure-vpc-lab`, `Env=lab`.

## Rationale
- **NAT instance must reside in a public subnet; private subnets route their default traffic to it; disable S/D check**.  
- **S3 Gateway Endpoint** is free and avoids NAT for S3 access.  
- **Bastion Host** belongs in a public subnet with restricted source IPs.  
- **VPC Flow Logs** capture source/destination IP/Port and accept/reject status.

## Validation Checklist
- [ ] VPC exists with CIDR `10.0.0.0/16`.  
- [ ] `public-a` associated to `rtb-public-a` with `0.0.0.0/0 → IGW`.  
- [ ] `private-a` associated to `rtb-private-a` with **no** internet route yet.  
- [ ] Tagging applied (optional).

## Next Steps
- Step 03: Bastion Host (public) and secure SSH access.  
- Step 04: NAT Instance (public) and add `0.0.0.0/0 → NAT instance` to `rtb-private-a`.  
- Step 05: S3 Gateway Endpoint for private S3 access.  
- Step 06: VPC Flow Logs for visibility.
