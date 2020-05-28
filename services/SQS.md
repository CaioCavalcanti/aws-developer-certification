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
- You can use **SQS Delay Queues** to postpone delivery of new messages for a number of seconds
    - Messages sent to delay queue **remain invisible to consumers** for the duration of the delay period
    - **Default delay is 0 seconds, maximum is 15 minutes**
    - For standard queues, changing this setting doesn't affect the delay of messages already in the queue, only for new messages
    - For FIFO queues, this affects all messages already in the queue
    - Has the same goal as the visibility timeout, to make messages unavailable to consumers for a specific period of time
- The **difference between delay queues and visibility timeout** is that, for delay queues, a message is hidden *when it is first added to queue*, whereas for visibility timeouts a message is hidden *only after it is consumed from the queue*
- You should use delay queues when:
    - You are working with a large distributed application which may need to introduce a delay in processing
    - You need to apply a delay to an entire queue of messages
    - For example, adding a delay of a few seconds, to allow for updates to your sales and stock control databases before sending a notification to a customer confirming an online transaction
- For **large SQS messages**, you need to use **S3, AWS SDK for Java and Amazon SQS Extended Client Library for Java**
    - This is especially useful for storing and consuming **messages up to 2GB** in size
    - Allows you to:
        - Specify whether messages are always stored in S3 or only when the message size exceeds 256 KB
        - Send a message that references a single message object stored in an S3 bucket
        - Get and delete the corresponding message object from S3
    - You **can't manage large messages from any other tool**, such as AWS CLI, AWS/SQS Console, SQS API or any other AWS SDK