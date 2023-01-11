![placeholder image](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/public-subnet-traffic.png)

# Create a VPC with resources to communicate with the Internet

## Introduction

A VPC with a Public Subnet and an Internet Gateway that allows an EC2 Instance to communicate with the internet.

## Prerequisite

AWS free tier account.

## Services Covered

- EC2
- VPC

## Try yourself

### Step 1 — 
Create a VPC.
- Search VPC and click Create VPC.
- In Resources to create, select VPC only.
- Give a name.
- Select IPv4 CIDR manual input and enter 10.0.0.0/16 .
- Select No IPv6 CIDR block Tenancy as Default.
- Create VPC.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/day11.JPG)

### Step 2 — 
Create a Subnet.
- Click on Subnets on left side in VPC console.
- Click Create Subnet.
- For VPC ID, select the VPC that was created earlier.
- In Subnet settings, give a subnet name indicating that it is a public subnet.
- Choose an Availability Zone.
- Enter 10.0.0.0/24 for IPv4 CIDR block.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/day11.1.JPG)

### Step 3 — 
Create an EC2 instance.
- Select the AMI and OS that is free tier eligible. 
- Create a Key Pair.
- Under Network Settings, select the VPC and Subnet that was created earlier.
- Enable Auto-assign public IP.
- Create a Security group. Remove existing rule andd only add the rule that allows ALL ICMP-IPv4, from anywhere in source type in inbound rules.
- Launch Instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/day11.2.JPG)

- Copy the Public IPv4 address and ping it from your computer. You can notice that all the packets that sent was lost. This is because we haven't configured an Internet Gateway to route the traffic through the Internet.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/day11.3.JPG)

### Step 4 — 
Create an Internet Gateway.
- Go to VPC and select Internet gateway on left side.
- Click Create internet gateway.
- Click on Actions and select Attach to VPC and select the VPC that was created earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/day11.4.JPG)

### Step 5 —
Edit the Route table.
- Got to Route tables on left side of VPC console.
- Select the Route table of the VPC we created earlier.
- Under Routes, Click Edit route.
- Add Route > Destination : 0.0.0.0/0 and Target : choose the Internet Gateway and select the Internet Gateway that was created earlier.
- Save changes.

## ☁️ Cloud Outcome

Ping into the EC2 instance from your computer and you'll get reply from the Instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/011/day11.5.JPG)

Successfully created a VPC with an EC2 instance in a Public Subnet with Internet Gateway to route traffic through the Internet.

Terminate the EC2 instance, delete Key Pairs, delete Security group, detach and delete Internet Gateway, delete Subnet and then delete VPC to cleanup.

## Social Proof

[Blog](https://dev.to/aaditunni/create-a-vpc-with-resources-to-communicate-with-the-internet-1abf)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7018917549073444865-iso7?utm_source=share&utm_medium=member_desktop)
