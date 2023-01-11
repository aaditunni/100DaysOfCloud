# Create an Elastic Network Interface - Multiple IPs on an EC2

## Introduction

Create an additional elastic network interface and an additional elastic IP.  Attach these additional created resources with EC2, and use another security group with HTTP permission to test it.

## Prerequisite

AWS account.

## Services Covered

EC2

## Try yourself

### Step 1 — 
- Create a Security Group for the EC2 Instance that allows inbound HTTP traffic from anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.JPG)

- Create a Security Group for the Network Interface that allows inbound HTTP traffic from anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.1.JPG)

- Launch an EC2 instance that is free tier eligible. Use the security group created previously for EC2 instance. Keep everything else as default and add this User data:
    ```
        #!/bin/bash

        sudo su

        yum update -y

        yum install -y httpd

        systemctl start httpd

        systemctl enable httpd

        echo "<html> <h1> Hello, Welcome to my page. </h1> </ html>" > /var/www/html/index.html
    ```
- Remember the Availability Zone and the Subnet id.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.2.JPG)

### Step 2 — 
Go to Network Interfaces under Network and Security.
- Create a Network Interface and select the same subnet as the EC2 is in. Select the security group created before for the Network Interface.
- Click on actions, select Attach and attach it to the EC2 instance you created.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.3.JPG)

### Step 3 — 
Go to Elastic IPs under Network and Security.
- Create an Elastic IP by clicking on Allocate Elastic IP address.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.4.JPG)

### Step 3 — 
Go back to the Network Interface created and associate the Elastic IP to it.
- Select the Network Interface, click on actions and click Associate Address.
- Select the Elastic IP address allocated and the Private IP.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.5.JPG)

## ☁️ Cloud Outcome
We can see that IPv4 Public IP and the Elastic IP address are assigned to the same EC2 Instance. ie. multiple Public IPs for a single EC2 instance. Also, there will be 2 Private IPv4 addresses assigned.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.6.JPG)

Navigate to both the Public IP and the Elastic IP of the EC2 instance to validate that the server responses.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.7.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/009/day9.8.JPG)

Terminate the EC2 instance, delete the ENI and release the Elastic IP to cleanup.


## Social Proof

[Blog](https://dev.to/aaditunni/create-elastic-network-interface-multiple-ips-on-an-ec2-3189)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7018185848588251136-S9Zc?utm_source=share&utm_medium=member_desktop)
