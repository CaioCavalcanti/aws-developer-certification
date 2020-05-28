# Amazon Cognito
Cognito provides authentication, authorization and user management for your web and mobile apps. Your users can sign in directly with an user name and password or through a third party such as Facebook, Amazon, Google and Apple

## Useful Links
- [AWS - Security Best Practices for Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/managing-security.html)
- [AWS - Common Amazon Cognito Scenarios](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-scenarios.html)
- [YouTube - AWS re:Invent 2016: Serverless Authentication and Authorization: Identity Management (MBL306)](https://www.youtube.com/watch?v=n4hsWVXCuVI)
- [Ten Mile Square Blog - AWS SAM API with Cognito](https://tenmilesquare.com/aws-sam-api-with-cognito/)

## General Notes
- Acts as an **identity broker**, handling all interaction with web identity providers
- Some of the features provided:
    - Sign-up and sign-in to your apps
    - Access for guest users
    - Acts as an Identity Broker between your application and Web ID providers, so you don't need to write any additional code
    - Synchronizes user data for multiple devices
    - Recommended for all mobile apps running with AWS services
    - Multi-factor authentication
    - Email verification
        - You can customize the email using SES or use the default provided by Cognito
- For third party providers, it provides temporary credentials which map to an IAM role, allowing access to the required resources
- There's no need for the app to embed or store AWS credentials locally on the device, giving users a seamless experience across all devices
- The two **main components of Cognito** are user pools and identity pools
    - **User pools** are user directories that provide **sign-up and sign-in** options for your app users
    - **Identity pools** enable you to grant your users **access to other AWS services**
    - You can use both separately or together
- Cognito tracks the association between user identity and the various different devices they sign-in from and it uses **push synchronization** to push updates and synchronize user data across multiple devices
    - It uses **SNS to send a silent push notification** to all the devices associated with a given user identity whenever data stored in the cloud changes
