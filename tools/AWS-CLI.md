# Amazon Command Line Interface (CLI)
The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

It is good to have at least a basic knowledge of the CLI for the certification, but 

## Useful links
- [AWS - CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)
- [GitHub - Project aws/aws-cli](https://github.com/aws/aws-cli)

## General Notes
- Users need an access key and secret (*programmatic access type*) to configure the cli (``aws configure``) before using it
- **Least privilege**: always give your users the minimum amount of access required to do the job 
- Check [IAM](IAM.md) for more instructions on how to create users and get access keys/secrets
- When an access key is deleted/expired, you'll need to run ``aws configure`` again with the new key and secret
- AWS EC2 images already have it installed by default
- You can use it from your pc