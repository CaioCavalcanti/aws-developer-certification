# Amazon Elastic Block Store (EBS)
Storage volume for EC2 instances

## Useful links
- [AWS - EBS FAQ](https://aws.amazon.com/ebs/faqs/)

## Volume Types
- **General purpose SSD (GP2)**
    - General purpose for workloads below 10k IOPS
    - Balances price and performance
    - Ratio of 3 IOPS per GB with up to 10k IOPS and the ability to burst up to 3k IOPS for extended periods of time for voulmes at 3334 GiB and above
- **Provisioned IOPS SSD (IO1)**
    - Designed for I/O intensive applications such as large relational or NoSQL databases
    - If you need more than 10K IOPS
    - Can provision up to 20k IOPS per volume
- **Throughput Optimized HDD (ST1)**
    - Cannot be a boot volume
    - Used for big data, data warehouses, log processing
- **Cold HDD (SC1)**
    - Cannot be a boot volume
    - Lowest cost for infrequently accessed workloads
    - Used for file servers
- **Magnetic (Standard)**
    - Lowest cost per gigabyte of all EBS volume types *that is bootable*
    - Ideal for workload where data is accessed infrequently and the lowest storage cost is important
    - Previous generation