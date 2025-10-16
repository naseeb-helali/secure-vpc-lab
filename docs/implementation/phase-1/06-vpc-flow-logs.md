# VPC Flow Logs (Network Observability)

## Objective
Enable VPC Flow Logs to capture network traffic metadata (source IP, destination IP, ports, action ACCEPT/REJECT) for monitoring and troubleshooting connectivity across subnets and interfaces.

## Why this matters? 
- Flow Logs record traffic at **VPC**, **Subnet**, or **Network Interface** levels.  
- Data can be sent to **CloudWatch Logs** or **S3** for analysis.  
- They provide insight into routing, security group, and NACL issues without packet inspection.

## Console Instructions
1. Go to **VPC → Your VPCs → vpc-secure-lab → Flow Logs → Create flow log**.
2. **Filter:** `All` (for the lab).
3. **Destination:**  
   - Option 1 — **CloudWatch Logs**:  
     - Create a new Log Group named `/aws/vpc/secure-lab/flowlogs`.  
     - Create a new IAM Role allowing VPC to publish to CloudWatch.  
   - Option 2 — **S3 Bucket** if you prefer raw log storage.  
4. **Aggregation Interval:** 10 minutes (default).  
5. Add tags (e.g., `Project=secure-vpc-lab`, `Env=lab`).  
6. Create the Flow Log.

## Validation
1. SSH to the **private EC2** via Bastion.  
2. Run:
   ```bash
   curl -I https://aws.amazon.com
   aws s3 ls

This shows traffic from Private EC2 to S3 and Bastion with status ACCEPT/REJECT.

3. In CloudWatch Logs, check /aws/vpc/secure-lab/flowlogs for entries like:
10.0.2.10 54.239.28.85 443 49152 6 ACCEPT OK
10.0.2.10 10.0.1.10 22 50432 6 REJECT SG
4. (Optional) Temporarily tighten a Security Group to generate REJECT records for testing.



## Cost and Retention

Flow Logs are low cost but not free — limit retention to short periods.

Use S3 destination for long-term archival and CloudWatch for active monitoring.


## Best Practices

Enable Flow Logs at VPC level for complete coverage.

Use filters in CloudWatch to trace specific ENIs or private IPs.

Integrate with AWS Lambda or Athena for analysis in Phase 2.


## Verification Checklist

[ ] Flow Logs enabled for vpc-secure-lab.

[ ] Log Group exists (/aws/vpc/secure-lab/flowlogs).

[ ] Entries show ACCEPT and REJECT records.

[ ] Retention policy set (≤ 7 days for lab).

[ ] No impact on traffic performance observed.
