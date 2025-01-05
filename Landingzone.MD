
---
kicker: "AWS CloudFormation"
title: "Automating AWS Landing Zone Setup"
subtitle: "A Comprehensive Guide to Establishing a Secure and Scalable AWS Environment"
---

## AWS Landing Zone Automation With CloudFormation
![AWS Landing Zone Organizational Structure](./image.png)
### Introduction
Establishing a secure, scalable, and well-governed environment is crucial for organizations in today's cloud-centric world. This is where a landing zone comes into play. A landing zone is a pre-configured, secure, multi-account AWS environment that adheres to best practices. It provides a foundation for your AWS infrastructure, ensuring compliance, security, and efficient management.

For more information on landing zones, you can refer to the [AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html).

### Why the Landing Zone
A landing zone is essential for several reasons:
 - **Security and Compliance**: It ensures that your AWS environment adheres to security and compliance standards.
 - **Scalability**: It provides a scalable foundation for your AWS resources.
 - **Governance**: It helps in managing and governing multiple AWS accounts efficiently.
 - **Cost Management**: It aids in tracking and managing costs across different accounts.

### How a Landing Zone is Created

Creating a landing zone involves several steps and best practices to ensure a secure and scalable environment. Here's a high-level overview of the process:

1. **Use AWS Organizations**: Leverage AWS Organizations to create and manage multiple AWS accounts. This helps in isolating workloads and managing permissions effectively.

2. **Implement Security Controls**: Use AWS Control Tower to enforce security policies and guardrails. AWS Control Tower automates the setup of a secure, well-architected multi-account AWS environment based on AWS best practices.

3. **Centralize Logging and Monitoring**: Set up centralized logging and monitoring using AWS CloudTrail and Amazon CloudWatch. AWS CloudTrail records AWS API calls for your account and delivers log files to you, while Amazon CloudWatch monitors your AWS resources and applications in real-time. This ensures that all activities are logged and monitored for security and compliance purposes.

4. **Automate with Infrastructure as Code**: Use AWS CloudFormation to automate the provisioning of your landing zone. AWS CloudFormation allows you to model and set up your Amazon Web Services resources so that you can spend less time managing those resources and more time focusing on your applications. CloudFormation templates allow you to define your infrastructure as code, making it easy to replicate and manage.

5. **Regular Audits and Compliance Checks**: Conduct regular audits and compliance checks using AWS Config and AWS Security Hub. AWS Config provides a detailed view of the configuration of AWS resources in your account, while AWS Security Hub aggregates, organizes, and prioritizes security alerts and findings from multiple AWS services. These services help you maintain compliance with industry standards and best practices.

For detailed guidance on building a landing zone, check out the [AWS Prescriptive Guidance on Building a Landing Zone](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/building-landing-zones.html).

### AWS Landing Zone Organizational Structure
Here's a sample structured organization for our AWS landing zone:

1. **Security Organizational Unit (OU)**
  - **Centralized Logging**: This unit is responsible for aggregating and storing logs from various AWS accounts and services. It ensures that all activities are logged for security and compliance purposes.
  - **Security Roles**: This unit manages roles and permissions related to security, ensuring that security policies and guardrails are enforced across the organization.

2. **Sandbox OU**
  - **For Experimentation and Testing**: This unit provides a safe environment for developers and teams to experiment with new services, features, and configurations without affecting production environments. It allows for innovation and testing of new ideas.

3. **Development OU**
  - **For Development Environments**: This unit hosts development environments where application development takes place. It provides isolated environments for developers to build and test their applications before moving to higher environments.

4. **UAT (User Acceptance Testing) OU**
  - **For Testing Environments**: This unit is dedicated to user acceptance testing. It allows stakeholders and end-users to test and validate the applications in an environment that closely resembles production, ensuring that the applications meet the required standards and specifications.

5. **Production OU**
  - **For Production Environments**: This unit hosts the production environments where live applications and services run. It ensures high availability, reliability, and performance for end-users. Strict security and compliance measures are enforced in this unit.

6. **Shared Resources OU**
  - **For Shared Services and Resources**: This unit manages shared services and resources that are used across multiple accounts and environments. It includes services like networking, directory services, and other common infrastructure components that need to be centrally managed.

This structured organization helps in maintaining a clear separation of responsibilities, enhances security, and ensures efficient management of AWS resources across different stages of the application lifecycle.

### Creating a Landing Zone Using AWS CloudFormation
AWS CloudFormation is a service that allows you to define and provision AWS infrastructure as code. By using CloudFormation templates, you can automate the creation of your landing zone.

#### Step 1: Define the CloudFormation Template
First, you need to define a CloudFormation template that specifies the resources and configurations for your landing zone. In the following section, we will break down our template written in YAML language to understand the various sections in detail.

#### CloudFormation Template Breakdown

A CloudFormation template is a JSON or YAML formatted text file that describes the AWS infrastructure needed to deploy your landing zone. The template consists of several key sections:

1. **Parameters**: This section defines the inputs required for the template. Parameters allow you to customize aspects of your template at runtime. For example, you can specify the names of organizational units, account IDs, and retention periods.

2. **Resources**: This is the most critical section of the template. It defines the AWS resources that CloudFormation will create and manage. In our case, this includes the landing zone setup, IAM roles, and role attachments.

3. **Outputs**: This optional section declares output values that you can import into other stacks or view on the CloudFormation console. Outputs can include resource IDs, IP addresses, or any other information you might need after the stack is created.

4. **Mappings**: This section allows you to create custom mappings like region-specific AMIs, which can be used to conditionally set values in your template.

5. **Conditions**: Conditions enable you to control whether certain resources are created or certain properties are assigned based on input parameter values.

6. **Metadata**: This section is used to include additional information about the template. It can be helpful for documentation purposes.

Below is an example of a CloudFormation template that sets up a landing zone with various organizational units and IAM roles:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: AWS Landing Zone Setup with User Roles and Attachments

# Metadata section
Metadata:
  AWS::CloudFormation::Interface:
   ParameterGroups:
    - Label:
       default: "Organizational Units"
      Parameters:
       - SecurityOuName
       - SandboxOuName
       - DevelopmentOuName
       - UatOuName
       - ProductionOuName
       - SharedResourcesOuName
    - Label:
       default: "Account IDs"
      Parameters:
       - CentralizedLoggingAccountId
       - SecurityAccountId
    - Label:
       default: "Retention Periods"
      Parameters:
       - LoggingBucketRetentionPeriod
       - AccessLoggingBucketRetentionPeriod
    - Label:
       default: "KMS Key"
      Parameters:
       - KMSKey

# Mappings section
Mappings:
  RegionMap:
   us-east-1:
    AMI: ami-0axxxxxxxxxxxx
   us-west-2:
    AMI: ami-0axxxxxxxxxxxx

# Conditions section
Conditions:
  IsUSEast1: !Equals [ !Ref "AWS::Region", "us-east-1" ]
  IsUSWest2: !Equals [ !Ref "AWS::Region", "us-west-2" ]

# Parameters section 
Parameters:
  GovernedRegions:
   Type: List
   Description: List of governed regions
  SecurityOuName:
   Type: String
   Description: The security Organizational Unit name
  SandboxOuName:
   Type: String
   Description: The sandbox Organizational Unit name
  DevelopmentOuName:
   Type: String
   Description: The development Organizational Unit name
  UatOuName:
   Type: String
   Description: The UAT Organizational Unit name
  ProductionOuName:
   Type: String
   Description: The production Organizational Unit name
  SharedResourcesOuName:
   Type: String
   Description: The shared resources Organizational Unit name
  CentralizedLoggingAccountId:
   Type: String
   Description: The AWS account ID for centralized logging
  SecurityAccountId:
   Type: String
   Description: The AWS account ID for security roles
  LoggingBucketRetentionPeriod:
   Type: Number
   Description: Retention period for centralized logging bucket
  AccessLoggingBucketRetentionPeriod:
   Type: Number
   Description: Retention period for access logging bucket
  KMSKey:
   Type: String
   Description: The ARN of the KMS key for encryption

# Resources section
Resources:
  MyLandingZone:
   Type: 'AWS::ControlTower::LandingZone'
   Properties:
    Version: '1.0'
    Tags:
      - Key: 'Environment'
       Value: 'Production'
    Manifest:
      governedRegions: !Ref GovernedRegions
      organizationStructure:
       security:
        name: !Ref SecurityOuName
       sandbox:
        name: !Ref SandboxOuName
       development:
        name: !Ref DevelopmentOuName
       uat:
        name: !Ref UatOuName
       production:
        name: !Ref ProductionOuName
       sharedResources:
        name: !Ref SharedResourcesOuName
      centralizedLogging:
       accountId: !Ref CentralizedLoggingAccountId
       configurations:
        loggingBucket:
          retentionDays: !Ref LoggingBucketRetentionPeriod
        accessLoggingBucket:
          retentionDays: !Ref AccessLoggingBucketRetentionPeriod
        kmsKeyArn: !Ref KMSKey
        enabled: true
      securityRoles:
       accountId: !Ref SecurityAccountId
       accessManagement:
        enabled: true

  ManagementRole:
   Type: 'AWS::IAM::Role'
   Properties:
    RoleName: !Ref ManagementRoleName
    AssumeRolePolicyDocument:
      Version: '2012-10-17'
      Statement:
       - Effect: Allow
        Principal:
          AWS: '*'
        Action: 'sts:AssumeRole'
    Policies:
      - PolicyName: ManagementPolicy
       PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
           Action:
            - 'ec2:*'
            - 's3:*'
            - 'cloudwatch:*'
           Resource: '*'

  DeveloperRole:
   Type: 'AWS::IAM::Role'
   Properties:
    RoleName: !Ref DeveloperRoleName
    AssumeRolePolicyDocument:
      Version: '2012-10-17'
      Statement:
       - Effect: Allow
        Principal:
          AWS: '*'
        Action: 'sts:AssumeRole'
    Policies:
      - PolicyName: DeveloperPolicy
       PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
           Action:
            - 'ec2:*'
            - 's3:*'
            - 'cloudwatch:*'
           Resource: '*'

  SalesRole:
   Type: 'AWS::IAM::Role'
   Properties:
    RoleName: !Ref SalesRoleName
    AssumeRolePolicyDocument:
      Version: '2012-10-17'
      Statement:
       - Effect: Allow
        Principal:
          AWS: '*'
        Action: 'sts:AssumeRole'
    Policies:
      - PolicyName: SalesPolicy
       PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
           Action:
            - 's3:*'
            - 'cloudwatch:*'
           Resource: '*'

  AttachManagementRole:
   Type: 'AWS::IAM::RolePolicyAttachment'
   Properties:
    RoleName: !Ref ManagementRoleArn
    PolicyArn: !Sub 'arn:aws:iam::${ManagementAccountId}:policy/ManagementPolicy'

  AttachDeveloperRole:
   Type: 'AWS::IAM::RolePolicyAttachment'
   Properties:
    RoleName: !Ref DeveloperRoleArn
    PolicyArn: !Sub 'arn:aws:iam::${DeveloperAccountId}:policy/DeveloperPolicy'

  AttachSalesRole:
   Type: 'AWS::IAM::RolePolicyAttachment'
   Properties:
    RoleName: !Ref SalesRoleArn
    PolicyArn: !Sub 'arn:aws:iam::${SalesAccountId}:policy/SalesPolicy'

# Outputs section
Outputs:
  LandingZoneId:
   Description: "The ID of the created landing zone"
   Value: !Ref MyLandingZone
  ManagementRoleArn:
   Description: "The ARN of the Management IAM Role"
   Value: !GetAtt ManagementRole.Arn
  DeveloperRoleArn:
   Description: "The ARN of the Developer IAM Role"
   Value: !GetAtt DeveloperRole.Arn
  SalesRoleArn:
   Description: "The ARN of the Sales IAM Role"
   Value: !GetAtt SalesRole.Arn
  CentralizedLoggingBucket:
   Description: "The name of the centralized logging S3 bucket"
   Value: !Ref CentralizedLoggingBucket
  AccessLoggingBucket:
   Description: "The name of the access logging S3 bucket"
   Value: !Ref AccessLoggingBucket
```

This template provides a comprehensive setup for an AWS landing zone, including organizational units, centralized logging, and IAM roles. By defining your infrastructure as code, you can ensure consistency and repeatability across your AWS environments. 

#### Additional Roles and Responsibilities Note
More roles and their responsibilities can be added based on the needs and responsibilities following least privilege rules for employees of the company e.g DevSecOps roles, cloud engineer role etc. 

For more detailed information on setting up a landing zone, refer to the [AWS Prescriptive Guidance on Setting Up a Landing Zone](https://docs.aws.amazon.com/prescriptive-guidance/latest/designing-control-tower-landing-zone/setup.html).

#### Step 2: Deploy the AWS CloudFormation Stack Using AWS CLI
To deploy the CloudFormation stack using the AWS CLI, follow these steps:

1. **Install AWS CLI**: If you haven't already, install the AWS CLI by following the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

2. **Configure AWS CLI**: Configure the AWS CLI with your credentials by running the following command:
   ```sh
   aws configure
   ```
   You'll be prompted to enter your AWS Access Key ID, Secret Access Key, region, and output format.

3. **Deploy the CloudFormation Stack**: Use the following command to deploy the CloudFormation stack:
  ```sh
    aws cloudformation create-stack --stack-name MyLandingZoneStack --template-body file://C:\Users\blaise\Document\Projects\landingzone\landingzonetemplate.yaml --parameters file://C:\Users\blaise\Document\Projects\landingzone\parameters.json
  ```

#### Step 3: Verify the Created Template and Its Resources Using AWS CLI

After deploying the CloudFormation stack, it's essential to verify that the template and its resources have been created correctly. Follow these steps to ensure everything is set up as expected using the AWS CLI:

1. **Check Stack Status**:
   - Use the following command to check the status of your CloudFormation stack:
     ```sh
     aws cloudformation describe-stacks --stack-name MyLandingZoneStack
     ```
   - Ensure the stack status is `CREATE_COMPLETE`. If the status is `CREATE_FAILED`, review the events and logs to identify and resolve any issues.

2. **List Stack Resources**:
   - Use the following command to list all resources created by the stack:
     ```sh
     aws cloudformation list-stack-resources --stack-name MyLandingZoneStack
     ```
   - Verify that all expected resources (e.g., organizational units, IAM roles, logging configurations) have been created.

3. **Describe IAM Roles**:
   - Use the following command to describe the IAM roles created by the CloudFormation stack:
     ```sh
     aws iam get-role --role-name ManagementRole
     aws iam get-role --role-name DeveloperRole
     aws iam get-role --role-name SalesRole
     ```
   - Ensure that the roles have the correct policies attached and that they are assigned to the appropriate accounts.

4. **Verify Logging and Monitoring**:
   - Use the following command to check the status of CloudTrail:
     ```sh
     aws cloudtrail describe-trails
     ```
   - Ensure that CloudTrail is logging events for all specified regions.

   - Use the following command to check CloudWatch metrics:
     ```sh
     aws cloudwatch list-metrics
     ```
   - Verify that CloudWatch is monitoring the necessary metrics.

5. **Test Access and Permissions**:
   - Use the following command to assume the created IAM roles and test their access and permissions:
     ```sh
     aws sts assume-role --role-arn arn:aws:iam::xxxxxxxxx012:role/ManagementRole --role-session-name ManagementSession
     aws sts assume-role --role-arn arn:aws:iam::xxxxxxxxx012:role/DeveloperRole --role-session-name DeveloperSession
     aws sts assume-role --role-arn arn:aws:iam::xxxxxxxxx012:role/SalesRole --role-session-name SalesSession
     ```
   - Verify that users in different departments (e.g., management, developers, sales) can assume their respective roles and perform the necessary actions.

### Conclusion
Automating your AWS landing zone setup with CloudFormation ensures a secure, scalable, and well-governed environment. Leverage CloudFormation templates for consistency, compliance, and efficient management of AWS resources.

For more information, refer to the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

