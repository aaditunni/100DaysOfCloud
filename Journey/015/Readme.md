# Amazon EC2 - Auto Scaling Groups - Application Load Balancer

## Introduction

Create an Auto Scaling Group (ASG) and place it behind an Application Load Balancer (ALB) and deploy Instances with a Target Group. The Instance will be stressed to cause spike in CPU utilization above the threshold which will cause the Health checks to fails and replace and auto-scale accordingly with healthy ones.

## Prerequisite

AWS free tier account.

## Services Covered
- EC2
    -  Auto Scaling Group (ASG)
    - Application Load Balancer (ALB)

## Try yourself

### Step 1 — 
Create Application Load Balancer, Security Group for ALB and Target Group.
- Open EC2 Dashboard.
- On left side menu, go t Load Balancer under Load Balancing.
- Click Create Load Balancer.
- Select Application Load Balancer.
- Give a name, scheme as Internet-facing and IPv4 IP address type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.JPG)

- In network mapping, select default VPC and at least to subnets.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.1.JPG)

- Under Security groups, remove the existing group attached and create and new security group that allows all inbound HTTP traffic from anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.2.JPG)

- Back in the Load Balancer creation page, click on the refresh icon next to security groups and select the security group that was created.
- In Listeners and routing, click on Create target group   link. Use default configurations and give it a name and create the target group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.3.JPG)

- Back in the Listens and routing section, select the target group created. Listener should be set as HTTP on Port 80.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.4.JPG)

### Step 2 — 
Create Security Group for EC2 Instances.
- Go to Securtiy Groups and create a new security group for the instances. 
- Add inbound rules for both SSH and HTTP from anywhere.


![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.5.JPG)

### Step 3 — 
Create Launch Template.
- On left side menu, under Instances, select Launch Templates and click Create launch template.
- Give a name, choose Amazon Linux 2 as AMI, t2.micro for instance type, create a Key Pair an attach the previously created security group for EC2.
- In Advanced details, paste the following code :
    ```
        #!/bin/bash

        sudo amazon-linux-extras install epel

        sudo yum install -y httpd php

        service httpd start

        sudo yum install -y stress
    ```
- Create Launch Template.

### Step 4 — 
Create Auto Scaling Group.
- On left side menu, under Auto Scaling, select Auto Scaling Groups and click Create Auto Scaling group.
- Give a name, select the launch template created earlier and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.6.JPG)

- Select the Subnets chosen for the Load Balancer and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.7.JPG)

- Under Load Balancing, select Attach to an existing load balancer and select the one created earlier.
- Under Health Checks - optional, check the ELB box and change Health grace period to 120 seconds and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.8.JPG)

- Under Group Size, change Max Capacity to 4.
- Under Scaling policies, choose Target tracking scaling policy and Metric Type as Average CPU utilization and Target value as 50.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.9.JPG)

- Keep clicking Next until you Create the Auto Scaling Group.

- Goto Load Balancer and copy and paste the DNS name in a browser tab.
- SSH into the EC2 instance sing EC2 Instance Connect and run the following command:
    ```
        stress --cpu 2 --io 1 --vm 1 --vm-bytes 128M --timeout 5m
    ```
- This will run the stress test to increase CPU utilization.
- New EC2 instances will be created.

Delete Application Load Balancer, Auto Scaling Group, Target Group, Key Pairs, Security Groups and Launch Template to cleanup.

## ☁️ Cloud Outcome

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.12.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/015/day15.11.JPG)

The increase in CPU utilization caused the Health check to fail thereby terminating it and at the same time creation of new EC2 instance using Auto Scaling Group and balancing the traffic through the Load Balancer.

## Social Proof

[Blog](https://dev.to/aaditunni/amazon-ec2-auto-scaling-groups-application-load-balancer-37j1)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7020349463672070144-SAeq?utm_source=share&utm_medium=member_desktop)