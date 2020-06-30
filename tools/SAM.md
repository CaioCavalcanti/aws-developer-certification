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
- You can deploy **nested applications** from S3 buckets or Serverless Application Repository using `AWS::Serverless::Application`

## Lambda Deployments
- When you create Lambda functions with SAM it comes buil-int with CodeDeploy to provide gradual deployments, that will automatically:
    - Deploy new versions of your Lambda function and automatically create aliases that point to the new version
    - Gradually shift traffic to the new version, until you confirm it's working as expected or you roll back the update
    - Define pre-traffic and post-traffic test functions to verify that the newly deployed code is configured correctly and your application operates as expected
    - Roll back the deployment if CloudWatch alarms are triggered
- If you enable gradual deployment through SAM template, a **CodeDeploy resource is automatically created for you**
- Template example for gradual deployment
```yml
Resources:
    MyLambdaFunction:
        Type: AWS::Serverless::Function
        Properties:
            Handler: index.handler
            Runtime: nodejs4.3
            CodeUri: s3://bucket/code.zip
            
            # Name of the alias that will point to the new version deployed
            AutoPublishAlias: live
            DeploymentPreference:
                Type: Canary10Percent10Minutes 
                Alarms:
                    # A list of alarms that you want to monitor
                    - !Ref AliasErrorMetricGreaterThanZeroAlarm
                    - !Ref LatestVersionErrorMetricGreaterThanZeroAlarm
                Hooks:
                    # Validation Lambda functions that are run before & after traffic shifting
                    PreTraffic: !Ref PreTrafficLambdaFunction
                    PostTraffic: !Ref PostTrafficLambdaFunction
```
- `AutoPublishAlias`: by providing an alias name, SAM:
    - Detects when new code is deployed, based on changes to the function S3 URI
    - Creates and publishes an updated version of that function with the latest code
    - Creates an alias with a name that your provide (unless an alias already exists) and points to the updated version
- `DeploymentPreference`
    - `Type`: 
        - `Canary`: traffic is shiffted in **two increments**, you can choose from predefined options that specify the percentage of traffic that's shiffted to the updated version in the first increment and the interval before the traffic is shiffted in the second increment:
            - `Canary10Percent30Minutes`
            - `Canary10Percent5Minutes`
            - `Canary10Percent10Minutes`
            - `Canary10Percent15Minutes`
        - `Linear`: traffic is shifted in **equal increments with equal number of minutes between each increment**, you can choose from predefined linear options that specify the percentage of traffic that's shifted in each increment and the number of minutes between each increment
            - `Linear10PercentEvery10Minutes`
            - `Linear10PercentEvery1Minute`
            - `Linear10PercentEvery2Minutes`
            - `Linear10PercentEvery3Minutes`
        - `AllAtOnce`: all traffic is shifted to the new function version at once
    - `Alarms`: list of CloudWatch alarms that are triggered by any errors raised by the deployment
        - They automatically roll back your deployment when triggered
    - `Hooks`
        - `PreTraffic`:
            - Before traffic shifting starts, CodeDeploy invokes this function
            - The function **must call back to CodeDeploy and indicate success or failure**
            - If the function fails, it aborts and reports a failure back to CloudFormation, otherwise it proceeds to traffic shifting
        - `PostTraffic`
            - CodeDeploy will invoke this function after traffic shifting completes
            - Same as `PreTraffic` function, it needs to report success of failure back to CodeDeploy
            - Use this function to run **integration tests or other validation actions**