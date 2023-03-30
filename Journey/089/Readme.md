# Layered Security in a VPC

## Introduction

Create a multi layered VPC security and launch 2 EC2 instances.

## Prerequisite

AWS free tier account.

## Services Covered

- VPC
- EC2

## Try yourself

### Step 1 —
- Go to the VPC console.
- Create a new VPC. 
    - For the IPV4 CIDR block enter: 10.0.0.0/16, no IPV6 needed, tenancy: Default.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.JPG)

- Create Internet Gateway (select it from left side menu) and attach it to the VPC.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.2.JPG)

- Create 2 Subnets.
    - One public and one private.
    - Public one should provide 10.0.1.0/24 as IPv4 CIDR block, no preference for AZs. 
    - For the private one enter 10.0.2.0/24 for the CIDR block, no preference for AZs.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.3.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.4.JPG)

- Create 2 route tables
    - One for public routes and another for private routes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.6.JPG)

   - Select Public Route Table and then click on Routes in the bottom window --> Click on Edit Routes --> Click on Add Route --> Destination → 0.0.0.0/0 and in Target → IGW select the Internet Gateway and choose our Internet Gateway (leave the existing route ).- Click Save Routes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.7.JPG)

   - Associate the Public subnet with this Public Route Table. Select Public_Route_Table --> Click on the Subnets associations Tab in the bottom window --> Click on Edit subnet Association --> Select the public subnet --> Click on Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.8.JPG)
    
   - Similarly associate private Route Table with private subnet.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.9.JPG)

- Create a security group which will provide security at the instance level.
    - Under Inbound Rules, click on Add Rule to allow SSH and ICMP traffic from IPv4 anywhere.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.10.JPG)

- Create Network ACL. Give a name and select the new VPC.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.11.JPG)

   - Select your NACL and then in the bottom window click on Inbound Rules → Edit Inbound Rules and then click on the Add Rule to allow traffic for SSH (Rule number 100) and ICMP (Rule number 200) with Source 0.0.0.0/0.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.12.JPG)

   - Click on Outbound Rules → Edit Outbound Rules and then click on the Add Rule to allow traffic for SSH (Rule number 100), ICMP (Rule number 200) and Custom TCP (Port range 1024-65535) with Source 0.0.0.0/0.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.13.JPG)

   - Associate both public and private subnets with this NACL. Select the NACL → Click on Subnet Association in the bottom window → Edit the Subnet association → Select both subnets we created and click on Edit.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.14.JPG)

- Launch two EC2 instance.
    - One in public subnet and one in private one. Under Network settings, select the new VPC, public subnet for public EC2 instance an private subnet for private EC2 instance, Enable auto-assign public IP and select the security group created for EC2 instance.
    - Leave everything as default configurations.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.15.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.16.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.17.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.18.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.19.JPG)

- SSH to the public instance. Ping to the private IP of your private instance by using the below command:
    ```
        ping < Private EC2 private IP address >
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/089/day89.20.JPG)

### Step 2 — Cleanup
- Terminate the EC2 instances.
- Delete the VPC and everything associated with it wil be automatically delete.

## ☁️ Cloud Outcome

Created a multi layered VPC security and launched 2 EC2 instances.

## Social Proof

[Blog](https://dev.to/aaditunni/layered-security-in-a-vpc-5812)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7047303901083049984-vnab?utm_source=share&utm_medium=member_desktop)
