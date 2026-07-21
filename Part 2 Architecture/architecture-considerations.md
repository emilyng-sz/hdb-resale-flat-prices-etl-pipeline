# Architecture Considerations

![Architecture Diagram](AWS_architecture_diagram.png)

## Data Ingestion Flow
1. EventBridge triggers the scheduled ingestion job.
2. Lambda downloads datasets from data.gov.sg over HTTPS.
3. Lambda uploads raw datasets to Amazon S3 using the S3 Gateway Endpoint.
4. AWS Glue validates and transforms the data.
5. Cleaned datasets are written into the Cleansed and Curated zones.
6. Metadata is registered in the AWS Glue Data Catalog.

## Assumptions
- AWS accounts are separated into Security, Data Lake and Analytics.
- All compute resources run in private subnets.
- Outbound internet access occurs through a NAT Gateway.
- Amazon S3 Block Public Access is enabled.
- Data is encrypted using SSE-KMS.
- All network communication uses TLS 1.2+.
- Least privilege is enforced using IAM and Lake Formation.

## Design Considerations
### Security

- Private subnets
- NAT Gateway for controlled outbound access
- IAM least privilege
- SSE-KMS encryption
- Lake Formation permissions

### Scalability

- Serverless ingestion
- S3 storage
- Athena serverless queries

### Reliability

- EventBridge scheduling
- CloudWatch monitoring
- CloudTrail auditing

### Performance

- S3 Gateway Endpoint
- Multipart uploads
- Partitioned data lake

### Cost Optimisation

- Athena pay-per-query
- S3 lifecycle policies
- Glue serverless ETL