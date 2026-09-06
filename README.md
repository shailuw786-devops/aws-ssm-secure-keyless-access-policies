# AWS SSM Keyless Access Policies

This repository provides reusable IAM policies and tagging strategies to enable secure, tag-based access via AWS Systems Manager (SSM) Session Manager, eliminating the need to manage static SSH keys or open inbound port 22.

## Key Features
* **Tag-Based Scoping:** Restricts user sessions to specific environments (e.g., `Environment=Production`, `Project=AppX`).
* **Keyless Shell Access:** Enables interactive CLI sessions through AWS SSM.
* **Audit Logging:** Integrates with AWS CloudWatch and S3 for session logging.

## IAM Policy Example: Application-Specific SSM Access

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSSMStartSessionWithTags",
      "Effect": "Allow",
      "Action": [
        "ssm:StartSession"
      ],
      "Resource": [
        "arn:aws:ec2:*:*:instance/*"
      ],
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Environment": "Production",
          "aws:ResourceTag/Application": "PaymentGateway"
        }
      }
    },
    {
      "Sid": "AllowSSMDocumentAccess",
      "Effect": "Allow",
      "Action": [
        "ssm:StartSession",
        "ssm:TerminateSession",
        "ssm:ResumeSession"
      ],
      "Resource": [
        "arn:aws:ssm:*:*:document/SSM-SessionManagerRunShell"
      ]
    },
    {
      "Sid": "AllowDescribeForCLI",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ssm:DescribeSessions",
        "ssm:GetConnectionStatus"
      ],
      "Resource": "*"
    }
  ]
}
