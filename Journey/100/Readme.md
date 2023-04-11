# Create an advanced AWS VPC to host a fully functioning cloud native application

## Introduction

The VPC will span 2 AZs, and have both public and private subnets. An internet gateway and NAT gateway will be deployed into it. Public and private route tables will be established. An application load balancer (ALB) will be installed which will load balance traffic across an auto scaling group (ASG) of Nginx web servers installed with the cloud native application frontend and API. A database instance running MongoDB will be installed in the private zone. Security groups will be created and deployed to secure all network traffic between the various components.
For demonstration purposes only - both the frontend and the API will be deployed to the same set of ASG instances - to reduce running costs.
The ALB will configured with a single listener (port 80). 2 target groups will be established. The frontend target group points to the Nginx web server (port 80). The API target group points to the custom API service (port 8080). The Web Application is a Programming Language Vote App where you can vote or your favorite language from a list.

## Prerequisite

- AWS free tier account.
- Terraform installed.
- AWS CLI installed.
- AWS credentials configured (in your vscode, run "aws configure" and configure your access key, secret key, region, etc).
- [Project file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/tree/main/Journey/100/app-day100) downloaded and extracted.
- A Key Pair created.
- Your IP address noted.

## Services Covered

- Terraform
- EC2

## Try yourself

### Step 1 — Summary of Step
- Open the folder in your coder editor. I use VS Code.
- The region is in us-west-2 so it will be deployed in that region. You can go through the code, understand it an make changes if you wish. You can change the region, AMI, instance type but make they are supported in that region you select.
- Run:
    ```
        terraform init
    ```
- Run the below code to see the execution plan:
    ```
        terraform plan
    ```
- It will ask you for the key_name (value of your Key Pair). Enter the name of your Key Pair.
- Then it will ask for the workstation_ip (your IP address). Enter it with /32. Eg: 192.168.10.1/32.
- Run the below command to deploy the resources:
    ```
        terraform apply
    ```
- Enter the same details again.
- Type yes to proceed.
- Go to the AWS console and check if the resources are created.

- You will get a url of your load balancer in your terminal or you can get the url from your Load Balancer console to access the webpage.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/100/day100.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/100/day100.1.JPG)

- In order to destroy the resource you might want to run plan and see what's needed to be done by terraform, and save the output to a file:
    ```
        terraform plan -destroy -out destroy.plan
    ```
- Then apply that plan:
    ```
        terraform apply destroy.plan
    ```
- This will delete all the resources created and cleanup.

## ☁️ Cloud Outcome

Created an advanced AWS VPC to host a fully functioning cloud native application that lets you vote your favorite programming language from a list.

## Social Proof

[Blog](https://dev.to/aaditunni/create-an-advanced-aws-vpc-to-host-a-fully-functioning-cloud-native-application-1l4k)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7051625951188664320-uLk-?utm_source=share&utm_medium=member_desktop)
