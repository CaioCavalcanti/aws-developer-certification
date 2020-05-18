# Amazon API Gateway
Is a fully managed service that makes it easy for developers to publish, maintain and secure APIs in any scale.

## Useful Links
- [AWS - API Gateway FAQs](https://aws.amazon.com/api-gateway/faqs/)
- [AWS - Security best practices in Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/security-best-practices.html)
- [AWS Architecture Blog - Things to Consider When You Build REST APIs with Amazon API Gateway](https://aws.amazon.com/blogs/architecture/things-to-consider-when-you-build-rest-apis-with-amazon-api-gateway/)

## General Notes
- Exposes HTTPS endpoints to define a RESTful API
- Serverlessly connect to services like Lambda & DynamoDB
- Sends each API endpoint to a different target
- Run efficiently with **low cost**
- **Scale** effortlessly
- Track and control usage by API key
- **Throttle requests** to prevent attacks
- Can connect to CloudWatch to **log all requests** for monitoring
- Maintain **multiple versions** of your API and create **different stages**
- Enable **request and/or response transformation**
- Comes with API Gateway default domain, but you can use **custom domain** as well
- Supports **AWS Certificate Manager** for SSL/TLS certificates
- You can enable **API Caching** to cache your endpoint's response to **reduce the number of calls** made to your endpoint and also **improve latency**
- When using JS/AJAX that uses multiple domains with API Gateway, ensure that **CORS is enabled**
- You can use **API Gateway Import API** feature to import an API from an external definition file into API Gateway.
    - Supports Swagger v2.0 definition files
- **API Throttling**
    - Limits the steady-state request rate to **10,000 requests per second by default**
    - The **maximum concurrent requests is 5,000** across all APIs within an AWS account
    - If you go over the limit you will receive a **429 Too Many Requests** error response
    - You can throttle using individual API keys, limiting individual clients
- You can use API Gateway as a **SOAP web service passthrough**