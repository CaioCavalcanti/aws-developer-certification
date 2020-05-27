# AWS Serverless Application Model (SAM)
AWS SAM is an open-source framework that you can use to build serverless applications on AWS.

## Useful Links
- [AWS - SAM FAQs](https://aws.amazon.com/serverless/sam/faqs/)
- [AWS - AWS SAM CLI Command Reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-command-reference.html)
- [AWS - AWS SAM Specification](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification.html)
- [GitHub - awslabs/serverless-application-model](https://github.com/awslabs/serverless-application-model)
- [GitHub - aws-samples/serverless-app-samples](https://github.com/aws-samples/serverless-app-examples)

## General Notes
- Is an extensions to CloudFormation used to define serverless applications
- You can use SAM CLI to:
    - locally build, debug and test SAM applications on a Lambda-like execution environment
    - package and deploy your code to AWS
- SAM template is **compatible with CloudFormation template**
    - It requires a line with `Transform: AWS::Serverless-2016-10-31`, which tells CloudFormation that this is a SAM template
- You can integrate with CodePipeline
- Is installed on CodeBuild hosts by default, no need to run install cmd
- `sam package` will create the CloudFormation template and upload it to S3 bucket
- `sam deploy` will read the packaged template and create the CloudFormation stack
    - It handles change sets automatically