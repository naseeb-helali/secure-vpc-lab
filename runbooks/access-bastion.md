(3) runbooks/access-bastion.md

# Runbook — Access via Bastion

## Principles
- Bastion في Public Subnet مع Elastic IP مفضل وتقييد المصدر في SG/NACL (صفحة 20)20.

## SSH (Linux/Mac)
ssh -i ~/.ssh/<key>.pem ec2-user@<BASTION_PUBLIC_IP>

## From Bastion to Private
ssh -i ~/.ssh/<key>.pem ec2-user@<PRIVATE_EC2_IP>

## Windows Tip
تأكد من تشغيل OpenSSH Agent وإضافة المفتاح (Page 24)21.
