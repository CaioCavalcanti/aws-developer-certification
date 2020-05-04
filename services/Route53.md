# Route 53
It's Amazon's DNS service, allows you to map your domain names to EC2 instances, load balancers and S3 buckets.

## Useful links
- [AWS - Route 53 FAQ](https://aws.amazon.com/route53/faqs/)

## General Notes
- You can use it to create and manage domain names
- Can be used to monitor health and performance of your application, web servers and other resources
- You can use **Weighted Round Robin (WRR)** to do A/B testing
- **Latency Base Routing (LBR)** can be used to improve an app performance for a global audience, by routing end users to the AWS region that provides the lowest latency
    - You need to create a new record set as LBR-enabled
- **Geo DNS** lets you balance load by directing requests to specific endpoints based on the geographic location from which the request originates. This can be used to present localized content, showing the right language or restricting access to specific markets
    - Provides three levels of granularity: continent, country and state
        - You can overlap regions, Route 53 will return the most specific Geo DNS record that includes that location (state > country > continent > global)
    - Can be combined with other routing types, such as LBR, to enable low-latency and fault-tolerant architecture
    - It is recommended to create a global record to ensure Route 53 can provide a response from all possible locations
- The main difference between LBR and Geo DNS is that Geo DNS bases routing decisions on the geographic location of the requests, which is a good proxy for latency in some cases, but not always.
    - Use **LBR** when the goal is to **minimize end-user latency**
    - Use **Geo DNS** when you have **compliance, locatization requirements** or other use cases that require stable routing from a specific geography to a specific endpoint
- **Traffic Flow** can improve the performance and availability of your app by connecting users to the best endpoint based on latency, geography and endpoint health
    - You can configure policies to route traffic based on constraints that you care most about
    - Provides a visual editor
    - You can create traffic policy versions to easily roll out and roll back configuration changes (i.e. A/B testing)
- You can use Route 53 to configure **Private DNS**, allowing you to use authoritative DNS within your VPC without exposing your DNS records
- **Health Checks** are automated requests sent over the internet to your app to verify that your application is reachable, available and functional.
    - By default, an endpoint is considered "unhealthy" after failing three consecutive observations. The health check will continue trying and will resume sending traffic once it passes three consecutive observations
    - This threshold can be change to any value between 1 and 10
    - The interval of the observations is 30s by default and can be changed to 10s
    - Each health check is conducted independently from multiple locations around the world, the number and set of locations is configurable
- **DNS Failover** uses the results of the health checks to route users to healthy endpoints
    - You can configure DNS Failover for ELBs, Route 53 creates and manages the health checks for the ELB automatically
    - It can only failover to healthy endpoints, if there's no healthy endpoint remaning, Route 53 will behave as if all health checks are passing
    - You can also **failover based on metrics from CloudWatch**, such as CPU load, network and memory. In this case, the health check becomes unhealthy whenever its associated CloudWatch metrics enters an alarm state
        - This can be used to failover endpoints that cannot be reached by the standard Route 53 Health Check (from internet), such as private IP adresses or instances within a VPC