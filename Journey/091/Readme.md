# Launch a NGINX server on EC2 using Terraform

## Introduction

Create a terraform template to deploy security group, use default VPC and an EC2 instance running NGINX.

## Prerequisite

- AWS free tier account.
- Terraform installed.
- AWS CLI installed.
- AWS credentials configured (in your vscode, run aws configure and configure your access key, secret key, region, etc).

## Services Covered

- Terraform
- EC2

## Try yourself

### Step 1 — Summary of Step
- Create a folder for your terraform code.
- In terminal, go to the folder or directory you created.
- Run:
    ```
        terraform init
    ```
- Create a terraform file with .tf extension. Installing terraform extension in vscode will help with proper formatting. 
    ```
        provider "aws" {
        profile = "default"
        region  = "us-east-1"
        }

        resource "aws_default_vpc" "default" {}

        resource "aws_security_group" "prod_web" {
        name        = "prod_web"
        description = "allow standard http and https ports inbound everything outbound"

        ingress {
            from_port   = 80
            to_port     = 80
            protocol    = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
        ingress {
            from_port   = 443
            to_port     = 443
            protocol    = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
        egress {
            from_port   = 0
            to_port     = 0
            protocol    = "-1"
            cidr_blocks = ["0.0.0.0/0"]
        }
        }
        resource "aws_instance" "prod_web" {
            ami           = "ami-08749d2ea24a3e607"
            instance_type = "t2.micro"

            vpc_security_group_ids = [
            aws_security_group.prod_web.id
            ]

            tags = {
            "Terraform" : "true"
        }
        }
    ```
- Paste the above code in your terraform file and save it. 
- Go to the AWS Marketplace and search for NGINX server, choose the one from Bitnami, subscribe it and copy the AMI ID and replace it with the AMI ID in the above code. For instance type select any free tier one.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/091/day91.2.JPG)

- Run the below code to see the execution plan:
    ```
        terraform plan
    ```
- Run the below command to deploy the resources:
    ```
        terraform apply
    ```
- Type yes to proceed.
- Go to the AWS console and check if the resources are created.
- Go to the EC2 instance and check it out using the Public IP address.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/091/day91.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/091/day91.1.JPG)

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

Created a terraform template to deploy security group, use default VPC and an EC2 instance running NGINX.

## Social Proof

[Blog](https://dev.to/aaditunni/launch-a-nginx-server-on-ec2-using-terraform-4jbg)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7048043657140015104-IX0L?utm_source=share&utm_medium=member_desktop)
