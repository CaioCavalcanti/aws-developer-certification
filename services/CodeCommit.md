# AWS CodeCommit
CodeCommit is a fully managed source control service that hosts secure Git-based repositories. It eliminates the need to operate your own source control system or worry about scaling infrastructure.

## Useful Links
- [AWS - CodeCommit FAQs](https://aws.amazon.com/codecommit/faqs/)
- [Practicing Continuous Integration and Continuous Delivery on AWS whitepaper](https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf)
- [YouTube - AWS re:Invent 2018: [REPEAT 1] Continuous Integration Best Practices (DEV319-R1)](https://www.youtube.com/watch?v=77HvSGyBVdU)

## General Notes
- Centralized code repository **based on Git**: store source code, binaries, libraries, images, etc.
- Enables collaboration: manages updates from multiple users
    - Access managed via IAM
- Version control: tracks and manages code changes and maintains version history
- You can configure notifications on repositories, with SNS and CloudWatch