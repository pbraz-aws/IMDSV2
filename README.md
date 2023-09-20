# Automate enforcement of IMDSv2 for EC2 instances using Step Functions and Systems Manager

## About

1. Uses DescribeInstances to find EC2 instances that do not have IMDSV2 set to “Required”
2. Iterates through the matched instances and runs the [AWSSupport-ConfigureEC2Metadata](https://us-east-1.console.aws.amazon.com/systems-manager/documents/AWSSupport-ConfigureEC2Metadata/description?region=us-east-1) Systems Manager Automation document which sets EnforceIMDSv2 to “required”
 
![Step Function Diagram](/images/stepfunction.png)

Below are the IAM permissions that CloudFormation creates to run this Step Function

```
Statement:
    - Effect: "Allow"
        Action:
        - "ec2:DescribeInstances"
        - "ec2:ModifyInstanceMetadataOptions"
        - "ssm:StartAutomationExecution"
        Resource: "*"
```
**Important**: If you enforce IMDSv2, then IMDSv1 no longer works, and applications that use IMDSv1 might not function correctly. Before enforcing IMDSv2, verify that any applications that use Amazon EC2 metadata are upgraded to a version that supports IMDSv2. For more information about instance metadata, see [Configure the instance metadata service.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html)
 
AWS provides no support for this Step Function and encourages customers to review before executing in their environment.

## Deployment
---
### AWS SAM
 Start by installing the [AWS SAM CLI](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwis8-ub6bmBAxXblWoFHZAxBGQQFnoECBAQAQ&url=https%3A%2F%2Fdocs.aws.amazon.com%2Fserverless-application-model%2Flatest%2Fdeveloperguide%2Finstall-sam-cli.html&usg=AOvVaw2kBjl30k-667NWc24k-fIu&opi=89978449)

 ```
 cd IDMSv2
 sam build && sam deploy -g
 ```