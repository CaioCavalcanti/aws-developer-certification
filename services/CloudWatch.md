# Amazon CloudWatch
CloudWatch is a **monitoring and observability service** that collects monitoring and operational data in the form of logs, metrics and events, and provides an unified view of AWS resources, applications and services that run on AWS and on-premises servers.

You can use CloudWatch to detect anomalous behavior in your environment, set alarms, visualize logs and metrics side by side, take automated actions, troubleshoot issues and discover insights to keep your apps running smoothly.

## Useful Links
- [AWS - CloudWatch FAQs](https://aws.amazon.com/cloudwatch/faqs/)

## General Notes
- Default host metrics:
    - CPU
    - Network
    - Disk
    - Status Check
    - By default monitoring is in **5 minutes intervals (standard monitoring)**, you can enable **detailed monitoring, which will turn it into 1 minute interval**
- Any other metric is considered a **custom metric**
    - For example, RAM Utilization
- CloudWatch Logs will **store your log data indefinitely by default**
    - You can change the retention for each log group at any time
- You can retrieve data using the **GetMetricStatistics API** or by using third party tools offered by AWS partners
- You can retrieve data from any terminated EC2 or ELB instance after its termination
- Metrics granularity depedsn on the AWS service, many services provide default metrics in 1 minute, but it can be 3 or 5 minutes depending on the service
    - The **minimum granularity for custom mettrics is 1 minute**
- You can create **alarms** to monitor any metric in your account
    - You need to configure the *thresholds* in which to trigger the alarm and the *actions* that should be taken if an alarm state is reached
- CloudWatch **can be used on premise**, just need to install the SSM agent and CloudWatch agent
- When you set an alarm on a high-resolution metric, you can specific a **high-resolution alarm** with a period of **10 seconds or 30 seconds**, or a regular alarm with a period of any multiple of 60 seconds
    - There is a higher charge for high-resolution alarms
- When setting up alarms, you need to inform:
    - `Period`: the length of time (expressed in seconds) to evaluate the metric to create each individual data point for a metric.
    - `Evaluation Period`: the number of the most recent data points to evaluate when determining alarm state
    - `Datapoints to Alarm`: the number of data points within the evaluation period that must be breaching to cause the alarm to go to the *ALARM* state
        - The breaching data points do not have to be consecutive, the just must all be within the last number of data points equal to evaluation period