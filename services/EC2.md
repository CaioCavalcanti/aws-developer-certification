# Amazon Elastic Compute Cloud (EC2)
Provides resizable compute capacity in the cloud, reducing the time required to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirements change.


## Useful links
- [AWS - EC2 FAQ](https://aws.amazon.com/ec2/faqs/)
- [AWS - EC2 Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-best-practices.html)

## Best Practices
- **Security**
    - Manage access using identity federation, IAM users and IAM roles
    - Establish credential management policies and procedures for creating, distributing, rotating and revoking AWS access credentials
    - Implement least permissive rules for your security group
    - Regularly patch, update and secure operating system and applications on your instance
- **Storage**
    - Understand the implications of the root device type for data persistence, backup and recovery
    - Use separate EBS volumes for the operating system versus your data and ensure the volume with your data persists after instance termination
    - Use the instance store available for your instance to store temporary data.
        - Remember that data stored in instance store is deleted when you stop or terminate the instance
    - If you use instance store for database store, ensure that you have a cluster with a replication factor that ensures fault tolerance
    - Encrypt EBS volumes and snapshots
- **Resource Management**
    - Use instance metadata and custom resource tags to track and identify your AWS resources
    - View your current limits for EC2 and plan to request any limiti increases in advance of the time you'll need them
- **Backup and recovery**
    - Regularly backup your EBS volumes using snapshots and create an Amazon Machine Image (AMI) from your instance to save the configuration as a template for launching future instances
    - Deploy critical components of your application accross multiple Availability Zones and replicate your data appropriately
    - Design your apps to handle dynamic IP addressing when your instances restarts
    - Monitor and respond to events
    - Ensure that you are prepared to handle failover
    - Regularly test the process of recovering your instances and EBS volumes if they fail

## Pricing options
### On Demand
- Pay a fixed rate by hour or minute as you go, with no commitment

#### Recommended for
- Users that want EC2 with low costs and flexibility without any up-front payment or long-term commitment
- Apps with short term, spiky or unpredictable workloads that cannot be interrupted
- Apps being developed or tested on EC2 for the first time

### Reserved
- Provides a reserved capacity with a lower price on the hourly rate for an instance, but requires a 1/3 year(s) commitment
- You can make up-front payments to reduce even more the total costs
- **Standard**: up to 75% off on-demand
- **Convrertible**: up to 54% off on-demand, enable you to change the attributes of the instance as long as the exchange results in RIs of equal or greater value
- **Scheduled**: enable to launch instances on a time window to match a predictable recurring schedule, requiring a fraction of a day, week or month

#### Recommended for
- Apps with steady state or predictable usage
- Apps that require reserver capacity
 
### Spot
- Enables you to bid the price you want for an instance capacity, providing lower costs for applications that have flexible start and end times
- If a spot instance is terminated by EC2, you won't be charged for partial hour usage. However, if you terminate the instance yourself, you will be charged for the complete hour in which the instance ran

### Recommended for
- Apps that have a **flexible start and end times**
- Apps that are only feasible at a very low compute prices
- Users with an urgent need for large ammounts of additional computing capacity

### Dedicated hosts
- Physical server dedicated for your use, can help you reduce costs by allowing you to bring your own server-bound licenses (i.e. VMWare)

#### Recommended for
- **Regulatory requirements** that may not support multi-tenant virtualization
- Licensing that does not support multi-tenancy or cloud deployments
- Can be purchased on-demand (hourly rate) or as reservation (up to 70%)

## Instance types
|Family|Speciality|Use case|
|--|--|--|
|F1|Field programmable gate array|Genomics research, financial analysis, real-time video processing, big data|
|I3|High speed storage|NoSQL DBs, Data Warehousing|
|G3|Graphics intensive|Video encoding, 3D application streaming|
|H1|High disk throughput|MapReduce-based workloads, distributed file systems such as HDFS and MapR-FS|
|T2|Lowest cos, general purpose|Web servers, small DBs|
|D2|Dense storage|Fileservers, data warehousing, hadoop|
|R4|Memory optimized|Memory intensive apps/DBs|
|M5|General purpose|Application servers|
|C5|Compute optimized|CPU intensive apps/DBs|
|P3|Graphics/General purpose GPU|Machine learning, bitcoin mining|
|X1|Memory optimized|SAP HANA, Apache Spark, etc|

## General Notes
- You can attach and detach roles to running EC2 instances without stopping or terminating those instances
- There's a limit of on-demand instances based on vCPUs used per region
- EC2 restricts e-mail traffic over port 25 by default. You can still send e-mails using a different port or using Amazon Simple Email Service (SES)