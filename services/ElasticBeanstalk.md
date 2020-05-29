# AWS Elastic Beanstalk
Elastic Beanstalk is a service for deploying and scaling web applications developed in many popular languages, such as Java, .NET, PHP, Node.js, Python, Ruby, Go and Docker onto server platforms like Apache Tomcat, Nginx, Passenger and IIS.

Developers can focus on writing code and don't need to worry about any of the underlying infrastructure needed to run the application.

## Useful Links
- [AWS - Elastic Beanstalk FAQs](https://aws.amazon.com/elasticbeanstalk/faqs/)
- [AWS - Security best practices for Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/security-best-practices.html) 

## General Notes
- Automatically scales your application up and down
- You can select the EC2 instance type that is optimal for your application
- **Managed Platform Updates feature** automatically applies updates to the operating system, Java, PHP, Node.js, etc
    - It's enabled by default for some environment types, such as PHP
- You can monitor application health via a dashboard and integrate with CloudWatch and X-Ray for performance data and metrics
- You upload your code and Elastic Beanstalk will handle deployment, capacity provisioning, load balancing, auto-scaling and application health
- You retain full control of the underlying AWS resources powering your application and you pay only for the AWS resources required to store and run your application, such as EC2 instances and S3 buckets
- You can fully manage your EC2 instances or let Elastic Beanstalk take full administrative control
- You can **customize your environment using configuration files**, defining packages to install, run shell commands, specify services to enable, etc
    - Written in YAML or JSON format
    - Files must have a `.config` extension
    - Must be saved inside a folder called `.ebextensions`
    - `.ebextensions` folder must be included in the top-level directory of your application source code bundle
- Supports two ways of **integrating an RDS database with your Beanstalk** environment:
    - Launch the **RDS instance from within your Beanstalk environment**
        - Good option for **dev and test** deployments
        - Not ideal for production environments, since the database will be terminated when you terminate the environment
    - Launch it **directly from RDS, decoupled from your Beanstalk environment**
        - Preferred for production environments
        - Gives more flexibility, allowing you to connect multiple environments to the same database and choose a wider range of database providers
        - Allow you to tear down your application environment without affecting the database instance
        - **Requires an additional security group** must be added to the environment's Auto Scaling group
        - **You need to provide connection string configuration** using environment properties
- Supports deployment of **Docker containers**
    - **Single container**: runs the Docker container on an EC2 instance provisioned by Elastic Beanstalk
    - **Multiple containers**: Elastic Beanstalk will build an ECS cluster and deploy multiple Docker containers on each instance
- During environment creation, configuration options are applied from multiple sources with the following precedence, from highest to lowest:
    - Settings applied directly to the environment: settings specified during create or update environment operation from the API by any client, AWS Console, EB CLI, AWS CLI and SDKs
        - AWS Console and EB CLI also apply [recommended values](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html#configuration-options-recommendedvalues) for some options that apply at this level, unless overridden
    - Saved configurations: settings from any options that are not applied directly to the environment are loaded from saved configuration (available in S3 for example), if specified
    - Configuration files: settings that are not applied directly in the environment and also not specified in a saved configuration, are loaded from configuration files available in the  `.ebextensions` directory at the root of the application source bundle
        - Configuration files are **executed in alphabetical order**
    - Default values: if a configuration option has a default value, it only applies when the option is not set at any of the above steps
- **Settings in configuration files are not applied directly** to the environment and **cannot be removed without modifying the configuration files and deploying a new application version**.

## Deployment policies
- **All at once**
    - Deploys the new version to all instances simultaneously
    - All instances are out of service while the deployment takes place
    - You will experience an **outage while the deployment takes place**
    - If the update fails, you need to roll back the changes by re-deploying the original version to all instances
    - **Not recommended for mission-critical production systems**
- **Rolling**
    - Deploys the new version in batches
    - Each batch of instances is taken out of service while the deployment takes place
    - Your **environment capacity will be reduced** by the number of instances in a btach while the deployment takes place
    - **Not ideal for performance sensitive systems**
    - If the update fails, you need to perform an additional rolling update to roll back the changes
- **Rolling with additional batch**
    - Launches an additional batch of instances
    - Deploys the new version in batches
    - **Maintains full capacity** during the deployment process
    - If the update fails, you need to perform an additional rolling update to roll back the changes
- **Immutable**
    - Deploys the new version to a fresh group of instances in their own new autoscaling group
    - When the new instances pass their health checks, they are moved to your existing auto scaling group and the old instances are terminated
    - **Maintains full capacity** during deployment process
    - The impact of a failed update is far less and the rollback process requires only terminating the new auto scaling group
    - **Preferred option for mission critical production systems**