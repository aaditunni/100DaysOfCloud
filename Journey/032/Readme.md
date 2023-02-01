# Amazon EC2 Auto Scaling with EC2 Spot Instances

## Introduction

 Create a stateless, fault tolerant workload using Amazon EC2 Auto Scaling with launch templates to request Amazon EC2 Spot Instances. Amazon EC2 Auto Scaling allows creation of collections of instances known as Amazon EC2 Auto Scaling groups (ASGs) and will maintain, scale-up, or scale-down the number of instances needed based on scaling policies that are defined based on the demands of that particular application.y.

## Prerequisite

AWS account.

## Use Case

Spot Instances are recommended for stateless, fault-tolerant, flexible applications. Amazon EC2 Spot Instances are spare capacity available at steep discounts compared to the EC2 On-Demand price, typically between 70% and 90% off.

## Services Covered

EC2

## Try yourself

### Step 1 — Create Application Load Balancer, Security Group for ALB and Target Group.
- Open EC2 Dashboard.
- On left side menu, go t0 Load Balancer under Load Balancing.
- Click Create Load Balancer.
- Select Application Load Balancer.
- Give a name, scheme as Internet-facing and IPv4 IP address type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.JPG)

- In network mapping, select default VPC and all the subnets.
- Under Security groups, remove the existing group attached and create and new security group that allows all inbound HTTP traffic from anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.3.JPG)

- Back in the Load Balancer creation page, click on the refresh icon next to security groups and select the security group that was created.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.4.JPG)

- In Listeners and routing, click on Create target group link. Use default configurations and give it a name and create the target group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.5.JPG)

- For Health Checks, leave the protocol as HTTP and the path as / .

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.6.JPG)

- Expand the Advanced health check settings and change the Healthy threshold to 2 and the Interval to 10. Leave the other settings as they are. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.7.JPG)

- Back in the Listens and routing section, select the target group created. Listener should be set as HTTP on Port 80.
- Leave the other settings as they are.
- Click Next: Register Targets.
- No changes are needed on Step 5: Register Targets.
- After looking over Step 6: Review, click Create.

### Step 2 — Create Launch Template.
- On left side menu, under Instances, select Launch Templates and click Create launch template.
- Give a Launch Template Name and Template Version Description for this template.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.8.JPG)

- Choose Amazon Linux 2 AMI (HVM), SSD Volume Type. (ensure you choose the x86 version, and not ARM).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.9.JPG)

- Under Network settings, create a new security group that allows inbound HTTP on port 80 with source as the security group of the load balancer.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.21.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.10.JPG)

- In Advanced details, paste the following code :
    ```
    #!/bin/bash
    yum install httpd -y
    systemctl start httpd
    systemctl stop firewalld
    cd /var/www/html
    echo "this is my test site and the instance-id is " > index.html
    curl http://169.254.169.254/latest/meta-data/instance-id >> index.html
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.11.JPG)

- Create Launch Template.


### Step 3 — Create Auto Scaling Group.
- On left side menu, under Auto Scaling, select Auto Scaling Groups and click Create Auto Scaling group.
- Give a name, select the launch template created earlier and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.12.JPG)

- Under Network, select the default VPC and all the subnets.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.13.JPG)

- Under Instance type requirements, select Manually add instance types.
- Choose c5.large as the Primary instance type. Additional instance types are automatically selected and default to Family and generation flexible.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.14.JPG)

- In Instance purchase option, set the % On Demand to 10%. As this is done, the % Spot will automatically adjust to 90%. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.15.JPG)

- Select On-Demand allocation strategy as prioritized.
- In Spot allocation strategy, select the recommended price capacity optimized and click Next. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.16.JPG)

- In Configure advanced options, under Load balancing, select Attach to an existing load balancer.
- Select Choose from your existing load balancer target group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.17.JPG)

- Select the target group created.
- Under the Health Checks section, lower the Health Check Grace Period to 120 seconds and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.18.JPG)

- Set the Desired Capacity to 12, the Minimum Capacity to 6, and the Maximum Capacity to 12.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.19.JPG)

- Choose Target tracking scaling policy and leave the default settings of 50% Average CPU Utilization.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.20.JPG)

- Skip to review and create the ASG.

### Step 4 — Testing
- Go back to the Load Balancing console here. Find the DNS name for the Load Balancer created. Click on the copy icon next to the A record to load it into the clipboard and then paste it into a web browser.
- A simple website will be shown in the browser. Keep clicking refresh – each time you do this, the load balancer is sending your request to a different instance and you should see the instance-id changing with each request. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.24.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.25.JPG)

- In EC2 instance console, click on the gear settings icon on the upper right – scroll down through the Instance Attributes and choose Lifecycle. Click Confirm and Close. Note you can now see which instances are Spot easily by scrolling all the way to the right.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.22.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/032/day32.23.JPG)

- Play around with the instances by terminating them.
- The website will continue to work as long as you have some instances running. EC2 Auto Scaling will detect that the instances have failed or been terminated and will replace them with fresh instances and automatically add them back to the load balancer within a short period of time, while maintaining an even spread across all of the Availability Zones.


### Step 5 — Cleanup
- Delete the Auto Scaling group.
- Delete the Load Balancer.
- Delete the Target group.
- Delete the launch template.
- Delete the Security Groups.

## ☁️ Cloud Outcome

 Learned how to create an Auto Scaling group using EC2 Spot Instances with launch templates. Auto Scaling with load balancing will keep the stateless, fault-tolerant workload such as a web application running even after some instances are interrupted or experience other issues. 

## Social Proof

[Blog](https://dev.to/aaditunni/amazon-ec2-auto-scaling-with-ec2-spot-instances-3hbb)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7026617437609467904-dJFP?utm_source=share&utm_medium=member_desktop)