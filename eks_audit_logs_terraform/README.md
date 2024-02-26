# EKS Audit Log Integration with Lacework

This Terraform module automates the integration of Amazon EKS audit logs with Lacework for enhanced security and compliance monitoring. It provisions necessary AWS resources and configures them for optimal integration with Lacework.

## AWS Resources Provisioned

This module sets up the following resources:

- **S3 Buckets:**
  - A primary bucket for storing EKS audit logs.
  - An optional bucket dedicated to access logs.

- **KMS Keys and Aliases:**
  - Encrypts EKS Audit logs, SNS topics, and Kinesis Firehose data streams.
  - An alias for easier KMS key management.

- **SNS Topic:**
  - For downstream processing of audit log data.

- **IAM Roles and Policies:**
  - AWS Kinesis Firehose IAM role with S3 write access.
  - CloudWatch logs IAM role with Kinesis Firehose put record permissions.
  - Cross-account IAM role for Lacework with comprehensive access policies.

- **Kinesis Firehose Delivery Stream:**
  - Processes and stores logs efficiently.

- **CloudWatch Log Subscription Filters:**
  - Filters and forwards logs to Kinesis Firehose.

### Miscellaneous

- **Random ID Generation:** Ensures unique naming for resources.
- **IAM Role Policy Attachments:** Links IAM policies to their respective roles.
- **S3 Bucket Enhancements:** Configures public access blocks, versioning, server-side encryption, lifecycle rules, and ownership controls.
- **Dependency Management:** Utilizes a `time_sleep` resource to sequence resource creation.

## Configuration

Leverage an existing cross-account IAM role by specifying `iam_role_arn` and `iam_role_external_id` in your `terraform.tfvars` file. Provide an array of EKS cluster names and corresponding CloudWatch regions for monitoring.

### Manual Configuration in Lacework

After applying this module, you'll need to manually configure the Lacework console:

1. Navigate to **Settings** -> **Cloud Accounts** -> **Add New** -> **AWS** -> **Manual Configuration** -> **EKS Audit Log**.
2. Provide a canonical name for the integration.
3. Enter your AWS Account ID.
4. Specify the External ID and Role ARN of the pre-existing cross-account role.

### Required Terraform Output Values

You will need the following outputs from this Terraform module for Lacework configuration:

- **SNS ARN:** The ARN of the SNS topic created.
- **S3 Bucket ARN:** The ARN of the S3 bucket storing EKS audit logs.

## Conclusion

This module facilitates a seamless integration process by automating the provisioning of required AWS resources and guiding you through the final manual steps in the Lacework console for a complete setup.