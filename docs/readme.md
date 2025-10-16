# Secure, Cost-Optimized AWS VPC Lab (Phase 1)

## 🎯 الهدف
بناء مختبر VPC آمن ومقتصد في AWS يثبت: عزل الشبكات (Public/Private)، خروج آمن عبر NAT Instance،
وصول خاص إلى S3 عبر Gateway Endpoint، إدارة آمنة بـBastion، ومراقبة عبر VPC Flow Logs.

## 🧭 النطاق (Phase 1)
- VPC + Subnets عبر أكثر من AZ.
- Bastion Host في Public Subnet مع تقييد المصدر.
- NAT Instance بديلًا عن NAT Gateway لتقليل التكلفة.
- S3 Gateway Endpoint للوصول من الشبكات الخاصة دون إنترنت.
- VPC Flow Logs إلى CloudWatch أو S3.

## 🧩 لماذا هذه الاختيارات؟
- **NAT Gateway** يفرض تكلفة بالساعة وبالجيجابايت، لذا نستخدم **NAT Instance** للمختبر.
  - NAT Instance: في Public Subnet + Route افتراضي من Private إليه + تعطيل Source/Dest Check.
- **Gateway Endpoint (S3)** مجاني ويجنب المرور عبر NAT.
- **Bastion Host** للإدارة الآمنة وتقييد المصدر.
- **Flow Logs** لالتقاط Source/Dest IP/Port وحالة ACCEPT/REJECT.

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

## 🧱 Architecture Overview

### Network Structure
- **Public Subnet (10.0.1.0/24)** — Hosts Bastion & NAT Instance.  
- **Private Subnet (10.0.2.0/24)** — Hosts Application EC2.  
- **S3 Gateway Endpoint** — Internal access to S3 (free).  
- **VPC Flow Logs** — Sent to CloudWatch or S3 for visibility.

### Routing Logic
| Route Table | Destination | Target |
|--------------|--------------|--------|
| Public RT | 0.0.0.0/0 | Internet Gateway |
| Private RT | 0.0.0.0/0 | NAT Instance |
| Private RT | pl-aws-s3 | Gateway Endpoint |


### Subnet Plan
| Name      | CIDR         | AZ   | Purpose                |
|-----------|--------------|------|------------------------|
| public-a  | 10.0.1.0/24  | AZ-A | Bastion, NAT instance  |
| private-a | 10.0.2.0/24  | AZ-A | Application EC2        |


### Security Highlights
- Bastion accessible only from fixed IP via SSH(22).  
- Private EC2 accessible only from Bastion.  
- NAT Instance restricted (outbound only).  

### Trade-offs
- **NAT Instance** chosen for cost efficiency.  
- **Flow Logs** enabled with short retention for cost control.

### ✅ Sanity Checklist
- [ ] VPC created (10.0.0.0/16)
- [ ] Public Subnet (10.0.1.0/24) linked to IGW
- [ ] Private Subnet (10.0.2.0/24) linked to NAT Instance
- [ ] S3 Gateway Endpoint associated with private RT
- [ ] Bastion SG: SSH only from fixed IP
- [ ] App SG: SSH only from Bastion
- [ ] Flow Logs enabled (CloudWatch or S3)
