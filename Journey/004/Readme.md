# Deploy a WordPress page using CloudFormation

## Introduction

Deploy a WordPress page using the CloudFormation sample template and write a blog.

## Prerequisite

AWS free tier account.
## Use Case

- Automate, test, and deploy infrastructure templates with continuous integration and delivery (CI/CD) automations.
- Run anything from a single Amazon Elastic Compute Cloud (EC2) instance to a complex multi-region application.
- Define an Amazon Virtual Private Cloud (VPC) subnet or provisioning services like AWS OpsWorks or Amazon Elastic Container Service (ECS) with ease.

## Services Covered

- AWS CloudFormation

## Try yourself

### Step 1 — 
Goto EC2 console and on the the left hand side under Network and Security, select Key Pairs and create one. This will be needed while creating the CloudFormation template.

### Step 1 — 
Search CloudFormation and select create Stack.
- Select WordPress blog from the simple category in Select a sample template section and click Next
- Enter the Stack Name, DbName, DBPassword, DBRootPassword and DBUser.
- For KeyName, select the Key Pair you created earlier.
- Leave everyhting as default and click Next and finally click Submit to create the stack.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/004/day4.JPG)

### Step 3 — 
- Once the stack is created, goto Outputs and click on the link. This will open a page where you install WordPress.
- Fill in the details and install.
- Login and now you have your WOrdPress installed and ready to create a post.
- Create a blog to test it out.
- Delete the CloudFormation Stack to cleanup.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/004/day4.1.JPG)


## ☁️ Cloud Outcome

Deployed a WordPress page using the CloudFormation sample template and wrote a blog.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/004/day4.2.JPG)

## Social Proof

[Blog](https://dev.to/aaditunni/deploy-a-wordpress-page-using-aws-cloudformation-2jkm)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7016336355479707648-5IQo?utm_source=share&utm_medium=member_desktop)
