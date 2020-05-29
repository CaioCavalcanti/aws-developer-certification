# Amazon Kinesis
Kinesis is a platform on AWS to work with streaming data, making it easy to collect, process and analyzie real-time, streaming data. You can ingest real-time data such as video, audio, application logs, website clickstrams and IoT telemetry data for machine learning, analytics and other applications.

## Useful Links
- [AWS - Kinesis Video Streams FAQs](https://aws.amazon.com/kinesis/video-streams/faqs)
- [AWS - Kinesis Data Streams FAQs](https://aws.amazon.com/kinesis/data-streams/faqs)
- [AWS - Kinesis Firehose FAQs](https://aws.amazon.com/kinesis/data-firehose/faqs)
- [AWS - Kinesis Data Analytics FAQs](https://aws.amazon.com/kinesis/data-analytics/faqs)
- [YouTube - AWS re:Invent 2017: Real-Time Streaming Applications on AWS: Use Cases and Patterns (ABD203)](https://www.youtube.com/watch?v=r06aJcTYql8)

## General Notes
- There are **three core Kinesis services**:
    - **Kinesis Streams**
        - **Video Streams** for securely stream video from connected devices to AWS for analytics and machine learning
        - **Data Streams** fot custom applications process data in real-time
        - Consists of **shards**, a sequence of data records in a stream
        - Each data record has an unique sequence number
        - Producers sends data that are distributed into shards
        - Consumers receive data from shards, process, transform and send to a final destination
        - Each shard handles **5 transactions per second for read**, up to a maximum total data read rate of 2 MB per second
        - **Up to 1,000 records per second for writes**, up to a maximum total data write rate of 1 MB per second (including partition keys)
        - The data capacity of your stream is a function of the number of shards that you specify for the stream
        - **The total capacity of the stream is the sum of the capacities of its shards**
        - If you need to scale your capacity, you'll need to reshard, adding more shard accordingly
        - Data **retained for 24 hours** by default, up to 7 days
    - **Kinesis Firehose**
        - You don't need to configure shards, it is done automatically
        - You don't need to setup consumers, just send it directly to the destination (S3, Redshift, DynamoDB, etc)
        - You can transform data with Lambda
        - There is no data retention window
        - Built on top of Kinesis Stream
    - **Kinesis Analytics**
        - Allows you to analyze data from Kinesis Streams and Firehose using SQL queries
        - Built on top of Kinesis Stream
- For this certification is important to know the key differences and general architecture of each of those services
- **Kinesis Client Library (KCL)** runs on the consumer instances
    - It tracks the number of shards in your stream
    - Discovers new shards when you reshard
    - Ensures that for every shard there is a record processor
    - Manages the number of **records processors** relative to the number of shards and consumers to **balance load among the consumers**
- When scaling out consumers:
    - You should ensure that the **number of consumers does not exceed the number of shards**, except for failure or standby purposes
    - You **never** need **multiple consumers** to handle the processing load of **one shard**
        - However, **one consumer can process multiple shards**
    - You don't need to add more consumers just because you reshard
        - What drives the number of consumers is **CPU utilisation**, not the number of shards
        - Use an auto scaling group and base scaling decisions on CPU load on your consumers