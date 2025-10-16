---

## 📗 ملف GitHub: `docs/decisions/adr-0002-routing-and-endpoints.md`

```markdown
# ADR-0002: Private Routing via NAT Instance + S3 Gateway Endpoint

## Context
Private subnets need secure outbound connectivity for updates.  
A NAT Instance in the public subnet provides egress (pages 5–6)3.  
S3 access from private subnets can use a free Gateway Endpoint (pages 54–55)4.

## Decision
- Route `0.0.0.0/0` from Private-A to NAT Instance-A.  
- Enable **S3 Gateway Endpoint** and associate it with private route tables.  

## Consequences
✅ Reduced NAT traffic cost.  
✅ Enhanced security (no internet exposure).  
⚠️ Single-point NAT Instance (acceptable for Phase 1 Lab).  

## Status
Accepted for Phase 1.  
To be revisited with NAT Gateway in Phase 2.
