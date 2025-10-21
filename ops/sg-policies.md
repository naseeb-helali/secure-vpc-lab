# SG Policies (Summary)

- Bastion SG: اسمح TCP/22 فقط من عنوان IP ثابت للشركة/لديك.
- NAT Instance SG: لا حاجة لـInbound من الإنترنت؛ Outbound افتراضي.
- App SG: لا Inbound من الإنترنت؛ الإدارة عبر Bastion فقط.
