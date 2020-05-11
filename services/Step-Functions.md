# AWS Step Functions
Step Functions lets you orchestrate multiple AWS services into serverless workflows so you can build and update apps quickly.

## Useful Links
- [AWS - Step Functions FAQs](https://aws.amazon.com/step-functions/faqs/)
- [AWS - Best Practices for Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/sfn-best-practices.html)
- [AWS - Standard vs Express Workflows](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-standard-vs-express.html)
- [Amazon States Language](https://states-language.net/spec.html)
- [Serverless.com - AWS Step Functions - The Ultimate Guide](https://www.serverless.com/aws-step-functions)

## Best Practices
- Use Timeouts to avoid stuck executions
- Use ARNs instead of passing large payloads
- Avoid reaching the history quota
- Handle Lambda service exceptions
- Avoid latency when polling for activity tasks
- Choose the right workflow type for your application, Standard or Express

## General Notes
- Allows you to visualize and test your serverless applications
- Provides a graphical console to arrange and visualize the components of your application as series of steps, making it simple to build and run multistep applications
- Automatically triggers and tracks each step, retrying when there are errors, so your application executes in order and as expected
- Logs the state of each step, so when things do go wrong, you can diagnose and debug problems quickly
- Workflows are defined using **JSON-based Amazon States Language**
- Common use cases:
    - **Data processing**: consolidating data from multiple sources into unified reports, refining and reducing large data sets into useful formats, or coordinating multi-step analytics and machine learning workflows
    - **DevOps and IT automation**: build tools for CI/CD or even-driven applications that automatically respond to changes in infrastructure
    - **E-commerce**: automating mission-cricital business process, such as order fulfillment and invetory tracking
    - **Web apps**: implementing robust user registration processes and sign-on authentication
- There are two workflow types
    - **Standard workflows**: default type, should be used when you need long-running, durable and auditable workflows
    - **Express workflows**: should be used for high-volume, event processing workloads
- Your state machine executions will behave differently depending on which type you select
    - The type cannot be changed after the state machine has been created