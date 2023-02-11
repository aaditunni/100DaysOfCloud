# Run Commands on an EC2 Instance with AWS Systems Manager

## Introduction

Use AWS Systems Manager to remotely run commands on your Amazon EC2 instances. Systems Manager is a management tool that enables you to gain operational insights and take action on AWS resources safely and at scale. Using the run command, one of the automation features of Systems Manager, you can simplify management tasks by eliminating the need to use bastion hosts, SSH, or remote PowerShell.

## Prerequisite

AWS free tier account.

## Services Covered

- IAM
- EC2
- Systems Manager (SSM)

## Try yourself

### Step 1 — IAM role
Create an IAM role that will be used to give Systems Manager permission to perform actions on your instances.
- Goto the IAM console.
- In the left side menu, choose Roles, and then choose Create role.
- On the Select trusted entity page, under AWS Service, choose EC2, and then choose Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.JPG)

- On the Add permissions page, in the search bar type AmazonEC2RoleforSSM. From the policy list select AmazonEC2RoleforSSM and then choose Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.1.JPG)

- On the Name, review, and create page, in the Role name box, type in Day42_EC2_SSM_Role. In the Description box, type in Enables EC2 instances to access Systems Manager.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.2.JPG)

- Choose Create role.

### Step 2 — EC2 instance
Create an EC2 instance using the EnablesEC2ToAccessSystemsManagerRole role. This will allow the EC2 instance to be managed by Systems Manager.
- Goto the EC2 console and click Launch instance.
- Give a name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.3.JPG)

- Select the AMI (Amazon Linux 2) and instance type (t2.micro) that is free tier eligible.
- Under the Key pair name dropdown, choose Proceed without a key pair.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.4.JPG)

- Under Advanced details, in the IAM instance profile dropdown choose the Day42_EC2_SSM_Role role you created earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.5.JPG)

- Leave the rest all as default configurations.
- Launch Instance.

### Step 3 — SSM
- Goto the Systems Manager console.
- Under the Node Management section on the left side menu, choose Fleet Manager.
- Select the node ID created (EC2) to open the node detail page.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.6.JPG)

- On the node detail page, in the Node actions dropdown, select Execute run command.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.7.JPG)

- On the Run a command page, click in the search bar and select, Document name prefix, then click on Equals, then type in AWS-UpdateSSMAgent.
- Select the radio button on the left of AWS-UpdateSSMAgent. This document will upgrade the Systems Management agent on the instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.8.JPG)

- Scroll down to the Targets panel and select the check box next to your managed EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.9.JPG)

- Leave everything else as default configurations.
- Scroll down and select Run.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.11.JPG)

- Similarly, run another command on the same node id/instance.
- On the Run a command page, click in the search bar and select, Document name prefix, then click on Equals, then type in AWS-RunShellScript.
- Select the radio button on the left of AWS-RunShellScript. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.12.JPG)

- Scroll down to the Command Parameters panel and insert the following command in the Commands text box:
    ```
    sudo yum update –y
    ```
![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.13.JPG)

- Scroll down to the Targets panel and select the check box next to your managed EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.14.JPG)

- Scroll down and select Run.
- While your script is running remotely on the managed EC2 instance, the Overall status will be In Progress. Soon the Overall status will turn to Success. When it does, scroll down to the Targets and outputs panel and select the Instance ID of your instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.15.JPG)

- From the Output on: page, select the header of the Output panel to view the output of the update command from the instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/042/day42.16.JPG)

Terminate the EC2 instance to cleanup.

## ☁️ Cloud Outcome

 Created an EC2 instance and remotely run a command using AWS Systems Manager. 

## Social Proof

[Blog](https://dev.to/aaditunni/run-commands-on-an-ec2-instance-with-aws-systems-manager-1774)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7030147594462932993-SjiD?utm_source=share&utm_medium=member_desktop)
