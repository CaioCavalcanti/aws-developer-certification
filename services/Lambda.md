# AWS Lambda
Lets you run your code without provisioning or managing servers or containers, you pay only for the compute time you consume.

## Useful Links
- [AWS - Lambda FAQs](https://aws.amazon.com/lambda/faqs/)
- [AWS - Best Practices for Working with AWS Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
- [AWS Architecture Blog - How to Design Your Serverless Apps for Massive Scale](https://aws.amazon.com/blogs/architecture/how-to-design-your-serverless-apps-for-massive-scale/)

## General Notes
- Brief history:
    - Data Centre: on-prem
    - IaaS: EC2
    - PaaS: Elastic Beanstalk
    - Containers: ECS
    - Serverless: Lambda
- Benefits:
    - **Serverless**
    - Continuous scaling
    - Low cost
- Common use cases:
    - **Event-driven applications**, where the Lambda functions runs your code in response to events such as changes on S3 bucket or DynamoDB table
    - **API** where it runs your code in response to HTTP requests using API Gateway or AWS SDKs
- Languages supported:
    - Node.js
    - Java
    - Python
    - C#
    - Go
    - PowerShell
    - Ruby
- **Price** is based on:
    - **Number of requests**
        - First 1 million requests are free (per month)
        - $0.20 per million requests after that
    - **Duration**
        - Calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms
        - Price depends on the amount of memory you allocate to your function, you are charged $0.00001667 for every GB-second used
- Amazon Alexa is powered by Lambda
- **Scales out (not up) automatically**
- Lambda functions are **independent**, 1 event = 1 function
- Lambda functions **can trigger other lambda functions**
- **Architectures can get extremely complex**
    - AWS X-Ray allows you to debug
    - Additional effort to test, deploy and manage multiple environments as the application grows
- Lambda can do things globally, such as backing up S3 buckets to different regions

## Triggers
- Services that **Lambda reads event from**:
    - Amazon Kinesis
    - Amazon DynamoDB
    - Amazon SQS
- Services that **invoke Lambda functions synchronously**:
    - Elastic Load Balancing (Application Load Balancer)
    - Amazon Cognito
    - Amazon Lex
    - Amazon Alexa
    - Amazon API Gateway
    - Amazon CloudFront (Lambda@Edge)
    - Amazon Kinesis Data Firehose
    - AWS Step Functions
    - Amazon Simple Storage Service Batch
- Services that **invoke Lambda functions asynchronously**
    - Amazon Simple Storage Service
    - Amazon Simple Notification Service
    - Amazon Simple Email Service
    - AWS CloudFormation
    - Amazon CloudWatch Logs
    - Amazon CloudWatch Events
    - AWS CodeCommit
    - AWS Config
    - AWS IoT
    - AWS IoT Events
    - AWS CodePipeline