# Identity Access Management
IAM makes is easy to provide users secure access to your AWS resources, by enabling you to manage IAM users and their access and manage federated users.

It's available at `AWS Console > Services > Security, Identity & Compliance > IAM`

## Critical Terms
- Users: end users (people)
- Groups: a collection of users under one set of permissions
- Roles: used to define a set of permissions to AWS resources
- Policies: a document that defines one or more permissions. It can be attached to an user, group or/and role.

## Best practices
- Follow security checklist after creating a new account
    - Delete your root access keys
        - Is usually done automatically
    - Enable multi factor authentication (MFA) on root account
    - Create individual IAM users
    - Use groups to assign permissions
    - Apply an IAM password policy
- Rotate credentials regularly
- Grant least privilege
- Use roles to delegate permissions
- Monitor activity in your AWS account
- Create an administrator user to manage users, so you don't need to use the root account
- Use policy simulator to test policy changes to ensure they have the desired effect before committing them to production

## Useful links
- [AWS - IAM FAQ](https://aws.amazon.com/iam/faqs/)
- [AWS - IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS - IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [AWS - IAM Policy Simulator](https://policysim.aws.amazon.com)

## General Notes
- IAM is universal, does not apply to individual regions
- Has no additional charges
- Root account:
    - Is the e-mail used to create the AWS account
    - Has full access (is not bounded to any IAM policies)
    - Should not be used for day to day activities (use IAM users instead)
- You can configure federation to enable SSO
- New users have no permissions by default, you need to set it up
- You can configure a custom AWS Console address for your users to login (i.e. `https://{accountAlias}.signin.aws.amazon.com/console`)
- User access types:
    - Programmatic
        - Enables an access key id and secret to access via AWS API, SDK, CLI, etc
        - Used by applications
        - Access key secret is shown only once when you create it, if you lose its value you'll need to create a new one
    - AWS Management Console
        - Enables password to allow user to access AWS Management Console
- Policy document example to grant access to add, update, and delete objects from a specific folder, example_folder, in a specific bucket, example_bucket:
```
{
    "Version":"2012-10-17",
    "Statement": [
        {
            "Effect":"Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion"
            ], 
            "Resource":"arn:aws:s3:::example_bucket/example_folder/*"
        }
    ]
}
```
- Any AWS resources created by a user are under control of and paid for by its AWS account
- Groups cannot belong to other groups
- Security credentials available: AWS access key, X.509 certificate, SSH key (only for CodeCommit at the moment), password for web app logins, or a MFA device
- You can't set usage quota for individual IAM users. Limits are set at the account level.
- There is no limit to the number of IAM roles you can assume, but you can only act as one IAM role when making requests to AWS services
- You can protect APIs with MFA, it is available for all AWS services that support temporary security credentials
- MFA cannot be used with federated users