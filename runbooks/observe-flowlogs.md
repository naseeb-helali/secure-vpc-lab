runbooks/observe-flowlogs.md

# Runbook — Observe VPC Flow Logs

## What you'll see
- Source IP/Port, Destination IP/Port, Status (ACCEPT/REJECT) (صفحة 64)22.
- نشر إلى CloudWatch Logs أو S3 (صفحة 64)23.

## Quick checks
- فلترة REJECT لمعرفة أسباب الفشل (SG/NACL/Route).
- استهداف ENI/Private IP الخاص بالـEC2 أثناء البحث.
