# Amazon DynamoDB
Amazon DynamoDB is a fast and flexible **NoSQL database** service for all applications that need consistent, **single-digit millisecond latency at any scale**. It is a **fully managed database** and supports both **document and key-value** data model.

Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT and many other applications.

## Useful Links
- [AWS - DynamoDB FAQs](https://aws.amazon.com/dynamodb/faqs/)
- [AWS - Best Practices for Designing and Architecting with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
- [AWS - DynamoDB Data Modeling: Key Principles, Practical Patterns](https://aws.amazon.com/dynamodb/resources/reinvent-2019-data-modeling)

## General Notes
- Is a serverless service
- Data is always stored on SSD storage and **spread across 3 geographically distinct locations**
- There are **2 consistency models**:
    - **Eventual consistent reads (default)**
        - Consistency across all copies of data is usually reached **within a second**
        - **Best read performance**
    - **Strongly consistent reads**
        - Returns a result that reflects all writes that received a successful response prior to the read
- Supports **documents** and **key-value** data
- Documents can be written in **JSON, HTML or XML**
- DynamoDB stores and retrieves data based on a Primary Key, which can be of two types:
    - **Partition Key**
        - Unique attribute (e.g. user id)
        - The value of the partition key is input to an internal hash function which determines the partition or physical location on which the data is stored (**used for sharding**)
        - If you are using the partition key as your primary key, then no two items can have the same partition key
    - **Composite Key**
        - **Partition Key + Sort Key** (e.g. same user posting multiple times to a forum)
        - Two items may have the same partition key, but the must have a different sort key
        - All items with the same partition key are stored together, then sorted according to the sort key value
        - Allows you to **store multiple items with the same partition key**
- Authentication and Access Control is managed using **IAM Policies**
- You can create an IAM user with specific permissions to access and create DynamoDB tables
- You can create an IAM role to enable temporary access keys to access DynamoDB
- You can use special **Condition on an IAM Policy to restric user access to only their own records**
    - Use condition parameter `dynamodb:LeadingKeys` on IAM Policy
- There are two types of **index** that are supported to help speed-up your queries:
    - **Local Secondary Index**
        - Can only be created when you are creating your table
        - You cannot add, remove or modify it later
        - It has the same partition key as your original table, but a different sort key
        - Useful when you need an **alternative sort key**
    - **Global Secondary Index**
        - Can be created at any time
        - Has a different partition and sort key
        - Useful to speed up queries on **non-key attributes**
- **Provisioned throughput** is the mechanism used to define the capacity of the performance requirements in DynamoDB
    - It is measured in **capacity units**
    - You can specify requirements in terms of read and write capacity units, where:
        - **1 Write Capacity Unit = 1x 1KB Write per second**
        - **1 Read Capacity Unit =**
            - **1x Strongly Consistent Read of 4KB per second**
            - OR
            - **2x Eventually Consistent Reads of 4KB per second** (**default**)
    - The more capacity units you provision, the more you pay
    - When your request rate is higher than the reat/write capacity povisioned on your table, you'll get a `ProvisionedThroughputExceededException`
        - **AWS SDK will automatically retry** the requests until it succeeds, using **jitter based bacoff algorithm**
        - If you are not using the SDK you can reduce the request frequency or use an **exponential backoff**, by using progressively longer waits
- You also have an option to use **On-Demand Capacity**
    - Charges apply for reading, writing and storing data, and you pay for only what you use (per request)
    - You don't need to specify your requirements, as DynamoDB instantly scales up and down based on the activity of your app
    - **Great for unpredicatable workloads**
- **DynamoDB Accelerator (DAX)** is a fully managed, clustered **in-memory cache for DynamoDB**
    - Up to 10x **read performance improvement**
    - Ideal for read-heavy and bursty workloads
    - Write-through caching service: data is written to the cache at the same time as the backend
    - You application should point to DAX cluster instead of the table, if the item is not cache, DAX performs an **eventually consistent** GetItem operation
    - May be able to reduce provisioned read capacity
    - Does not support lazy loading
- DynamoDB **transactions** are used to ensure ACID transactions (atomic, consistent, isolated and durable) on mission-critical applications
    - Read or write multiple items acoress multiple tables as an all or nothing operation
    - Check for a pre-requisite condition before writing to a table
- **TTL** defines an expire time for your data
    - Expired items are marked for deletion and will be removed in the next 48h
    - Great for removing irrelevant or old data, such as:
        - Session data
        - Event logs
        - Temporary data
    - Reduces costs by automatically removing data which is no longer relevant
    - Is expressed in epoch/unix timestamp
    - You can choose any column name, as long as it is a timestamp
- **DynamoDB Stream** is a time-ordered sequence of item level modifications (insert, update, delete)
    - The modifications are **recorded as logs**, which are **encrypted at rest** and **stored for 24 hours**
    - Can be used for auditing, replaying transactions in different tables, and triggering events based on changes, such as a lambda function
    - Accessed using a dedicated endpoint
    - The **primary key is recorded by default**, but you can capture before and after images
    - Events are recorded in near real-time
    - Applications can take actions based on contents
    - **Lambda polls DynamoDB stream**, and can execute code based on the stream event


## Query vs Scan
- A **query** operation:
    - Find items in a table **using only the primary key** attribute
    - You can use the **sort key** to refine the results
    - Results are always sorted by the sort key, and you can **reverse the order** by setting the parameter **ScanIndexForward** to false
    - Is eventually consistent by default, but you can explicitly set to be strongly consistent
- A **scan** operation:
    - Examines every item** in the table
- Both return all data attributes by default, but you can use **ProjectionExpression** parameter to return only the attributes you need
- **Query is more efficient** than a Scan, since scan dumps the entire table and filters out the result
    - In a large table, a scan operation can use up the provisioned throughput in just a single operation
- Improving performance:
    - Return items on **small page sizes**, for both query and scan
    - **Avoid using scan operations** if you can: design tables in a way that you can use Query, Get or BatchGetItem
    - By default, a scan operation processes data sequentially, in batches of 1MB. Use **parallel scans** instead by logically dividing a table or index into segments and scanning each segment in parallel
        - **Avoid parallel scans** if your table or index is already incurring heavy read/write activity from other applications
- **BatchGetItem** allows you to pass **multiple Partition Key values in a single request**
    - **Query does not support this**