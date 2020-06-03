# Amazon Simple Queue Service (SQS)
SQS is a fully managed **message queuing service** that enables you to decouple and scale microservices, distributed systems and serverless applications.

You can use SQS to send, store and receive messages between software components at any volume, without losing messages or requiring other services to be available.

## Useful Links
- [AWS - SQS FAQs](https://aws.amazon.com/sqs/faqs/)
- [AWS - Best Practices for Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-best-practices.html)
- [AWS - Amazon SQS security best practices](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-security-best-practices.html)
- [AWS Compute Blog - New for AWS Lambda – SQS FIFO as an event source](https://aws.amazon.com/blogs/compute/new-for-aws-lambda-sqs-fifo-as-an-event-source/)
- [AWS - Amazon SQS dead-letter queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)

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
        - Limit of **300 transactions per second (TPS)** per API method (`SendMessage`, `ReceiveMessage` or `DeleteMessage`)
            - If you use batch it supports up to 3,000 transactions per second, per API method (`SendMessageBatch`, `ReceiveMessage` or `DeleteMessageBatch`)
        - Same capabilities as standard queues
        - The name of a FIFO queue **must end with the `.fifo` suffix**
- The **visibility timeout** is the amount of time that the message is invisible in the queue after a reader picks it up.
    - If the message is processed before the visibility time expires, the message is deleted
    - If the message is not processed within that time, the message will become visible again and another reader will pick it up. This could result in **the same message being delivered twice**
- The **default visibility timeout is 30 seconds** and the maximum is 12 hours
    - You can change the visibility timeout using `ChangeMessageVisibility` API
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
- On fanout notifications, when multiple SQS queues are subscribed to a single topic in SNS, **each queue will receive an identical message instantaneously**, this is useful for idependent parallel processing
    - The SNS topic needs permission to send messages to SQS, `sqs:SendMessage`
    - The queue needs to subscribe to SNS topic

## FIFO Queues
- Key terms:
    - **Message deduplication ID**
        - Is the token used for deduplication of sent messages
        - If a message with a particular message deduplication ID is sent successfully, any messages sent with the same message deduplication ID **are accepted but aren't delivered** during the **5-minute deduplication interval
        - Message deduplication applies to an entire queue, not to individual message groups
        - SQS continues to keep track of the message deduplication ID even after the message is received and deleted
    - **Message group ID**
        - It's a tag that specifies that a message belongs to a specific message group
        - Messages that belong to the same message group are **always processed one by one, in a strict order relative to the message group**
            - However, messages that belong to different message groups might be processed out of order
    - **Receive request attempt ID**
        - It's the token used for deduplication of `ReceiveMessage` calls
    - **Sequence number**
        - The large, non-consecutive number that SQS assigns to each message
- Sending messages
    - If multiple messages are sent in succession to a FIFO queue, each with a distinct message deduplication ID, SQS stores the messages and acknowledges the transmission
    - Then, each message can be received and prcessed in the **exact order in which the messages were transmitted**
    - Messages are ordered based on message group ID. If **mulitple hosts (or threads on the same host)** send messages with the same message group ID to a FIFO queue, **SQS stores the messages in the order in which they arrive after processing**.
        - To ensure SQS preserves the order in which messages are sent and received, **ensure that each producer uses an unique message group ID to send all its messages**.
    - If you don't provide a message group ID, **the action fails** 
- Receiving messages
    - You **can't request to receive messages with a specific message group ID**
    - When receiving messages from a FIFO queue with multiple message group IDs, SQS first attempts to return as many messages with the same group ID as possible. This allow other consumer to process messages with a different group ID
    - It is possible to **receive up to 10 message in a single call** using `MaxNumberOfMessages` request parameter on `ReceiveMessage`
- Exactly-once processing is achieved via deduplication, that must be configured in one of the following options:
    - Enable **content-based deduplication**, where SQS uses a SHA-256 hash to generate the message deduplication ID using the **body of the message, but not the attributes of the message**.
    - Explicitly provide the **message deduplication ID** (or view the sequence number) for the message
        - Parameter `MessageDeduplicationId`
        - If the queue has `ContentBasedDeduplication` set, your message `MessageDeduplicationId` overrides the generated one
    - If the queue doesn't have `ContentBasedDeduplication` enabled and the `MessageDeduplicationId` is not provided on the message, the action will fail

## Lambda Integration
- Lambda **polls the queue and invokes your function synchronously** with an event that contains the queue messages
- It **reads messages in batches** and **invokes your function once for each batch**
- When your function successfully processes a batch, Lambda deletes its message from the queue
- If your function is throttled, returns an error or doesn't respond, the message becomes visible again
- **All messages in a failed batch return to the queue**
    - Your function code **must be able to process the same message multiple times without side effects**
- **Scaling and processing**
    - For standard queues
        - Lambda uses **long polling** until the queue becomes available
        - When messages are available, Lambda **reads up to 5 batches and sends them to your function**
        - If messages are still available, Lambda **increase the number of processes** that are reading batches by **up to 60 more instances per minute**
        - The **maximum number of batches thhat can be processed simultaneously by an event source mapping is 1,000**
    - For FIFO queues
        - Lambda sends messages to your function in the order that it receives them
        - Lamnda **sorts the messages into groups and sends only one batch at a time for a group**
        - If the function fails, all retries are attempted on the affected messages before Lambda receives additional messages from the same group
        - Your function can scale in concurrency to the number of active message groups
- To allow your function time to process each batch of messages, **set the source queue's visibility timeout to at least 6 times the function timeout**
    - This extra time allows your Lambda to retry if your function is throttled while it's processing a previous batch
- When your function fails, Lambda leaves the message in the queue. After the visibility timeout occurs, Lambda receives the message again
    - To send messages to a second queue after a number of receives, you need to **configure a dead-letter queue on your source queue, not on the function**
    - The dead-letter queue that your configure on a function is used for the function's asynchronous invocation queue, not for event source queues
- To work with SQS, Lambda execution role needs the following **permissions to manage messages**:
    - `sqs:ReceiveMessage`
    - `sqs:DeleteMessage`
    - `sqs:GetQueueAttributes`

## Dead-letter Queues
- SQS supports dead-letter queues, which other queues (source queues) can target for messages that can't be consumed successfully
- It's useful for debugging your app or messaging system because they let you **isolate problematic messages** to determine why they can't be processed
- When you designate a queue to be a source queue, **a dead-letter queue is not created automatically**
- The **redrive policy** specifies the source queue, the dead-letter queue and the conditions under which SQS moves the messages from the former to the latter if the consumer fails to process the message a speficied number of times
- When the `ReceiveCount` for a message exceeds the `maxReceiveCount` for a queue, SQS moves the message to a dead-letter queue, with its original message ID
- Multiple queues of the same type can **target the same dead-letter queue**
- When a message is moved to a dead-letter queue, the enqueue timestamp remains unchanged
    - As a best practice, set the **retention period of a dead-letter queue to be longer** than the retention period of the source queue
- A dead letter queue must:
    - Use the same AWS Account
    - Reside in the same region as the other source queues
    - Be of the same type as the source queue

- The dead-letter queue for a FIFO queue **must also be a FIFO queue**
- When to use deade letter queue?
    - Use it when your app don't depend on ordering, together with standard queues
    - Use it to decrease the number of messages and reduce the possibility of exposing your system to **poison-pill messages** (messages that can be received but can't be processed)
    - **Don't use it** with standard queues when you want to be able to keep **retrying the transmission of a message indefinitely**. For example, waiting for a dependent process to become available
    - **Don't use it** with FIFO queues if you don't want to break the exact order of the messages or operations