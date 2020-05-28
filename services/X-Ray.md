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
- To configure X-Ray in your system **you'll need both X-Ray SDK and X-Ray daemon**
    - Required for the services that doesn't have it built-in (i.e. EC2, ECS)
    - X-Ray SDK send the data to the X-Ray daemon, which **buffers segments in a queue and uploads them to X-Ray in batches**
    - High-level configuration for:
        - On-prem and EC2: install the X-Ray daemon on the server
        - Elastic Beanstalk: install the X-Ray daemon on the EC2 instances inside your environment
        - ECS: install X-Ray daemon on **its own Docker container on your ECS cluster** alongside your app
- You can add **annotations** when instrumenting your application to record additional information about requests
    - Key-value pairs
    - Annotations are indexed for use with filter expressions, so you can search for traces that contain specfici data and group related traces together in the console