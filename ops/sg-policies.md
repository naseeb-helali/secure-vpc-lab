ops/sg-policies.md

# SG Policies (Summary)

- Bastion SG: اسمح TCP/22 فقط من عنوان IP ثابت للشركة/لديك (صفحة 20)24.
- NAT Instance SG: لا حاجة لـInbound من الإنترنت؛ Outbound افتراضي.
- App SG: لا Inbound من الإنترنت؛ الإدارة عبر Bastion فقط.
