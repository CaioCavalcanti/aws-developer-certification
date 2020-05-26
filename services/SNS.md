# Amazon Simple Notification Service (SNS)
SNS is a fully managed **pub/sub messaging service** that enables you to decouple microservices, distributed systems and serverless applications.

## Useful Links
- [AWS - SNS FAQs](https://aws.amazon.com/sns/faqs/)
- [AWS - Amazon SNS security best practices](https://docs.aws.amazon.com/sns/latest/dg/sns-security-best-practices.html)

## General Notes
- Instantaneous, **push-based delivery** (no polling, as in SQS)
- You can send **push notifications** to:
    - Mobile devices
    - SMS text message
    - E-mail
    - SQS queue
    - HTTP endpoint
    - Trigger Lambda functions
- Allows you to group multiple recipients using **topics**, an "access point" that allows recipients to dynamically subscribe to identical copies of the same notification
- A **topic supports deliveries to multiple endpoint types** (i.e. iOS, Android and SMS), and SNS delivers the formatted copies of your message to each subscribe
- All messages are **stored redundantly across multiple AZs**
- **Maximum message size is 256KB**, and **each 64KB chuck of published message is billed as 1 request**
- Pricing:
    - First 1 million SNS requests per month are free
    - $0.50 per million mobile push notification
    - $0.60 per million notification deliveries over HTTP
    - $0.75 per 100 notification deliveries over SMS (based on region)
    - $2.00 per 100,000 notification deliveries over E-mail
    - No charge for deliveries to SQS and Lambda