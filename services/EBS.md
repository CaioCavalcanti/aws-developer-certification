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

## General Notes
- EBS volume can persist independently of the life of the EC2 instance, different from the local instance store, which persists only as long as the instance is alive
- If you are using an EBS volume as root partition, set "Delete on termination" to no if you want to persist the data out of the EC2 instance lifetime
- Snapshots can be taken in real time while the volume is attached and in use. However, snapshots only capture data that has been written to the volume, which might exclude data that has been lolcally cached by your app or OS
    - To ensure a consistent snapshot, it's recommended to detach the volume or shutdown the instance if it's a root volume
- The time taken to create a snapshot depends on several factors, including the amount of data that has changed since the last snapshot
- Each snapshot is given an unique identifier, so customer can do a point-in-time recovery
- Snapshots can be shared or private
- **Fast Snapshot Restore (FSR)** improves performance whenever you need to restore data from a snapshot, it is intended for VDI, test/dev volume copies and booting from custom AMIs.
    - It does not impact snapshot creation time, only restore
    - There is a limit on the number of volumes that can be created with immediate full performance
- EBS enables encryption at rest using Amazon-managed keys or your own keys using Amazon KMS