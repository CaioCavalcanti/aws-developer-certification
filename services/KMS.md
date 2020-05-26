# AWS Key Management Service (KMS)
KMS is a managed service that makes it easy for you to create and control encryption keys used to encrypt your data. It is integrated with other AWS services, such as EBS, S3, RDS and others.

## Useful Links
- [AWS - KMS FAQs](https://aws.amazon.com/kms/faqs/)
- [AWS Key Management Service Best Practices whitepaper](https://d0.awsstatic.com/whitepapers/aws-kms-best-practices.pdf)

## General Notes
- Encryption keys are **regional**
- KMS is **multi-tenant**, if there's a requirement that doesn't allow it, you should use **CloudHSM** instead
- Split **key administrative and key usage permissions** for security
- **Customer Managed Key (CMK)** consists of:
    - Alias
    - Creation date
    - Description
    - Key state
    - Key material (either customer provided or AWS generated)
- Keys can never be exported
- You can **share CMKs with external accounts** (only for usage)
- Has **built-in auditing with CloudTrail**
- To delete a key you need to first **disable** it and then **schedule deletion**, which **requires a waiting period from 7 to 30 days**
- Cryptographic best practices **discourage extensive reuse of encryption keys**. You have two options to create new cryptographic material for your CMK, you can:
    - **Create new CMK** and change your applications or aliases to use the new CMKs
    - **Enable automatic key rotation** for an existing CMK
- **Automatic key rotation**
    - Benefits:
        - **The properties of the key**, including its key ID, key ARN, region, policies, and permissions, **do not change when the key is rotated**
        - You do not need to change applications or aliases that refer to the CMK ID or ARN
        - After you enable key rotation, KMS **rotates the CMK automatically every year**. You don't need to remember or schedule the update.
    - Is **disabled by default**
    - KMS **saves the CMK's older cryptographic material in perpetuity** so it can be used to decrypt data that it encrypted. 
    - KMS **does not delete any rotated key material** until you delete the CMK
- **Envelope encryption** is the process of **encrypting the key that is used to encrypt data**
    - CMK is used to decrypt the data key (aka envelope key)
    - Envelope key is used to decrypt the data
- Main commands from aws CLI (also available on API and SDKs):
    - `aws kms encrypt`
    - `aws kms decrypt`
    - `aws kms re-encrypt` (when you want to re-encrypt the file with a new CMK)
    - `aws kms enable-key-rotation`