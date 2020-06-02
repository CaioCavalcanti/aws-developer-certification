# AWS CodeDeploy
CodeDeploy is a fully managed deployment service that automates software deployments to a variety of compute services, such as EC2, Lambda and on-premise servers.

## Useful Links
- [AWS - CodeDeploy FAQs](https://aws.amazon.com/codedeploy/faqs)
- [AWS - CodeDeploy Deployments](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html)
- [AWS - AppSpec 'hooks' Section](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html)

## General Notes
- Deployment approaches:
    - **In-Place**
        - Application is stopped on each instance and the new release is installed
        - Known as **rolling update**
        - Reduces capacity during deployment
            - Stop instance (remove from ELB)
            - Install **new revision** on stopped instance
            - Start instance (adds back to ELB)
            - Continues to deploy to next instance
        - For **rollback** you need to **re-deploy the previous version**
        - **Not supported for Lambda**
        - Great for deploying for the first time and for dev/test environments
    - **Blue/Green**
        - New instances are provisioned and the new release is installed on the new instances
        - Blue represents the active deployment, green is the new release
        - Seamless approach, doesn't impact capacity
            - Create new instances (green)
            - Install new revision on new instances
            - Adds new instances to ELB
            - Traffic is routed away from the old instances (blue)
            - Old instances are removed from ELB and terminated
        - For **rollback** just make ELB **route traffic back to the original (blue) environment**
            - Only possible if you haven't already terminated the old instances
        - You **pay for 2 environments until you terminated the old instances**
        - **Safest option for production envrionment**
- **AppSpec** file is a configuration file that **defines the parameters** to be used during deployment
    - For EC2 and on-prem systems, only YAML is supported
    - For Lambda and ECS, both YAML and JSON are supported
    - It is used to manage each deployment as a series of lifecycle event hooks, which are defined on the event hooks
    - The event hooks and file structure depends on the target (EC2, Lambda or ECS)
    - The `appspec.yml` file **must be placed in the root of the directory of your revision**
- The content in the `resources` section of the AppSpec file varies, depending on the compute platform of your deployment
```yml
resources:
    # Lambda
    - lambda-function:
        type: "AWS::Lambda::Function"
        properties:
            # all required
            name: name-of-lambda-function-to-deploy
            alias: alias-of-lambda-function-to-deploy
            currentversion: version-of-the-lambda-function-traffic-currently-points-to
            targetversion: version-of-the-lambda-function-to-shift-traffic-to
    # ECS
    - ecs-service:
        Type: "AWS::ECS::Service"
        Properties:
            # required
            TaskDefinition: task-definition-ARN
            LoadBalancerInfo:
                ContainerName: ECS-container-name-for-your-ECS-app
                ContinerPort: port-used-by-your-ECS-application
            # optional
            PlatformVersion: ECS-service-platform-version
            NetworkConfiguration:
                AwspcConfiguration:
                    Subnets: ["ecs-subnet-1", "ecs-subnet-n"]
                    SecurityGroups: ["ecs-security-group-1", "ecs-security-group-n"]
                    AssignPublicIp: "ENABLED-or-DISABLED"
```
- You can use **CodeDeploy agent** on an EC2 instance or on-prem server
    - It is required **only if you deploy to an EC2 instance on on-prem server**, it isn't required for ECS and Lambda deployments
- CodeDeploy agent **archives revisions and log files on instances** and **cleans up these artifacts to conserve disk space**
    - Application revision deployment logs:
        - You can use `:max_revisions` option in the agent configuration file to specify the number of revisions to archive
        - The agent will also archive the log files for those revisions
        - All others are deleted, with the **exception of the log file of the last successful deployment, which is always retained**, even if the number of failed deployments exceeds the number of retained revisions
        - If no value is specified, it retains the **five most recent revisions in addition to the currently deployed revision**
    - CodeDeploy logs:
        - Log file is rotated daily at midnight (instance time)
        - Logs are **deleted after seven days**
- You can **configure CodeDeploy agent to capture wire log from S3** by setting `:log_aws_wire:` to true and specify a custom location to store the logs with `:log_dir:`
- When there is a deployment phase with **content overwrite option as "Overwrite"**, it will **delete all files from previous deployment session**
    - If this deployment fails, **to restore previous working deployment, either Manual or Automatic rollback must be enabled**
    - Files that are deleted as part of the deployment phase need to be **added manually**
- If you accidentally **deleted the deployment log file** on an instance, CodeDeploy **does not create a replacement log file**
    - To **create a new log file**, you need to **restart CodeDeployAgent**

## Lifecycle Event Hooks
- Lifecycle event hooks are run in a specific order known as **run order**
- You don't need to use all hooks, just pick the ones that make sense for your application deployment process
- For the exam is good to have a high level understanding of the logical flow and the run order
- Some events cannot be scripted, you can just specify files to reference
- **EC2**
    - In-place deployment works in 3 broadly phases
    - **Phase 1**: de-register instances from ELB
        - `BeforeBlockTraffic`: tasks you want to run on instances before they are de-registered from ELB 
        - `BlockTraffic`: de-register instances from ELB. **Cannot be scripted**.
        - `AfterBlockTraffic`: tasks you want to run on instances after they are de-registered
    - **Phase 2**: actual deployment
        - `ApplicationStop`: tasks you need to run to gracefully stop your application
        - `DownloadBundle`: CodeDeploy agent copies the application revision files to a temporary location. **Cannot be scripted**.
        - `BeforeInstall`: pre-install scripts, e.g. decrypting files, backing up current version
        - `Install`: copy application revision files to final location. **Cannot be scripted**.
        - `AfterInstall`: post-install scripts, e.g. configuration, file permissions, etc
        - `ApplicationStart`: start any services that were stopped during `ApplicationStop`
        - `ValidateService`: run tests to validate the service
    - **Phase 3**: re-register instances in ELB
        - `BeforeAllowTraffic`: tasks you want to run on instances before they are re-registered in ELB
        - `AllowTraffic`: re-register instances in ELB. **Cannot be scripted**.
        - `AfterAllowTraffic`: tasks you want to run on instances after they are re-registered to ELB
- **ECS**
    - The event hooks actions are Lambda functions
    - `BeforeInstall`
    - `Install`: **Cannot be scripted**.
    - `AfterInstall`
    - `AllowTestTraffic`: **Cannot be scripted**.
    - `AfterAllowTestTraffic`
    - `BeforeAllowTraffic`
    - `AllowTraffic`: **Cannot be scripted**.
    - `AfterAllowTraffic`
- **Lambda**
    - The event hooks actions are Lambda functions
    - `BeforeAllowTraffic`
    - `AllowTraffic`: **Cannot be scripted**.
    - `AfterAllowTraffic`
    - The function needs to use CodeDeploy SDK to get the deployment id and event hook id and return a validation test result, with either 'Succeeded' or 'Failed'
    - The deployment fails if CodeDeploy is not notified by the Lambda function within one hour