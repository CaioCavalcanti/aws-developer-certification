# Identity Access Management
IAM makes is easy to provide users secure access to your AWS resources, by enabling you to manage IAM users and their access and manage federated users.

## Useful links
- [AWS - IAM FAQ](https://aws.amazon.com/iam/faqs/)
- [AWS - IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS - IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [AWS - IAM Policy Simulator](https://policysim.aws.amazon.com)
- [AWS - Security Token Service - AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html)

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
- Do not share access keys and secrets, create one per user
- If you publish your key and secret by mistake on github, assume that it is compromised and reveoke it right away, as there are bots scanning repositories to get keys/secrets
- **Roles** allow you to not use access key id and secret and **are preferred from a security perspective**
    - The permissions of your IAM user and any roles that you assume are not cumulative, **only a set of permissions is active at a time**
    - When you assume a role, you temporarily give up your previous user or role permissions and work with the permissions that are assigned to the role. When you exit the role, your user permissions are automatically restored
- Changes on a policy take effect immediately
-  **Web Identity Federation** lets you give your users access to AWS resources after they have successfully authenticated with a web-based identity provider (IdP), such as Amazon, Facebook or Google
    - Following a successful authentication, the user receives an authentication code from the IdP, which they can trade for temporary AWS security credentials
    - It is provided by **Amazon Cognito**
- There are **three different types of IAM policies** available:
    - **Managed Policies**
        - Is **created and administered by AWS**
        - Provided for common use cases based on job function, e.g. AmazonDynamoDBFullAccess, AWSCodeCommitPowerUser
        - Help you assign appropriate permissions to your users, groups and roles without having to write the policy yourself
        - A single managed policy **can be attached to multiple users, groups or roles within the AWS account and across different accounts**
        - **You cannot change the permissions** defined in a managed policy
    - **Customer Managed Policies**
        - Is a **standalone policy that you create and administer inside your own AWS account**
        - You can attach this policy to multiple users, groups and roles, but **only within your AWS account**
        - You can copy an existing AWS managed policy and customize it to fit the requirements of your organization
        - Recommended for use cases where the existing AWS managed policies don't meet the needs of your environment
    - **Inline Policies**
        - Is an IAM policy which is actually **embedded with the user, group or role to which it applies**
        - There is a strict 1:1 relationship between the entity and the policy
        - When you delete the user, group or role in which the inline policy is embedded, **the policy will also be deleted**
        - In most cases, AWS recommends using managed policies over inline policies
        - This policy is useful when you want to **be sure that the permissions in the policy are not inadvertently assigned to any other user, group or role** than the one for which it was created
    - Provices **Security Token Service (STS)** web service to **request temporary, limited-privilege credentials** for IAM users or federated users.
    - One of the STS actions is `AssumeRoleWithWebIdentity`, which returns temporary security credentials for users who have been authenticated in a mobile or web application with a web identity provider, such as Cognito, Amazon, Facebook, Google or any OIDC-compatible identity provider
        - By default, the security credentials created by `AssumeRoleWithWebIdentity` **last for one hour**, but you can configure it using the parameter `DurationSeconds`, from 15 minutes up to the maximum session duration setting for the role, which can be from 1 hour to 12 hours.
        - The token created returns `AssumedRoleUser` ARN and `AssumedRoleID`, which are used to reference the temporary credentials, instead of an IAM role or user
- **IAM Policy Simulator** allows you to:
    - Test the effects of IAM policies before committing them to production
    - Validate that the policy works as expected
    - Test policies already attached yo existing users
        - Great for troubleshooting an issue which you suspect is IAM related
- You use different types of security credentials depending on how you interact with AWS. For example, you use a user name and password to sign in to the AWS Management Console. You use access keys to make programmatic calls to AWS API operations or to use AWS CLI commands.
    - When working on development, you need to use AWS access keys to work with AWS resources
- To **assume a role**, an application calls AWS **STS AssumeRole API** operation and passes the ARN of the role to use
    - When you call this API you can pass an optional JSON policy to restrict permissions (not possible to grant permissions in excess of those allowed by the role assumed)

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