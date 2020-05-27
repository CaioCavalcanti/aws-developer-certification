# AWS CodeBuild
CodeBuild is a fully managed continuous integration service that compiles source code, runs tests and produces software packages that are ready to deploy. With CodeBuild you don't need to provision, manage and scale your own build servers.

## Useful Links
- [AWS - CodeBuild FAQs](https://aws.amazon.com/codebuild/faqs)
- [YouTube - AWS re:Invent 2017: Continuous Integration Best Practices for Software Development Teams (DEV322)](https://www.youtube.com/watch?v=GEPJ7Lo346A)

## General Notes
- The build specification is defined on a `buildspec.yml` file
    - File should be on the root directory, otherwise you need to provide the path where it is stored
    - You can override the `buildspec.yml` file if necessary
- You can view the full log in CloudWatch
- You can setup notifications
- Pricing:
    - Free tier includes 100 minutes of build.general1.small per month and does not expire after the 12-month free tier term
    - After that, princing depends on the size of build instance type and region