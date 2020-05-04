# Amazon Elastic Load Balancer (ELB)
Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions.

It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones. Elastic Load Balancing **offers three types of load balancers** that all feature the high availability, automatic scaling, and robust security necessary to make your applications fault tolerant.

## Useful links
- [AWS - Elastic Load Balancing FAQ](https://aws.amazon.com/elasticloadbalancing/faqs/?nc=sn&loc=6)

## Types
### Application load balancer
- Best suited for HTTP and HTTPS traffic
- Operate at individual request level (Layer 7) and are application-aware
- Can be used to create advanced request routing, sending specific requests to specific web servers
- Routes traffic to targets within VPC based on the content of the request
- Supports WebSockets and Secure WebSockets
- Request tracing is enabled by default

### Network load balancer
- Best suited for TCP traffic where **extreme performance** is required
- Operate at connection level (Layer 4)
- Is capable of handling millions of requests per second, maintaining ultra-low latencies
- Higher costs

### Classic load balancer
- Legacy, not recommended
- Can be used for both HTTP/HTTPS (Layer 7) and TCP (Layer 4)

## General Notes
- When a classic load balancer stops responding, it responds with a 504 error (Gateway timeout). This means that the app is not responsing within the idle timeout period
- You can use the header **X-Forwarded-For** to get the IPv4 address of your end user

