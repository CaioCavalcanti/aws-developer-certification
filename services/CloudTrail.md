# AWS CloudTrail
CloudTrail is a service that helps you enable governance, compliance and operational risk **auditing of your AWS account**.

## Useful Links
- [AWS - CloudTrail FAQs](https://aws.amazon.com/cloudtrail/faqs/)
- [AWS - Security Best Practices in AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/best-practices-security.html)

## General Notes
- **Actions taken by an user, role or an AWS service** are recorded as events in CloudTrail
- Events include actions taken in **AWS Console, AWS CLI and AWS SDKs and APIs**
- Is enabled when the AWS account is created
- When an activity occurs in your AWS account, it is **recorded in a CloudTrail event**
    - You can view events in the **Event history** on CloudTrail console
- For developer exam, is important to understand the **difference between CloudWatch, CloudTrail and AWS Config**
- Event history keeps the past **90 days of activity** in your AWS account, and you can configure a **trail** to send records to a S3 bucket.