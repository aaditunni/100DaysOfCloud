# Creating New EC2 Instance using Snapshot

## Introduction

Creation of an snapshot of an EC2 instance and launching a new EC2 instance using the AMI of that snapshot.

## Prerequisite

AWS free tier account.

## Use Case

EBS Snapshots are a point-in-time copy of your data, and can be used to enable disaster recovery, migrate data across regions and accounts, and improve backup compliance.

## Services Covered

- EC2

## Try yourself

### Step 1 — 
Launch an EC2 Instance.
 - Create and attach a Security Group allowing inbound traffic for SSH and HTTP from anywhere.
 - Use the following User data:
    ```
    #!/bin/bash

    sudo su

    yum update -y

    yum install httpd -y

    echo "<html><h1> Welcome to Aadit's Server </h1><html>" >> /var/www/html/index.html

    systemctl start httpd

    systemctl enable httpd
    ```
- Test the web server by navigating to the instances through its Public IP. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.1.JPG)

### Step 2 — 
Create a Snapshot of EC2 Instance. 
- Under Elastic Block Store, navigate to Snapshots and create Instance snapshot of your instance.
- Give a name and description for the snapshot and leave everything else as default.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.2.JPG)

### Step 3 — 
Create AMI with Snapshot. 
 - Select the created Snapshot, click on Actions and select 'Create image from snapshot'.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.3.JPG)

### Step 3 — 
Create an EC2 Instance using newly create AMI. 
- Navigate to AMIs and launch a new instance of type t2.micro using the AMI and add the following User data:
    ```
    #!/bin/bash -ex 

    sudo service httpd restart
    ```
- Attach the same Security group used for the previous EC2 Instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.4.JPG)


## ☁️ Cloud Outcome

When the new instance's status is running, navigate to the instance through its Public IP and you should get the same response as from the first Instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/007/day7.6.JPG)

## Social Proof

[Blog](https://dev.to/aaditunni/creating-new-ec2-instance-using-snapshot-2cmi)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7017464758677221376-JIK-?utm_source=share&utm_medium=member_desktop)
