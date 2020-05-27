# AWS CloudFormation
CloudFormation is a service that allows you to manage, configure and provision your AWS **infrastructure as code**. 

## Useful Links
- [AWS - CloudFormation FAQs](https://aws.amazon.com/cloudformation/faqs/)
- [AWS - AWS CloudFormation Best Practices](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html)
- [AWS - Template Snippets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/CHAP_TemplateQuickRef.html)

## General Notes
- Resources are defined using a CloudFormation template
- CloudFormation interprets the template and makes the appropriate API calls to create the resources you have defined
- Supports YAML or JSON
- Benefits:
    - Infrastructure is provisioned consistently, with fewer mistakes
    - Less time and effort than configuring things manually
    - You can version control and peer review your templates
    - Free to use (charged only for what you create)
    - Can be used to manage updates and dependencies
    - Can be used to rollback and delete the entire stack
- CloudFormation Template:
    - Describes the endstate of the infrastructure you are either provisioning or changing
    - After creating the template, you upload it to CloudFormation using S3
    - CloudFormation reads the template and makes the API calls on your behalf
    - The resulting resources are called a **Stack**
    - `Resources` is the only mandatory section of the template
- You can use **change sets** to update stacks, so you can preview how proposed changes to a stack might impact your running resources. The usual process is:
    - Create change set
    - View change set
    - Execute change set
- Use CloudFormation whenever is possible, it will save you time setting up and tearing down, if necessary
- You can use **designer** on browser to create the template

## Template Structure
```yml
AWSTemplateFormatVersion: "2010-09-09" # only valid value
Description: "Details about the template"

# provides details about the template
Metadata:
    Databases:
        Description: "Info about the databases"

#input values
Parameters:
    EnvType: 
        Description: Environment type.
        Default: test
        Type: String
        AllowedValues: 
            - prod
            - test
        ConstraintDescription: must specify prod or test.

# matches a key to a corresponding set of values
Mappings:
    RegionMap: 
        us-east-1: 
            "HVM64": "ami-0ff8a91507f77f867"
        us-west-1: 
            "HVM64": "ami-0bdb828fd58c52235"

# use to create/update resources under certain circumstances
Conditions: 
    CreateProdResources: !Equals [ !Ref EnvType, prod ]

# includes snippets of code outside the main template
Transform:
    Name: "AWS::Include"
    Parameters:
        Location: "s3://bucketName/snippet.yaml"

# required, declares the AWS resources you want to include on the stack
Resources:
    myEC2Instance: 
        Type: "AWS::EC2::Instance"
        Properties: 
            ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
            InstanceType: m1.small
    NewVolume: 
        Type: "AWS::EC2::Volume"
        Condition: CreateProdResources
        Properties: 
            Size: 100
            AvailabilityZone: 
            !GetAtt EC2Instance.AvailabilityZone

# declares output values that can you can import into other stacks, return in response or view on console
Outputs:
    InstanceID:
        Description: The Instance ID
        Value: !Ref myEC2Instance
    VolumeId: 
        Condition: CreateProdResources
        Value: 
            !Ref NewVolume

```