# Identity Access Management
Allows you to manage users and their level of access to AWS Console.

## Critical Terms
- Users: end users (people)
- Groups: a collection of users under one set of permissions
- Roles: used to define a set of permissions to AWS resources
- Policies: a document that defines one or more permissions. It can be attached to an user, group or/and role.

## Best practices
- Lock your AWS account root user access keys
- Enable multi factor authentication (MFA)
- Configure a strong password policy for your users
- Rotate credentials regularly
- Grant least privilege
- Use groups to assign permissions to IAM Users
- Use roles to delegate permissions
- Monitor activity in your AWS account

## Useful links
- [AWS - IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)