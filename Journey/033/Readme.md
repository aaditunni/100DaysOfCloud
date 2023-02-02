# Path-based routing on an Application Load Balancer

## Introduction

A path-based routing on an Application Load Balancer where the default routing is to a target group named main with two instances assuming it as the instances that host the main page. There are two other target groups names football and cricket with two instances each assuming these two are instances that hosts for football and cricket page. When the ALB url has a path '/football' or '/cricket', it routes it to the football or the cricket webpage respectively.

## Prerequisite

AWS free tier account.

## Services Covered

EC2

## Cloud Research

I had tried to do this a few times but always ended up getting some sort of errors. The webpage never loaded when I add the path to the url. I tried to figure out and solve it but always failed and ended up doing something else as I was short on time. Today, I decided to do it no matter what and find the problem. 
I figured that if the instances in the target group that are routed based on path have their index.html file in a directory named after the path name in /var/www/html, then it works perfectly.
For example, I have a target group that routes based on path value '/football'. I created a directory in /var/www/html named football and moved the index.html file to that directory.

## Try yourself

### Step 1 — Create Security Group for Load Balancer and EC2 instances.
Load Balancer
- Go to Security Groups and create a new security group for the instances.
- Add inbound rules for both HTTP from anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.JPG)

EC2 Instances.
- Create a new security group for the instances.
- Add inbound rules for both SSH and HTTP with source as security group of load balancer.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.1.JPG)

Go back to the security group of load balancer and in outbound rules, add HTTP with source as security group of EC2 instances.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.1.5.JPG)

### Step 2 — Launch EC2 Instances.
- Click on Launch Instances in Instance dashboard.
- Give a name - Main, choose Amazon Linux 2 as AMI, t2.micro for instance type, continue without a Key Pair and attach the previously created security group for EC2.
- For number of instances on the right hand side, enter 2.
In Advanced details, under User Data, paste the following code :
    ```
    #!/bin/bash
    # Use this for your user data (script from top to bottom)
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from Main</h1>" > /var/www/html/index.html
    ```
- Similarly repeat the above steps and create two more instances with the Name - Football and in Advanced details, under User Data, paste the following code :
    ```
    #!/bin/bash
    # Use this for your user data (script from top to bottom)
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from Football</h1>" > /var/www/html/index.html
    cd /var/www/html
    sudo mkdir football
    sudo mv index.html football/index.html
    ```    
- Again repeat the above steps and create two more instances with the Name - Cricket and in Advanced details, under User Data, paste the following code :
    ```
    #!/bin/bash
    # Use this for your user data (script from top to bottom)
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from Cricket</h1>" > /var/www/html/index.html
    cd /var/www/html
    sudo mkdir cricket
    sudo mv index.html cricket/index.html
    ```    

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.2.JPG)

### Step 3 — Create Target Groups.
Target group for main
- Go to target group dashboard.
- Click on Create target group.
- For Choose a target type, choose Instances.
- Give a name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.3.JPG)

- Leave the rest all as default configurations and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.4.JPG)

- Under Register target, choose the instances that are named main and click include as pending below.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.6.JPG)

- Similarly make another target group for football with football instances as target. Under Health checks, for Health check path give it as /football  .

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.7.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.9.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.10.JPG)

- Make another target group for cricket with cricket instances as target. Under Health checks, for Health check path give it as /cricket  .

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.11.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.12.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.13.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.14.JPG)

### Step 4 — Create an Application Load Balancer.
- EC2 Dashboard.Open 
- On left side menu, go to Load Balancer under Load Balancing.
- Click Create Load Balancer.
- Select Application Load Balancer.
- Give a name, scheme as Internet-facing and IPv4 IP address type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.15.JPG)

- In network mapping, select default VPC and at least to subnets.
- Under Security groups, remove the existing group attached and add the security group created for the load balancer.
- In the Listens and routing section, select the target group created for main. Listener should be set as HTTP on Port 80.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.16.JPG)

### Step 5 — Edit listener rules in ALB.
- Go to the ALB created.
- Under listeners, click on the listener. It will open a new page.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.17.JPG)

- Under Rules tab, click Manage rules.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.18.JPG)

- Click on the + icon.
- Click on Insert rule.
- Select Add condition and choose Path.
- Give the value as /football* and confirm.
- Click on Add action an select forward ti and select the football target group and confirm.
- Save the rule.
- Add another rule with path as /cricket* and forward it to cricket target group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.19.5.JPG)

### Step 6 — Testing and Cleanup.
- Copy the Application Load Balancer DNS name  and paste it in another tab and hit enter.
- This will open the Main webpage.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.19.JPG)

- Add /football or /cricket to the end of the url and hit enter. This will open the football or cricket webpage respectively.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.20.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/033/day33.21.JPG)

## ☁️ Cloud Outcome

Created an Application Load Balancer that routes traffic based on the Path.

## Social Proof

[Blog](https://dev.to/aaditunni/path-based-routing-on-an-application-load-balancer-3cib)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7026988327018901504-CrRI?utm_source=share&utm_medium=member_desktop)
