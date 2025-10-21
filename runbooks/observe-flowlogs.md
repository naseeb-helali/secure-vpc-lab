# Runbook — Observe VPC Flow Logs

## What you'll see
- Source IP/Port, Destination IP/Port, Status (ACCEPT/REJECT).
- Publish to CloudWatch Logs or S3.

## Quick checks
- Filter REJECT entries to identify failure causes (SG/NACL/Route).
- Target the ENI/Private IP of the EC2 instance during inspection.
