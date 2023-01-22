![placeholder image](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22cover.jpg)

# Create an EC2 instance in a private subnet to communicate with the Internet.

## Introduction

Create a VPC along with a public subnet and a private subnet. Attach an Internet Gateway to the VPC route the traffic from the public subnet to the Internet hence making it a public subnet an then attach a NAT Gateway to the public subnet and route the traffic from the private subnet to the NAT Gateway. This will let any resources in the private subnet to access to the internet but won't let in any incoming initiations from the Internet. 

## Prerequisite

AWS account.

## Use Case

NAT Gateway is best used when we need one-way communication from subnet to the internet. The main use case of NAT gateway is to provide internet access to private subnets so that, it can interact with repo’s “downloading packages”, “updating security patches”, etc.

## Services Used

- VPC
- EC2

## Try yourself

### Step 1 — VPC and Subnets
Create a VPC.
- Search VPC and click Create VPC.
- In Resources to create, select VPC only.
- Give a name.
- Select IPv4 CIDR manual input and enter 10.0.0.0/16 .
- Select No IPv6 CIDR block Tenancy as Default.
- Create VPC.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.JPG)

Create a Public and a Private Subnet.
- Click on Subnets on left side in VPC console.
- Click Create Subnet.
- For VPC ID, select the VPC that was created earlier.
- In Subnet settings, give a subnet name indicating that it is a public subnet.
- Choose an Availability Zone.
- Enter 10.0.0.0/24 for IPv4 CIDR block.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.1.JPG)

- Add a new Subnet and give a subnet name indicating that it is a private subnet.
- Choose an Availability Zone.
- Enter 10.0.1.0/24 for IPv4 CIDR block.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.2.JPG)

### Step 2 — Route table, Internet Gateway and NAT Gateway
Create an Internet Gateway.
- Go to VPC and select Internet gateway on left side.
- Click Create internet gateway.
- Click on Actions and select Attach to VPC and select the VPC that was created earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.3.JPG)

Create a new Route table.
Public and Private Subnet are associated to the same Route table which is the main Route table.
- Got to Route tables on left side of VPC console.
- Create a new Route table.
- Give a name - route_table_private.
- Select VPC created earlier and create the Route table.
- Under Explicit subnet association, select the Private subnet and Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.4.JPG)

By doing this, the public subnet will be the only subnet associated with the main Route table. Rename the main route table as route_table_public.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.5.JPG)

- Select the Public route table.
- Under Routes, Click Edit route.
- Add Route > Destination : 0.0.0.0/0 and Target : choose the Internet Gateway and select the Internet Gateway that was created earlier.
- Save changes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.6.JPG)

So far, we set up 2 Subnets. One Public and one Private,  attached an Internet Gateway to the VPC and routed the traffic in public subnet to the Internet Gateway. Any resources in the public subnet can talk to the Internet.

Now we need to Add a NAT Gateway in the public subnet and route the private subnet to this NAT GW so that the traffic from private subnet is routed to the NAT GW which in turns route it to the Internet Gateway as both are in the public subnet and hence allowing communication to the Internet.

Create an NAT Gateway.
- Go to VPC and select NAT gateway on left side.
- Click Create NAT gateway.
- Give a name, select the public subnet, choose connectivity type as Public and click on Allocate Elastic IP to allocate one.
- Create the NAT GW.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.7.JPG)

- Go to the Private route table.
- Under Routes, Click Edit route.
- Add Route > Destination : 0.0.0.0/0 and Target : choose the NAT Gateway and select the NAT Gateway that was created earlier.
- Save changes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.8.JPG)

NAT GW allows resources within a private subnet access to the Internet but NAT GW itself will block all incoming initiatives from the Internet.

### Step 3 — EC2 instances
Create an EC2 instance in the public subnet.
- Select the AMI (Amazon Linux 2) and instance type (t2.micro) that is free tier eligible. 
- Create a Key Pair.
- Under Network Settings, select the VPC and the Public Subnet that was created earlier.
- Enable Auto-assign public IP.
- Create a Security group that allows SSH from anywhere in source type in inbound rules.
- Launch Instance.

Create an EC2 instance in the private subnet.
- Select the AMI (Amazon Linux 2) and instance type (t2.micro) that is free tier eligible. 
- Choose the same Key Pair.
- Under Network Settings, select the VPC and the Private Subnet that was created earlier.
- Disable Auto-assign public IP.
- Create a Security group that allows SSH from the security group of the public EC2 instance in source type in inbound rules.
- Launch Instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.9.JPG)

Connect and test.
- Connect to the public EC2 instance using EC2 Instance Connect. 
- You can do this in your local machine instead of using Instance Connect if you have Mac or Linux based OS. 
- If you are running in your local machine then you can skip the next 3 steps.
- Create a file by running this command (change key_pair_name with the name of your Key Pair):
    ```
        vim key_pair_name.pem
    ```
- Open the Key pair that was downloaded to your local machine using a text editor and copy it.
- Type i in the vim editor and paste it.
- Save it by typing :wq and click Enter.
- Change the key permissions by running this command (change key_pair_name with the name of your Key Pair):
    ```
        chmod 400 key_pair_name.pem
    ```
- Connect into the private instance (change key_pair_name with the name of your Key Pair and private_ip with the private ip address of the private instance) :
    ```
        ssh -i "key_pair_name.pem" ec2-user@private_ip
    ```
- Ping 8.8.8.8 to check if you can access the Internet.
- You can notice that the private instance can access the internet. If you remove the NAT gateway frm the route table or delete it then the private instance won't be able to talk with the Internet.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/022/day22.10.JPG)

### Step 4 — Cleanup
- Terminate EC2 instances.
- Delete Security groups and Key Pair.
- Delete NAT Gateway.
- Release the Elastic IP
- Detach the Internet gateway and then delete it.
- Delete the Subnets.
- Delete the VPC. The route tables will be deleted automatically.

## ☁️ Cloud Outcome

Created an EC2 instance in a private subnet to communicate with the Internet through a Bastion Host.

## Social Proof

[Blog](https://dev.to/aaditunni/create-an-ec2-instance-in-a-private-subnet-to-communicate-with-the-internet-555m)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7022977858859851776-tUcz?utm_source=share&utm_medium=member_desktop)