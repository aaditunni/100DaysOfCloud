# Auto-Scaling with Network Load Balancer

## Introduction

Configure Auto Scaling to automatically launch EC2 instances using conditions described by CloudWatch alarms - CPU utilization.

## Prerequisite

AWS account.

## Use Case

 Network Load Balancer has been designed to handle sudden and volatile traffic patterns, making it ideal for load balancing TCP traffic. 

## Services Covered

EC2

## Try yourself

### Step 1 — Network Load Balancer and Target Group
- Goto the EC2 Dashboard.
- On left side menu, go to Load Balancer under Load Balancing.
- Click Create Load Balancer.
- Select Network Load Balancer.
- Give a name, scheme as Internet-facing and IPv4 IP address type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.JPG)

- In network mapping, select default VPC and all subnets.
- In Listeners and routing, click on Create target group link.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.3.JPG)

- Choose the target group as instances, give it a name, Protocol - TCP, Port - 80 and Health checks Interval set to 10 seconds.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.4.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.6.JPG)

- Create the target group.
- Back in NLB creation tab choose the newly created target group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.7.JPG)

- When the load balancer is created select it and from Actions drop-down menu choose Edit attributes. Then enable Cross-Zone Load Balancing.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.9.JPG)

- Go to Target Groups, choose the earlier created group and change it's Deregistration delay to 30 seconds under Attributes tab.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.11.JPG)

### Step 2 — Security Group and Launch Template
- Go to Security Groups and create a new security group for the instances.
- Add inbound rules for both SSH and HTTP from anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.12.JPG)

Create Launch Template.
- On left side menu, under Instances, select Launch Templates and click Create launch template.
- Give a name, choose Amazon Linux 2 as AMI, t2.micro for instance type, create a Key Pair and attach the previously created security group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.13.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.14.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.15.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.16.JPG)

- In Advanced details, Enable Detailed CloudWatch monitoring and in User Data paste the following code :
    ```
    #!/bin/bash

    # Enable the epel-release
    sudo amazon-linux-extras install epel

    # Install and start Apache web server
    sudo yum install -y httpd php

    # Start the httpd service
    service httpd start

    # Install CPU stress test tool
    sudo yum install -y stress
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.17.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.18.JPG)

- This bash script installs PHP, an Apache web server (httpd), and a tool for stress testing called Stress.
- Create Launch Template.

### Step 3 — Auto-Scaling Group
- On left side menu, under Auto Scaling, select Auto Scaling Groups and click Create Auto Scaling group.
- Give a name, select the launch template created earlier and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.19.JPG)

- Select the Subnets chosen for the Load Balancer and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.20.JPG)

- Under Load Balancing, select Attach to an existing load balancer and select the one created earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.21.JPG)

- Under Health check grace period, enter 80.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.22.JPG)

- Set Maximum capacity to 4.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.23.JPG)

- Scaling policies: choose Target tracking scaling policy.
- In the Instances need text-box, enter 80.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.24.JPG)

- Keep clicking Next until you Create the Auto Scaling Group.

### Step 4 — Testing and Cleanup
- Goto Load Balancer and copy and paste the DNS name in a browser tab.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.25.JPG)

- Terminate that running instance, the auto scaling group will detect the changes and initiate a new Instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.26.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.27.JPG)

- SSH to that new instance and run following command to run stress
    ```
    stress --cpu 2 --io 1 --vm 1 --vm-bytes 128M --timeout 5m
    ```
- This will cause a CPU utilization spike which will trigger the Auto Scaling Group to initiate additional instance. You can follow the CPU utilization in CloudWatch detailed monitoring.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.28.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/043/day43.29.JPG)

Delete Auto Scaling Group, Network Load Balancer, Target Group, Key Pairs, Security Groups and Launch Template to cleanup.

## ☁️ Cloud Outcome

Created an Auto-Scaling Group with Network Load Balancer.

## Social Proof

[Blog](https://dev.to/aaditunni/auto-scaling-with-network-load-balancer-52jf)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7030488230126891009-rEYT?utm_source=share&utm_medium=member_desktop)