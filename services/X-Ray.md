# AWS X-Ray
X-Ray is a service that collects data about requests that your application serves, and provides tools you can use to view, filter and gain insights into that data to identify issues and opportunities for optimization.

For any traced request to your application, you can see detailed information not only about the request and response, but also about calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs.

## Useful Links
- [AWS - X-Ray FAQs](https://aws.amazon.com/xray/faqs/)
- [YouTube - AWS re:Invent 2017: Deep-Dive for AWS X-Ray](https://www.youtube.com/watch?time_continue=6&v=QNlkAClqYiY)

## General Notes
- X-Ray SDK provides:
    - Interceptors to add to your code to trace incoming HTTP requests
    - Client handlers to instrument AWS SDK clients that your application uses to call other AWS services
    - A HTTP client to use to instrument calls to other internal an external HTTP web services
- Has integration with AWS services:
    - ELB
    - Lambda
    - API Gateway
    - EC2
    - Elastic Beanstalk
    - SQS
- Supported languages
    - Java
    - Go
    - Node.js
    - Python
    - Ruby
    - .NET
- It always **encrypts traces and related data at rest**
    - You can configure it to use an AWS KMS Customer Master Key (CMK)
- To configure X-Ray in your system **you'll need both X-Ray SDK and X-Ray daemon**
    - Required for the services that doesn't have it built-in (i.e. EC2, ECS)
    - X-Ray SDK send the data to the X-Ray daemon, which **buffers segments in a queue and uploads them to X-Ray in batches**
    - High-level configuration for:
        - On-prem and EC2: install the X-Ray daemon on the server
        - Elastic Beanstalk: install the X-Ray daemon on the EC2 instances inside your environment
        - ECS: install X-Ray daemon on **its own Docker container on your ECS cluster** alongside your app
    - By default, X-Ray daemon listen for traffic on **UDP port 2000**
        - This port can be changed while configuring daemon with CLI option `--bind {port}`
- You can add **annotations** when instrumenting your application to record additional information about requests
    - Key-value pairs
    - Annotations are indexed for use with filter expressions, so you can search for traces that contain specific data and group related traces together in the console
- **Service maps** can be used to create dependency trees between multiple services **across multiple regions** and also detect latency between these services which can help resolve performance issues
- When Lambda function is **invoked by non-instrumented services**, tracing can be enabled by **enable active tracing** under the function on the console or by CLI
- You can **retrieve data** from X-Ray directly from API with AWS CLI
    - `GetServiceGraph`
    - `GetTraceSummaries`
        - You can use two `TimeRangeType` when calling GetTraceSummaries:
            - TraceId: the default `GetTraceSummaries` search uses TraceID time and returns traces started within the computed `[start_time, end_time]` range
            - Event time: used to search for events as they happen over the time using timestamps. Returns traces active during `[start_time, end_time]` range, regardless of when the trace began
- For non-supported platforms, X-Ray daemon can be installed with AWS Beanstalk by downloading it from S3 bucket
- When X-Ray is integrated with SQS, there is no impact of trace header on SQS message size or message attribute quota


## Key concepts
- **Segments**
    - Services send data about their work as segments
    - A segment provides the resource's name, details about the request and details about the work done
    - Consists of tracing records for a request which a distributed application makes, and contains multiple system defined and user defined annotations, along with sub-segments consisting of remote calls made from this app

- **Segment documents**
    - Segment documents can be up to 64 kb in size
    - You can send segment documents directly to X-Ray using the `PutTraceSegments` API
    - Required fields in a segment document:
        - name
        - id
        - trace_id
        - start_time
        - end_time
    - Options fields:
        - service
        - user
        - origin
        - parent_id
        - http
        - aws
        - error, throttle, fault and cause
        - annotations
        - metadata
        - subsegments

- **Subsegments**
    - Provide more granular timing information and details about downstream calls that your app made to fulfill the original request
    - You can define arbitrary subsegments to instrument specific functions or lines of code in your app
    - For services that don't send their own segments, like DynamoDB, X-Ray uses subsegments to generate **inferred segments** and downstream nodes on the service map
        - This let you see all your downstream dependencies, even if they don't support tracing or are external
    - Sub-segments can also be sent independently apart from embedding in segment documents

- **Service graph**
    - A service graph is a JSON document that contains information about the services and resources that make up your app
    - The X-Ray console uses it to generate a visualization or a **service map**
    - X-Ray uses the data that your app sends to generate a service graph
    - Each resource that sends data to X-Ray appears as a service in the graph
    - **Edges** connect the services that work together to serve requests, connecting clients to your app and your app to the downstream services and resources that it uses

- **Traces**
    - A **trace ID** tracks the path of a request through your app
    - A trace collects all the segments generated by a single request
    - The first supported service that the request interacts with adds a **trace ID header to the request** and propagates it downstream to track the latency, disposition and other request data
    - Is a set of data point sharing the same trace id

- **Sampling**
    - X-Ray SDK applies a sampling algorithm to determine which requests get traced
    - **By default, X-Ray records the first request each second and five percent of any additional requests**
    - You can modify the default sampling rule and configure additional rules that apply sampling based on properties of the service or request
        - For example, tracing all calls that modify state or handle user accounts

- **Tracing header**
    - All requests are traced, up to a configurable minimum. After reaching the minimum, a percentage of requests are traced to avoid unnecessary cost
    - The sampling decision and trace ID are added to HTTP requests in **tracing headers** named `X-Amzn-Trace-Id`
        - `X-Amzn-Trace-Id: Root=1-5759e988-bd862e3fe1be46a994272793;Sampled=1`
    - A tracing header can originate from the X-Ray SDK, an AWS service or the client request.
        - Your app can remove the header from incoming requests to avoid issues caused by users adding trace IDs or sampling decisions to their requests
    - It can also contain a **parent segment ID** if the request originated from an instrumented app
        - `X-Amzn-Trace-Id: Root=1-5759e988-bd862e3fe1be46a994272793;Parent=53995c3f42cd8ad8;Sampled=1`

- **Filter expressions**
    - X-Ray console provides an easy-to-navigate view of the service graph, where you can drill down to traces for individual requests or use filter expressions to fin traces related to specific paths or users

- **Groups**
    - You can use a filter expression to define a criteria by which to accept traces into a group
    - You can call the group by name of by ARN to generate its own service graph, trace summaries and CloudWatch metrics
    - Once a group is created, incoming traces are checked against the group's filter expression as they are stored
    - Metrics for the numbers of traces matching each criteria are published to CloudWatch every minute
    - Updating a group's filter expression doesn't change data that's already recorded, the update applies only to **subsequent traces**
        - This can result in a merged graph of the new and old expression, to avoid this, delete the current group and create a fresh one
    - Groups are billed by the number of retrieved traces that match the filter expression

- **Annotations and metadata**
    - Annotations and metadata are aggregated at the trace level, and can be added to any segment or subsegment
    - Annotations are simple key-value pairs that **are indexed for use with filter expressions**
    - Use annotations to record data that you want to **use to group traces in the console** or when calling the `GetTraceSummaries` API
    - X-Ray indexes up to 50 annotations per trace
    - Metadata are key-value pairs with values of any type, including objects and lists, but **are not indexed**
    - Use metadata to record data you want to **store in the trace, but don't need to use for searching traces**

- **Errors, faults and exceptions**
    - X-Rays tracks errors that occur in your app code and errors that are returned by downstream services
    - Can be categorized as:
        - Error: **client errors (400 series errors)**
        - Fault: **server faults (500 series errors)**
        - Throttle: **throttling errors (429 too many requests)**
    - When an exception occurs while your application is serving an instrumented requests, the SDK records details about the exception, including the stack trace, if availabile