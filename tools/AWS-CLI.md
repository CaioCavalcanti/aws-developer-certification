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
- If you run it from an EC2 instance that has a role attached to it, with permission to do the action you're executing, you won't need to configure an user access key and secret.
    - For examble: attaching to an EC2 instance an EC2 role that has read only permission to S3, you'll be able to `aws s3 ls s3://{bucketName}` 
- Your credentials configured on `aws configure` are saved in a local configuration file on `~/.aws` (or `%UserProfile\.aws%` on windows)
    - You need to remove it to use the EC2 instance role instead of the user credentials