# AWS CodePipeline
CodePipeline is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates. It automates the build, test and deploy phases of your release process every time there is a code change, based on the release model you define.

## Useful Links
- [AWS - CodePipeline FAQs](https://aws.amazon.com/codepipeline/faqs)
- [AWS - CodePipeline Best Practices and Use Cases](https://docs.aws.amazon.com/codepipeline/latest/userguide/best-practices.html)
- [AWS - Create and Add a Custom Action in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-custom-action.html)

## General Notes
- Orchestrates build, test & deployment
- Automated: a pipeline is triggered automatically when a change is detected in your source code repository
- Integrates with:
    - CodeCommit
    - CodeBuild
    - CodeDeploy
    - Github,
    - Jenkins
    - Elastic Beanstalk
    - CloudFormation
    - Lambda
    - ECS
- CodePipeline **action**:
    - Is part of the sequence in a stage of a pipeline
    - Is a task performed on the artifact in that satge
    - Actions occur in a specified order, in sequence or parallel, as determined in the configuration of the stage
    - The default types of **actions** are:
        - Source
        - Build
        - Test
        - Deploy
        - Approval
        - Invoke
- You can create **custom actions** in case your release process includes activities that are not included in the default actions
    - When you create a custom action, you alse need to create a **job worker** to poll CodePipeline for job requests for this custom action, execute the job and return the result to CodePipeline.
    - The job worker can be located on any computer or resource **as long as it has access to the public endpoint for CodePipeline**.