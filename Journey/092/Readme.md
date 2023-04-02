# Deploy a Highly Available Group of EC2 Instances Using Terraform

## Introduction

Deploy a highly available group of EC2 Instances with load balancer and autoscaling group using Terraform.

## Prerequisite

- AWS free tier account.
- Terraform installed.
- AWS CLI installed.
- AWS credentials configured (in your vscode, run aws configure and configure your access key, secret key, region, etc).

## Services Covered

- Terraform
- EC2
- VPC

## Try yourself


### Step 1 — Summary of Step
- Create a folder for your terraform code.
- In terminal, go to the folder or directory you created.
- First we need to create a user-script file that will be called by the main terraform file while creating resources. Create a file named server.sh and paste the below code:
    ```
        #!/bin/bash
        apt update
        apt upgrade -y
        apt install apache2 -y
        echo "<h1>Hello world from $(hostname -f)</h1>" > /var/www/html/index.html
        systemctl start apache2
        systemctl enable apache2
    ```
- Create a terraform file with .tf extension. Installing terraform extension in vscode will help with proper formatting. 
- Paste the below code in your terraform file and save it. 
    ```
        provider "aws" {
        profile = "default"
        region  = "us-east-1"
        }

        resource "aws_vpc" "main" {
        cidr_block = "10.0.0.0/16"
        
        tags = {
            Name = "my-app-vpc"
        }
        }

        resource "aws_subnet" "public_a" {
        vpc_id     = aws_vpc.main.id
        cidr_block = "10.0.1.0/24"
        availability_zone = "us-east-1a"

        tags = {
            Name = "public_a"
        }
        }

        resource "aws_subnet" "public_b" {
        vpc_id     = aws_vpc.main.id
        cidr_block = "10.0.2.0/24"
        availability_zone = "us-east-1b"

        tags = {
            Name = "public_b"
        }
        }

        resource "aws_subnet" "public_c" {
        vpc_id     = aws_vpc.main.id
        cidr_block = "10.0.3.0/24"
        availability_zone = "us-east-1c"

        tags = {
            Name = "public_c"
        }
        }

        resource "aws_internet_gateway" "main-igw" {
        vpc_id = aws_vpc.main.id

        tags = {
            Name = "main-igw"
        }
        }

        resource "aws_route_table" "public_rt" {
        vpc_id = aws_vpc.main.id

        route {
            cidr_block = "0.0.0.0/0"
            gateway_id = aws_internet_gateway.main-igw.id
        }

        tags = {
            Name = "public_rt"
        }
        }

        resource "aws_route_table_association" "a" {
        subnet_id      = aws_subnet.public_a.id
        route_table_id = aws_route_table.public_rt.id
        }

        resource "aws_route_table_association" "b" {
        subnet_id      = aws_subnet.public_b.id
        route_table_id = aws_route_table.public_rt.id
        }

        resource "aws_route_table_association" "c" {
        subnet_id      = aws_subnet.public_c.id
        route_table_id = aws_route_table.public_rt.id
        }

        resource "aws_security_group" "my_sg" {
        name        = "my-sg"
        vpc_id      = aws_vpc.main.id

        ingress {
            description      = "Allow http from everywhere"
            from_port        = 80
            to_port          = 80
            protocol         = "tcp"
            cidr_blocks      = ["0.0.0.0/0"]
        }

        ingress {
            description      = "Allow http from everywhere"
            from_port        = 22
            to_port          = 22
            protocol         = "tcp"
            cidr_blocks      = ["0.0.0.0/0"]
        }

        egress {
            description      = "Allow outgoing traffic"
            from_port        = 0
            to_port          = 0
            protocol         = "-1"
            cidr_blocks      = ["0.0.0.0/0"]
        }

        tags = {
            Name = "my-sg"
        }
        }

        resource "aws_lb" "my_alb" {
        name               = "my-alb"
        internal           = false
        load_balancer_type = "application"
        security_groups    = [aws_security_group.my_sg.id]
        subnets            = [aws_subnet.public_a.id, aws_subnet.public_b.id, aws_subnet.public_c.id]
        }

        resource "aws_lb_listener" "my_lb_listener" {
        load_balancer_arn = aws_lb.my_alb.arn
        port              = "80"
        protocol          = "HTTP"

        default_action {
            type             = "forward"
            target_group_arn = aws_lb_target_group.my_tg.arn
        }
        }

        resource "aws_lb_target_group" "my_tg" {
        name     = "my-tg"
        target_type = "instance"
        port     = 80
        protocol = "HTTP"
        vpc_id   = aws_vpc.main.id
        }

        resource "aws_launch_template" "my_launch_template" {

        name = "my_launch_template"
        
        image_id = "ami-06878d265978313ca"
        instance_type = "t2.micro"
        
        user_data = filebase64("${path.module}/server.sh")

        block_device_mappings {
            device_name = "/dev/sda1"

            ebs {
            volume_size = 8
            volume_type = "gp2"
            }
        }

        network_interfaces {
            associate_public_ip_address = true
            security_groups = [aws_security_group.my_sg.id]
        }
        }

        resource "aws_autoscaling_group" "my_asg" {
        name                      = "my_asg"
        max_size                  = 5
        min_size                  = 2
        health_check_type         = "ELB"
        desired_capacity          = 2
        target_group_arns = [aws_lb_target_group.my_tg.arn]

        vpc_zone_identifier       = [aws_subnet.public_a.id, aws_subnet.public_b.id, aws_subnet.public_c.id]
        
        launch_template {
            id      = aws_launch_template.my_launch_template.id
            version = "$Latest"
        }
        }

        resource "aws_autoscaling_policy" "scale_up" {
        name                   = "scale_up"
        policy_type            = "SimpleScaling"
        autoscaling_group_name = aws_autoscaling_group.my_asg.name
        adjustment_type        = "ChangeInCapacity"
        scaling_adjustment     = "1"    # add one instance
        cooldown               = "300"  # cooldown period after scaling
        }

        resource "aws_cloudwatch_metric_alarm" "scale_up_alarm" {
        alarm_name          = "scale-up-alarm"
        alarm_description   = "asg-scale-up-cpu-alarm"
        comparison_operator = "GreaterThanOrEqualToThreshold"
        evaluation_periods  = "2"
        metric_name         = "CPUUtilization"
        namespace           = "AWS/EC2"
        period              = "120"
        statistic           = "Average"
        threshold           = "50"
        dimensions = {
            "AutoScalingGroupName" = aws_autoscaling_group.my_asg.name
        }
        actions_enabled = true
        alarm_actions   = [aws_autoscaling_policy.scale_up.arn]
        }

        resource "aws_autoscaling_policy" "scale_down" {
        name                   = "asg-scale-down"
        autoscaling_group_name = aws_autoscaling_group.my_asg.name
        adjustment_type        = "ChangeInCapacity"
        scaling_adjustment     = "-1"
        cooldown               = "300"
        policy_type            = "SimpleScaling"
        }

        resource "aws_cloudwatch_metric_alarm" "scale_down_alarm" {
        alarm_name          = "asg-scale-down-alarm"
        alarm_description   = "asg-scale-down-cpu-alarm"
        comparison_operator = "LessThanOrEqualToThreshold"
        evaluation_periods  = "2"
        metric_name         = "CPUUtilization"
        namespace           = "AWS/EC2"
        period              = "120"
        statistic           = "Average"
        threshold           = "30"
        dimensions = {
            "AutoScalingGroupName" = aws_autoscaling_group.my_asg.name
        }
        actions_enabled = true
        alarm_actions   = [aws_autoscaling_policy.scale_down.arn]
        }
    ```
- In this code, first we mention the provider which will be download the provider i.e aws modules and such when we run the below command. It also specifies our credentials that is used to access our aws account. Here, I configured the credentials through cli instead of hardcoding it.
- The resources created by the above code are:
    - VPC
    - Three public subnets in three different availability zones
    - Internet Gateway
    - Public Route table
    - Associate our public route table with all three public subnets
    - Security Group
    - Application Load Balancer that is publicly accessible
    -  Add the listener ports to the load balancer
    - Target group to forward the traffic of our load balancer
    - Launch template for our autoscaling group. Here, we mention the user-script and it calls from the server.sh script file
    - Autoscaling Group
    - Autoscaling Policies and CloudWatch Alarms to scale our instances based on the CPU utilization
    - 
- Run:
    ```
        terraform init
    ```
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
- Go to the Load Balancer and copy the DNS name and paste it in your browser and you'll be able to see your instances running.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/092/day92.JPG)

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

Deployed a highly available group of EC2 Instances with load balancer and autoscaling group using Terraform.

## Social Proof

[Blog](https://dev.to/aaditunni/deploy-a-highly-available-group-of-ec2-instances-using-terraform-2kn4)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7048279100389568512-WwkD?utm_source=share&utm_medium=member_desktop)
