# Phase 1: S3 Gateway Endpoint (Private Access)

## Objective
Allow private subnet instances to access Amazon S3 **without using the NAT instance or internet** — improving security and reducing cost to zero for S3 traffic.

## Why this matters (from study material)
- **Gateway Endpoints** connect private subnets directly to AWS services over the internal AWS network.  
- For **S3**, the endpoint type is **Gateway** (not Interface).  
- **Free of charge** — avoids NAT data processing costs (pages 51, 54–55)1.  
- Ensures data never traverses the public internet.

## Console Instructions
1. Navigate to **VPC → Endpoints → Create endpoint**.
2. **Service category**: AWS services.
3. **Service name**: `com.amazonaws.<region>.s3`
4. **VPC**: `vpc-secure-lab`
5. **Type**: **Gateway**
6. **Route tables**: select `rtb-private-a` (private route table).
7. **Policy**: `Full Access` (lab only; restrict in production).
8. Create endpoint.

Once created, a route to `pl-aws-s3` automatically appears in the private route table.

## Validation
1. SSH into the **private EC2** through the **Bastion**.
2. Run:
   ```bash
   aws s3 ls

You should see S3 bucket listings without NAT involvement. 3. (Optional) Stop the NAT instance temporarily — the command should still work, proving that S3 traffic uses the Gateway Endpoint.

## Verification Checklist

[ ] Endpoint created (com.amazonaws.<region>.s3).

[ ] Associated with private route table (rtb-private-a).

[ ] Route table shows pl-aws-s3 destination.

[ ] aws s3 ls succeeds from private EC2 even if NAT instance is stopped.

[ ] No additional data transfer cost (endpoint is free).


## Notes

S3 Gateway Endpoints are regional and automatically scale.

Use IAM or Endpoint Policies to limit accessible buckets (for production).

In Phase 2, this endpoint will integrate with IaC (Terraform) for reproducibility.
