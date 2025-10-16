---

(1) docs/readme.md

> الصق التالي كما هو، ثم خصّصه لاحقًا:



# Secure, Cost-Optimized AWS VPC Lab (Phase 1)

## 🎯 الهدف
بناء مختبر VPC آمن ومقتصد في AWS يثبت: عزل الشبكات (Public/Private)، خروج آمن عبر NAT Instance،
وصول خاص إلى S3 عبر Gateway Endpoint، إدارة آمنة بـBastion، ومراقبة عبر VPC Flow Logs.

## 🧭 النطاق (Phase 1)
- VPC + Subnets عبر أكثر من AZ.
- Bastion Host في Public Subnet مع تقييد المصدر (صفحة 20)7.
- NAT Instance بديلًا عن NAT Gateway لتقليل التكلفة (صفحات 5–6، 13)8.
- S3 Gateway Endpoint للوصول من الشبكات الخاصة دون إنترنت (صفحات 51، 54–55)9.
- VPC Flow Logs إلى CloudWatch أو S3 (صفحة 64)10.

## 🧩 لماذا هذه الاختيارات؟
- **NAT Gateway** يفرض تكلفة بالساعة وبالجيجابايت (صفحة 13)11، لذا نستخدم **NAT Instance** للمختبر.
  - NAT Instance: في Public Subnet + Route افتراضي من Private إليه + تعطيل Source/Dest Check (صفحات 5–6)12.
- **Gateway Endpoint (S3)** مجاني ويجنب المرور عبر NAT (صفحات 54–55)13.
- **Bastion Host** للإدارة الآمنة وتقييد المصدر (صفحة 20)14.
- **Flow Logs** لالتقاط Source/Dest IP/Port وحالة ACCEPT/REJECT (صفحة 64)15.

## 🧱 المعمارية (TL;DR)
- Public: Bastion + NAT Instance
- Private: App EC2
- S3 Gateway Endpoint، Flow Logs على مستوى VPC.

## 🧪 التحقق السريع
- SSH → Bastion → SSH → EC2 داخل Private.
- تحديث حزم من Private يمر عبر NAT Instance.
- `aws s3 ls` من Private يمر عبر S3 Gateway Endpoint (بدون NAT).
- راجع Flow Logs لنتائج ACCEPT/REJECT.

## 🧹 Teardown
راجع `runbooks/teardown.md` لتفكيك الموارد وتقليل التكلفة.


---
