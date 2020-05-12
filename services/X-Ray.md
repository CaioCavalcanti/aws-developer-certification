# AWS X-Ray
X-Ray is a service that collects data about requests that your application serves, and provides tools you can use to view, filter and gain insights into that data to identify issues and opportunities for optimization.

For any traced request to your application, you can see detailed information not only about the request and response, but also about calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs.

## Useful Links
- [AWS - X-Ray FAQs](https://aws.amazon.com/xray/faqs/)
- [YouTube - AWS re:Invent 2017: Deep-Dive for AWS X-Ray](https://www.youtube.com/watch?time_continue=6&v=QNlkAClqYiY)

## General Notes
- X-Ray SDK provides:
    - Interceptors to add to your code to trace incoming HTTP requests
    - Client handlers to instrument AWS SDK clients that your application uses to call other AWS services
    - A HTTP client to use to instrument calls to other internal an external HTTP web services
- Has integration with AWS services:
    - ELB
    - Lambda
    - API Gateway
    - EC2
    - Elastic Beanstalk
- Supported languages
    - Java
    - Go
    - Node.js
    - Python
    - Ruby
    - .NET
- It always **encrypts traces and related data at rest**
    - You can configure it to use an AWS KMS Customer Master Key (CMK)
