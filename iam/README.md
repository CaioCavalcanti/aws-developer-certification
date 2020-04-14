# Identity Access Management
Allows you to manage users and their level of access to AWS Console. Available at `AWS Console > Services > Security, Identity & Compliance > IAM`

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

## Useful links
- [AWS - IAM FAQ](https://aws.amazon.com/iam/faqs/)
- [AWS - IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS - IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

## General Notes
- IAM is universal, does not apply to individual regions
- Root account:
    - Is the e-mail used to create the AWS account
    - Has full access 
    - Should not be used for day to day activities (use IAM users instead)
- You can configure federation to enable SSO
- New users have no permissions by default, you need to set it up
- You can configure a custom AWS Console address for your users to login (i.e. https://{givenAlias}.signin.aws.amazon.com/console)
- User access types:
    - Programmatic
        - Enables an access key id and secret to access via AWS API, SDK, CLI, etc
        - Used by applications
        - Access key secret is shown only once when you create it, if you lose its value you'll need to create a new one
    - AWS Management Console
        - Enables password to allow user to access AWS Management Console
- Policy document example for full access:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```