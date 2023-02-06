# Visualize data in Amazon RDS for Microsoft SQL Server using Amazon QuickSight

## Introduction

Create and visualize data in an Amazon Relational Database (Amazon RDS) MS SQL Express server using Amazon QuickSight.

## Prerequisite

- AWS free tier account.
- Download and install [Microsoft SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)
- QuickSight Account

## Services Covered

- RDS
- QuickSight

## Try yourself

### Step 1 — RDS
- Go to the RDS console and choose the Region.
- In the Create Database section, choose Create Database.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.JPG)

- Select Easy create.
- In the Configuration section, for Engine type, choose Microsoft SQL Server.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.1.JPG)

- For DB instance size, choose Free tier.
- For DB instance identifier, type 'day37' or anything else.
- For Master username, enter admin.
- For Master password, type a unique password, and confirm password.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.2.JPG)

- Leave the rest all as default configurations.
- Create database.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.3.JPG)

### Step 2 — 
- Select the database and click Modify.
- In the Connectivity section, choose Additional Configuration.
- Choose Publicly accessible, and choose Continue.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.4.JPG)

- In the Scheduling of modifications section, choose Apply immediately.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.5.JPG)

- Modify DB instance.
- Go to your database and in the Connectivity & security section, choose the VPC security groups link.
- Click on the Security group id.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.6.JPG)

- On the sg-default page, in the Inbound rules section, choose Edit inbound rules.
- Add the inbound rule:
    - For Type, choose All TCP from the drop-down list.
    - For Source, choose Anywhere IPv4
    - Save rules.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.7.JPG)

### Step 3 — Microsoft SQL Server Management Studio
- Open Microsoft SQL Server Management Studio.
- In the SQL Server pop up window, enter the following details.
    - For Server Name, paste the database Endpoint and Port separated by commas.
    - To find the endpoint, open the Amazon RDS console, and choose the database. In the Connectivity & Security section, copy the Endpoint and Port.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.8.JPG)

   - Authentication - SQL Server Authentication.
    - For Login, type the username you entered when creating the database.
    - For Password, type the password you entered when creating the database.
    - Connect.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.9.JPG)

- In the left-hand menu, choose Databases.
- Right click and choose Create Database.
- Database name - Visualize. Then, choose OK.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.10.JPG)

- Select Visualize, and click New Query.
- In the Query editor, copy and paste the following script :
    ```
    CREATE TABLE newhire(
    empno INT PRIMARY KEY,
    ename VARCHAR(10),
    job VARCHAR(9),
    manager INT NULL,
    hiredate DATETIME,
    salary NUMERIC(7,2),
    comm NUMERIC(7,2) NULL,
    department INT)
    begin
    insert into newhire values
        (1,'JOHNSON','ADMIN',6,'12-17-1990',18000,NULL,4)
    insert into newhire values
        (2,'HARDING','MANAGER',9,'02-02-1998',52000,300,3)
    insert into newhire values
        (3,'TAFT','SALES I',2,'01-02-1996',25000,500,3)
    insert into newhire values
        (4,'HOOVER','SALES I',2,'04-02-1990',27000,NULL,3)
    insert into newhire values
        (5,'LINCOLN','TECH',6,'06-23-1994',22500,1400,4)
    insert into newhire values
        (6,'GARFIELD','MANAGER',9,'05-01-1993',54000,NULL,4)
    insert into newhire values
        (7,'POLK','TECH',6,'09-22-1997',25000,NULL,4)
    insert into newhire values
        (8,'GRANT','ENGINEER',10,'03-30-1997',32000,NULL,2)
    insert into newhire values
        (9,'JACKSON','CEO',NULL,'01-01-1990',75000,NULL,4)
    insert into newhire values
        (10,'FILLMORE','MANAGER',9,'08-09-1994',56000,NULL,2)
    insert into newhire values
        (11,'ADAMS','ENGINEER',10,'03-15-1996',34000,NULL,2)
    insert into newhire values
        (12,'WASHINGTON','ADMIN',6,'04-16-1998',18000,NULL,4)
    insert into newhire values
        (13,'MONROE','ENGINEER',10,'12-03-2000',30000,NULL,2)
    insert into newhire values
        (14,'ROOSEVELT','CPA',9,'10-12-1995',35000,NULL,1)
    end
    CREATE TABLE department(
    deptno INT NOT NULL,
    dname VARCHAR(14),
    loc VARCHAR(13))
    begin
    insert into department values (1,'ACCOUNTING','ST LOUIS')
    insert into department values (2,'RESEARCH','NEW YORK')
    insert into department values (3,'SALES','ATLANTA')
    insert into department values (4, 'OPERATIONS','SEATTLE')
    end
    ```
- Execute to run.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.11.JPG)

### Step 4 — 
The database no longer needs to be publicly accessible; the previous script downloaded the required scripts from the client.
- Open the RDS console, in the left-hand menu, choose Databases and select the database.
- Click Modify.
- In the Connectivity section, choose Additional Configuration.
- Choose Not publicly accessible, and choose Continue.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.12.JPG)

- In the Scheduling of modifications section, choose Apply immediately.
- Modify DB instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.13.JPG)

### Step 5 — 
Steps to create a security group for Amazon QuickSight to access the RDS database in a VPC.
- In the Connectivity & security section of the RDS database, copy the VPC id.
- Under Security, choose the VPC security groups link.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.14.JPG)

- On the Security Groups page, click on Create security group.
    - For Name, type RDS SG
    - For Description, type for QuickSight
    - For VPC, choose the VPC id for your RDS instance.
    - Create security group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.15.JPG)

- Create another security group.
    - For Name, type QuickSight SG
    - For Description, type for RDS
    - For VPC, choose the VPC id for your RDS instance.
    - In the Inbound rules section, choose Add rule.
        - For Type, choose All traffic
        - For Source, choose the security group of RDS SG.  
        - Create security group.    

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.16.JPG)

-  Go to the RDS SG and edit inbound rules and add.
    - For Type, choose MSSQL.
    - For Source, choose QuickSight SG.
    - Save.
 This security group is needed for Amazon RDS to connect Amazon QuickSight.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.17.JPG)

- Open the RDS console, in the left-hand menu, choose Databases and select the database.
- Click Modify.
- In the Connectivity section, for Security group, choose RDS SG and choose Continue.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.18.JPG)

- In the Scheduling of modifications section, choose Apply immediately.
- Modify DB instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.19.JPG)

### Step 6 — QuickSight
If you haven't created a QuickSight account then follow these steps :
    - Open the Amazon QuickSight landing page and choose
    Sign up for QuickSight.
    - On the Create your QuickSight account screen, for Edition, choose Standard and choose Continue.
    - Type a QuickSight account name and email address and keep the remaining default selections. 
    - Choose Finish.
    - Go to Amazon QuickSight to open Amazon QuickSight in the AWS Management Console.
- In the top right corner of the screen, and choose your username and from the drop-down list, choose Manage QuickSight.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.20.JPG)

- On the left navigation pane, choose Manage VPC connections. Then, choose Add VPC connection.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.21.JPG)

    - For VPC connection name, type RDSVPC.
    - For VPC ID, choose the id of the RDS DB.
    - For Subnet ID, choose one of the subnet the RDS DB is in.
    - For Security group ID, paste the id of RDS SG.
    - Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.21.5.JPG)

- On the top left corner of your screen, choose the QuickSight icon. Then, in the left navigation, choose Datasets.
- On the Datasets page, choose New dataset.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.22.JPG)

- On the Create a Datasets page, choose RDS.
- On the New RDS data source page, enter the following details:
    - For Data source name, type DataFromRDS.
    - For Instance ID, choose your DB.
    - For Connection type, choose RDSVPC
    - For Database name, type Visualize
    - For Username, type the username you entered when creating the Visualize database.
    - For Password, type the password you entered when creating the Visualize database

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.23.JPG)

    - Choose Validate connection. If the connection was successful, choose Create data source.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.24.JPG)

- On the Choose your table page, in the Schema section, choose dbo.
- In the Tables section, choose newhire.
- CLick Select.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.25.JPG)

- On the Finish dataset creation page, leave the default selections, and choose Visualize.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.26.JPG)

- In the Visualize page, in the Visual types section, choose the Stacked Area Line Chart.
- In the Fields list section, drag and drop ename and salary to the Field Wells section.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/037/day37.27.JPG)

Delete the QuickSight Analysis, Dataset, RDS DB instance to cleanup.

## ☁️ Cloud Outcome

Created a sample Amazon RDS instance on an MS SQL engine, and connected the data for visualization using Amazon QuickSight.

## Social Proof

[Blog](https://dev.to/aaditunni/visualize-data-in-amazon-rds-for-microsoft-sql-server-using-amazon-quicksight-kmp)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7028433861596319747--VfD?utm_source=share&utm_medium=member_desktop)
