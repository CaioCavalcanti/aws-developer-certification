# Amazon Relational Database Service (RDS)
RDS makes it easy to setup, operate and scale relational databases on cloud, by automating administration tasks such as backup, patching and database setup.

## Useful links
- [AWS - RDS FAQ](https://aws.amazon.com/rds/faqs/)
- [AWS - Best Practices for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html)

## Supported Databases
- Microsoft SQL Server
- Oracle Database
- MySQL Server
- PostgreSQL
- Amazon Aurora
- MariaDB

## General Notes
- It's used for OLTP on SQL, there are other services dedicated to NoSQL (Dynamo) and OLAP (RedShift)
- Create an inbound rule on security group to enable other services to access the database
- There are two types of backups: 
    - **Automated Backups**:
        - It's enabled by default
        - Backup is stored in S3 and you get free storage on the size of the database
        - Allow you to recover a database to any point within a retention period
        - Retention period can be between one and 35 days
        - Takes full daily snapshots and also stores transaction logs throughout the day
        - When you do a recovery, AWS will choose the most recent daily backup and then apply the transaction logs relevant for that day
        - Backups are taken within a defined window, during this time storage I/O might be suspended while the data is backed up and you might experience elevated latency
        - Backups are deleted when the RDS instance is deleted
    - **Database snapshots**:
        - Are done manually, initiated by the user
        - Stored even after you delete the RDS instance
- When you restore a backup, either automated or snapshot, the restored version will be a new RDS instance with a new DNS endpoint
- It supports encryption at rest, using Amazon KMS. Once the RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, as are its automated backups, read replicas and snapshots
- Encrpyting an existing RDS instance is not supported. To do so, you'll need to create a snapshot, make a copy of the snapshot and encrypt the copy
- **Multi-AZ** allow you to have an exact copy of your database in another availability zone
    - AWS handles replication, synchronizing writes automatically to the secondary database
    - In the event of a planned maintenance, DB instance failure or an AZ failure, it will automatically failover to the standby database, keeping the same endpoint
    - Is meant for **disaster recovery only**, is not meant for performance improvement
- **Read Replica** allow you to have a read-only copy of your database
    - Replication is asynchronous from the primary to the read replicas
    - The read replicas should be used for read-heavy database workloads
    - It's available for:
        - MySQL Server
        - PostgreSQL
        - MariaDB
        - Aurora
    - Is used for **scaling**, not for DR
    - You must have automatic backups turned on in order to deploy a read replica
    - You can have up to 5 read replicas copies of any database
    - You can have read replicas of read replicas, with some latency
    - Each read replica will have its own DNS endpoint
    - You can have read replicas with multi-AZ enabled
    - You can created read replicas of multi-AZ source databases
    - Read replicas can be promoted to be their own database. It will break the replication
    - You can have a read replica in another region