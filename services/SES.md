# Amazon Simple Email Service (SES)
SES is an email service designed to help marketing teams and application developers send marketing, notification and transactional emails to their customers using a pay as you go model.

## Useful Links
- [AWS - SES FAQs](https://aws.amazon.com/ses/faqs/)
- [AWS - Best Practices for Sending Email Using Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html)

## General Notes
- Can be used to **receive e-mails** and incoming emails can be delivered automatically to a S3 bucket or **trigger a Lambda function or SNS notification**
- Requires only an email address to start sending messages
    - Different from SNS, where consumers must subscribe to a topic to receive notification
- For the exam is importnant to know the differences between SES and SNS