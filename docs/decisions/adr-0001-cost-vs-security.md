(2) docs/decisions/adr-0001-cost-vs-security.md

# ADR-0001: NAT Instance vs NAT Gateway (Phase 1)

## Context
- NAT Gateway تكلفة بالساعة وبالجيجابايت (صفحة 13)16.
- NAT Gateway لا يدعم Security Groups (صفحة 9)17 ويوصى بواحد لكل AZ للـHA (صفحة 10)18.
- NAT Instance: يجب أن يكون في Public Subnet، Route من Private إليه، تعطيل Source/Dest Check (صفحات 5–6)19.

## Decision
اعتماد **NAT Instance** في المختبر (Phase 1) لتقليل التكلفة مع ضبط SG/NACL.

## Consequences
- ✅ تكلفة شبه صفرية للمختبر.
- ✅ مرونة عالية للتجارب.
- ⚠️ ليس HA مثل NAT Gateway.
- 🔁 في Phase 2/Production: الترقية إلى **NAT Gateway**.

## Status
Accepted (Phase 1). To be revisited in Phase 2.
