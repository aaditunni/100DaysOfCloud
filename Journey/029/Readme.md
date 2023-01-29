# Enabling Access Logs for Application Load Balancer

## Introduction

Create an Application Load Balancer for two Instances with Apache server on them. The ELB will create Access logs into an S3 bucket.

## Prerequisite

AWS free tier account.

## Use Case

Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses. You can use these access logs to analyze traffic patterns and troubleshoot issues.

## Services Covered

- EC2
- S3

## Try yourself

### Step 1 — 
- Launch an EC2 Instance of type Amazon Linux 2 - t2.micro.
- Continue without a Key Pair.
- Create a Security Group that allows inbound traffic for HTTP.
- Add this to User Data in Additional settings:
    ```
    #!/bin/bash
    sudo su
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "Response coming from server A" > /var/www/html/index.html
    ```
- Similarly create another instance with same security group and add this in User Data:
    ```
    #!/bin/bash
    sudo su
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "Response coming from server B" > /var/www/html/index.html
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.JPG)

### Step 2 — 
- On left side menu, go t Load Balancer under Load Balancing.
- Click Create Load Balancer.
- Select Application Load Balancer.
- Give a name, scheme as Internet-facing and IPv4 IP address type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.1.JPG)

- In network mapping, select default VPC and at least to subnets.
- Under Security groups, remove the existing group attached and select the same security group used for the EC2 instances.
- In Listeners and routing, click on Create target group link. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.2.JPG)

- Use default configurations and give it a name, select both the instances an click Include as pending below and create the target group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.3.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.4.JPG)

- Back in the ALB creation page, select the created Target group.
- Create the Load Balancer.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.5.JPG)

### Step 3 — 
- Go to the ALB.
- Under Attributes tab, click on Edit in the Attributes section.
- In Monitoring, switch on Access logs.
- Click on View so that it opens the S3 console.
- Create a S3 bucket for the ELB to store the access logs in.
- Give a bucket name an leave everything else as default configurations.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.6.JPG)

- Go to the Bucket and under Permissions tab, click on Edit in the Bucket Policy section and add this:
    - Replace elb-account-id with 127311923021 if the Region is US East (N. Virginia). If your Region is different then find the id from [here](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/enable-access-logging.html): 
    - Replace bucket-name/prefix with your bucket name and where in the bucket you want to store the files.
    - Replace your_aws-account-id with your account id.

    ```
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::elb-account-id:root"
        },
        "Action": "s3:PutObject",
        "Resource": "arn:aws:s3:::bucket-name/prefix/AWSLogs/your-aws-account-id/*"
        }
    ]
    }
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.6.5.JPG)

- Back in the ELB console, click Browse and select the S3 bucket created and Save the settings.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.7.JPG)

### Step 4 — 
- Use the ELB DNS name to access the load balancer in another browser tab.
- You can see the load balancing between the two EC2 instances.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.9.JPG)

- Check the S3 bucket to see the Access Logs stored.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/029/day29.10.JPG)

- Delete Application Load Balancer, Target Group, empty and delete S3 bucket, terminate EC2 instances and delete Security group to cleanup.

## ☁️ Cloud Outcome

Stored the ALB Access Logs in a S3 bucket.

## Social Proof

[Blog](https://dev.to/aaditunni/enabling-access-logs-for-application-load-balancer-2o6l)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7025453863272914944-qsj_?utm_source=share&utm_medium=member_desktop)