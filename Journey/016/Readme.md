# Migrate from RDS MySQL to Aurora MySQL in near zero downtime

## Introduction

Migrate from Amazon RDS MySQL to Amazon Aurora MySQL with minimal downtime. Amazon RDS uses the MySQL DB engines' binary log replication functionality and updates made to the source MySQL DB instance are asynchronously replicated to the Aurora Read Replica.

## Prerequisite

AWS account (will cost you less than $1 provided you follow the steps in the tutorial and terminate your resources at the end of the tutorial).

## Services Covered

- CloudFormation
- EC2
- AWS RDS
- AWS Aurora

## Try yourself

### Step 1 — EC2 Key Pair and CloudFormation template
- Create an EC2 Key Pair by going to the EC2 console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.JPG)

- Download this [CloudFormation template](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/CloudFormation.yaml)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.JPG)

### Step 2 — Deploy CloudFormation Stack
- Go to CloudFormation console.
- Click Create Stack.
- Choose Template is ready.
- Click Choose file to upload the file.
- Upload the yaml file and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.1.JPG)

- Select the Key Pair created earlier.
- Give a MasterPassword and MasterUsername for your MySQL database.
- Select the Subnet and the VPC the subnet belongs to (use default VPC).
- Leave the rest as default configuration and click Next until you reach the review page and then click Submit to create the Stack.
- Wait for the CREATE_IN_PROGRESS State to show COMPLETE.
        
The CloudFormation.yaml Template will launch an Amazon EC2 instance of type t2.micro with latest Amazon Linux 2 OS, bootstrap Apache/PHP, and install a simple address book web application. The template will also create an Amazon RDS MySQL database instance in free tier, i.e. of type db.t2.micro and with no Multi-AZ setup or read replicas. The WebTier Security Group will allow only SSH and HTTP connections to the web server (EC2 instance), and the DBTier Security Group will only allow the WebTier Security Group to initiate database connections to the RDS DB instance over the TCP port 3306.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.2.JPG)

- Go to Outputs section and open the Website URL in another tab.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.3.JPG)

- Click on RDS tab, enter the credentials and database name and click Submit.
- The RDS Endpoint can be found in the Outputs tab in Cloudformation console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.4.JPG)

- Once connected, you'll be redirected to a simple address book displaying information from the database.
- Play around with it by adding, modifying, removing the data.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.5.JPG)

### Step 3 — Create Aurora Read Replica
- Go to RDS console, click on Databases on the left side menu and select your RDS Database.
 Click on Actions and select Create Aurora read replica.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.6.JPG)

 - Configure the Aurora Read Replica with default values and give a suitable name for the DB instance identifier.


![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.7.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.9.JPG)

- Wait for the Database Status to show Available.
- The source DB instance rdsdb Role changes to that of Master from the role of Instance.
- Aurora DB cluster is created with 2 instances - Writer and Reader.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.10.JPG)

- Click on the Aurora Writer instance and go to the Connectivity and Security tab and click on the security group.
- This will take you to the Aurora security group.
- Edit inbound rules and add a rule that allows the WebTier Security Group to initiate database connections to the RDS DB instance over the TCP port 3306.
    - Type: MySQL/Aurora
    - Source: WebTier Security Group  

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.17.JPG)

### Step 4 — Change RDS DB Instance to read-only mode
- Go to Parameter groups on left side menu and select the custom parameter group that was created by CLoudFormation template.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.11.JPG)

- Click on Parameter group actions and click Edit.
- Search for read-only in the search bar in the Parameter section.
- Change the value to 1.
- This converts the instance to read-only mode.
- Save changes. The effect takes place immediately and doesn't require reboot.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.12.JPG)

- At this point, the migration of database from source RDS MySQL DB instance to Aurora DB Cluster is complete.

### Step 5 — Promote Aurora Read Replica to Standalone Cluster and Stop/Delete RDS DB Instance
- Select the Aurora Replica cluster, click on Actions and select Promote.
- Choose Promote read replica.
- Click on the Cluster and go to Logs and Events tab and make sure a Promoted log is logged in Recent events.
- After the promotion is complete, the source RDS MySQL DB Instance and the Aurora Read Replica are unlinked an can safely stop or delete the RDS DB Instance.
- AWS recommends to take a snapshot of the production RDS DB Instance prior to deleting it but in this I am not going to do it.
- Select the RS DB (rdsdb), click on Actions and choose Stop temporarily or Delete. Do not check the boc for Snapshot.
- It will start automatically in 7 days if stopped temporarily.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.13.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.14.JPG)

### Step 6 — Redirect Application to use Standalone Aurora Cluster
- Go to your EC2 instance and SSH into it using the EC2 Instance Connect.
- Navigate into the directory by running this command.
    ```
        cd /var/www/html
    ```
- Modify the rds.conf.php file.
    - Run this command to open the file in a editor.
        ```
            sudo vi rds.conf.php
        ```
    - Type the letter 'I' to go into insert mode.
    - Replace the RDS_URL with the Aurora Cluster Writer Node DNS name (select the Aurora Cluster and go to Connectivity an Security tab and under Endpoints, copy the writer instance endpoint).
    - Save the file (press Esc key and then type :wq and hit enter to save and quit the file).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.15.JPG)



### Step 7 — 
 - Go to Outputs section and open the Website URL in another tab. Choose the RDS tab. The web server connects to the Aurora Standalone Cluster and displays the same address book information from the database.
- Confirm that you can also make the write transactions to this database hosted on Aurora Standalone Cluster by playing around with the address book and add/edit/remove content from your database

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/016/day16.16.JPG)


- Cleanup
    - Select the Aurora Reader instance, click on Actions, then Delete. .
    - Select the Aurora Writer instance, click on Actions, then Delete. Uncheck ‘Create final snapshot’ and ‘Retain automated backups’ (For production database you want to create the final snapshot in the event you need to restore the database). This will delete the Aurora DB Cluster as well.
    - Delete the EC2 Key Pair.
    - Delete the CLoudFormation Stack. This will delete all the other resources like EC2 instance, RDS DB, etc created by the stack.
.
## ☁️ Cloud Outcome

Learned how migrate RDS MySQL database to Aurora MySQL database cluster by using Aurora Read Replica to achieve a near zero downtime. Also learned how to stop writing to RDS MySQL DB instance after replication lag approaches zero, and to convert the Aurora Read Replica into a standalone Aurora MySQL DB cluster.

## Social Proof

[Blog](https://dev.to/aaditunni/migrate-from-rds-mysql-to-aurora-mysql-in-near-zero-downtime-2dm0)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7020824312173477889-xb2v?utm_source=share&utm_medium=member_desktop)
