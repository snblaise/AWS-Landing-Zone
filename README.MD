
# AWS Landing Zone Setup with CloudFormation

![AWS Landing Zone Organizational Structure](Automating-landing-zone-deployment-on-AWS.png)

## Introduction
This project provides a CloudFormation template to set up a secure, scalable, and well-governed AWS landing zone. The landing zone includes multiple organizational units (OUs) and IAM roles for different departments, ensuring compliance, security, and efficient management of AWS resources.

## Features
- **Multi-Account Structure**: Leverages AWS Organizations to create and manage multiple AWS accounts.
- **Security Controls**: Implements security policies and guardrails using AWS Control Tower.
- **Centralized Logging and Monitoring**: Sets up centralized logging and monitoring using AWS CloudTrail and Amazon CloudWatch.
- **Infrastructure as Code**: Automates the provisioning of the landing zone using AWS CloudFormation.
- **Compliance Checks**: Conducts regular audits and compliance checks using AWS Config and AWS Security Hub.

## Prerequisites
- AWS CLI installed and configured. For more information, refer to the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).
- AWS account with necessary permissions to create and manage AWS resources.

## Steps for Implementation

### 1. Define the Organizational Structure
Here's a structured organization for your AWS landing zone:

1. **Security Organizational Unit (OU)**
   - Centralized logging
   - Security roles

2. **Sandbox OU**
   - For experimentation and testing

3. **Development OU**
   - For development environments

4. **UAT (User Acceptance Testing) OU**
   - For testing environments

5. **Production OU**
   - For production environments

6. **Shared Resources OU**
   - For shared services and resources

### 2. Create the CloudFormation Template
The CloudFormation template defines the resources and configurations for the landing zone. Below is an example template:

### 3. Set Up AWS CLI
To deploy the CloudFormation stack using the AWS CLI, follow these steps:

1. **Install AWS CLI**: If you haven't already, install the AWS CLI by following the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

2. **Configure AWS CLI**: Configure the AWS CLI with your credentials by running the following command:
   ```sh
   aws configure
   ```
   You'll be prompted to enter your AWS Access Key ID, Secret Access Key, region, and output format.

### 4. Deploy the CloudFormation Stack
In your terminal navigate to the directory where your files are located and use the following command to deploy the CloudFormation stack:

```sh
aws cloudformation create-stack --stack-name MyLandingZoneStack --template-body landingzone.yml --parameters parameters.json --capabilities CAPABILITY_NAMED_IAM
```

### 5. Verify the Created Template and Its Resources Using AWS CLI

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
     aws sts assume-role --role-arn arn:aws:iam::xxxxxxxx9012:role/ManagementRole --role-session-name ManagementSession
     aws sts assume-role --role-arn arn:aws:iam::xxxxxxxx9012:role/DeveloperRole --role-session-name DeveloperSession
     aws sts assume-role --role-arn arn:aws:iam::xxxxxxxx9012:role/SalesRole --role-session-name SalesSession
     ```
   - Verify that users in different departments (e.g., management, developers, sales) can assume their respective roles and perform the necessary actions.

### Conclusion
Creating a landing zone on AWS using CloudFormation is a powerful way to automate the setup of a secure, scalable, and well-governed AWS environment. By leveraging CloudFormation templates, you can ensure consistency, compliance, and efficient management of your AWS resources. Begin building your landing zone today to enhance your AWS infrastructure.

For more information on setting up and using the AWS CLI, you can refer to the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

