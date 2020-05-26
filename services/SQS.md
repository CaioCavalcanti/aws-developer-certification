# Amazon Simple Queue Service (SQS)
SQS is a fully managed **message queuing service** that enables you to decouple and scale microservices, distributed systems and serverless applications.

You can use SQS to send, store and receive messages between software components at any volume, without losing messages or requiring other services to be available.

## Useful Links
- [AWS - SQS FAQs](https://aws.amazon.com/sqs/faqs/)
- [AWS - Best Practices for Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-best-practices.html)
- [AWS - Amazon SQS security best practices](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-security-best-practices.html)
- [Building Scalable, Reliable Amazon EC2 Applications with Amazon SQS whitepaper](http://sqs-public-images.s3.amazonaws.com/Building_Scalabale_EC2_applications_with_SQS2.pdf)


## General Notes
- Was the first AWS service public available
- Is **pull-based**, if you need a pushed-based use SNS instead
- Messages can contain **up to 256KB of text in any format**
- Messages can be kept in the queue from 1 minute to 14 days, **default retention period is 4 days**
- Offers two types of queues:
    - **SQS Standard** (default)
        - Nearly-unlimited number of transactions per second
        - Delivery guarantee is **at least once**
        - Provides **best-effort ordering** to ensure messages are generally delivered in the same order as they are sent
    - **SQS FIFO**
        - First-in-first-out with **exactly-once** delivery
        - Messages order are **strictly preserved**
        - Messages are delivered and **remain available until a consumer processes and deletes it**
        - Supports **message groups** that allow multiple ordered message groups within a single queue
        - Limit of **300 transactions per second (TPS)**
        - Same capabilities as standard queues
- The **visibility timeout** is the amount of time that the message is invisible in the queue after a reader picks it up.
    - If the message is processed before the visibility time expires, the message is deleted
    - If the message is not processed within that time, the message will become visible again and another reader will pick it up. This could result in **the same message being delivered twice**
- The **default visibility timeout is 30 seconds** and the maximum is 12 hours
- SQS provides **short polling** and **long polling** to receive messages from a queue:
    - **Short polling**
        - Query only a subset of the servers to find messages available
        - **Responds right away, even if it has no messages**
        - Is used by default
    - **Long polling**
        - Query all servers to find messages available
        - Responds only after collecting at least one message, up the maximum number of messages specified in the request
        - Sends an **empty response only if the polling wait time expires**
        - Can save money
- **Long polling** is a way to retrieve messages from your SQS queues