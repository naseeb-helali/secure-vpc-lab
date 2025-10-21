# Runbook — Access via Bastion

## Principles
- Bastion in the Public Subnet with a preferred Elastic IP and source restriction in SG/NACL.

## SSH (Linux/Mac)
ssh -i ~/.ssh/<key>.pem ec2-user@<BASTION_PUBLIC_IP>

## From Bastion to Private
ssh -i ~/.ssh/<key>.pem ec2-user@<PRIVATE_EC2_IP>

## Windows Tip
- Make sure the OpenSSH Agent is running and the key is added.
