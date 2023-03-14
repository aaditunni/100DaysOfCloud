# Migrate an ASP.NET web application to AWS Elastic Beanstalk Windows Web Application Migration Assistant (WWAMA)

## Introduction

 Migrate a sample ASP.NET web application to a fully managed AWS Elastic Beanstalk environment using the Windows Web Application Migration Assistant (WWAMA).

## Prerequisite

- AWS free tier account.
- KeyPair

## Services Covered

- CloudFormation
- IAM
- EC2
- Elastic Beanstalk

## Try yourself

### Step 1 — CloudFormation
- Launch this [CloudFormation stack](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create?stackName=WWAMAStack&templateURL=https://public-read-location.s3.amazonaws.com/WWAMALab.yml) in us-east-1 to launch an EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.JPG)

- Click Next.
- Give a stack name.
- Select the previously created KeyPair.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.1.JPG)

- Keep clicking Next until you create the stack.
- Wait for the status to show CREATE_COMPLETE. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.2.JPG)

### Step 2 — IAM
- Create an IAM User for Programmatic access with the following permissions.
    - IAMReadOnlyAccess
    - AdministratorAccess-AWSElasticBeanstalk
    - AmazonS3FullAccess
- Create User.
- Create an Access Key and note the credentials.

### Step 3 — EC2
- Connect to the EC2 instance created by the CloudFormation template using RDP client.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.3.JPG)

- Click on Get password and upload the KeyPair file and click decrypt to get the password.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.4.JPG)

- Click on Download remote desktop and open it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.5.JPG)

- Click Connect.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.6.JPG)

- Enter the decrypted password.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.7.JPG)

- Click Yes for the certificate error dialog box.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.8.JPG)

- You will be logged in to your EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.9.JPG)

### Step 3 — Migration
- Open Powershell as Administrator in the EC2 instance.
- Run these commands to configure the AWS credentials.  Replace ACCESS_KEY and SECRET_ACCESS_KEY with your credentials.
    ```
        Import-Module AWSPowerShell
    ```
    ```
        Set-AWSCredential -AccessKey ACCESS_KEY -SecretKey SECRET_ACCESS_KEY -StoreAs default
    ```
- Go to C:\ drive and locate the file named wwama.zip.
- Right click on wwama.zip and extract the file.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.10.JPG)

- Open a web browser on the EC2 Windows Server instance and navigate to http://localhost/ 
- You will see the sample website that the migration assistant will migrate.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.11.JPG)

- In the same powershell move inside the wwama directory twice using the cd command and run this script to run the migration assistant.
    ```
        .\MigrateIISWebsiteToElasticBeanstalk.ps1
    ```
- The assistant prompts you for the location of your credentials file. Press ENTER to skip.
- When prompted for AWS Profile Name, enter default.
- Enter us-east-1 as the AWS Region where you'd like your Elastic Beanstalk environment to run when asked.
- The assistant then discovers any websites running on your IIS server and lists them. Enter the number 2 to migrate the sample site.
- The assistant then prompts you to update any connection strings selected above, press ENTER as there aren’t any connection strings in this application.
- Name your new Elastic Beanstalk application.
- When prompted to select the Windows Server version, type 6 and press Enter.
- Enter instance type that your application will run on. Type t2.micro.
- The migration assistant then migrates your application to Elastic Beanstalk.
- Navigate to your web application hosted on Elastic Beanstalk using the URL provided in PowerShell or through the AWS Elastic Beanstalk console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/073/day73.12.JPG)

### Step 4 Cleanup
- Terminate the Elastic Beanstalk environment and then delete it.
- Delete the CloudFormation Stack.
- Empty and delete S3 bucket.
- You can also delete the Access Key and delete the User if you want.

## ☁️ Cloud Outcome

 Migrated a sample ASP.NET web application to a fully managed Elastic Beanstalk environment using the Windows Web Application Migration Assistant (WWAMA).

## Social Proof

[Blog](https://dev.to/aaditunni/migrate-an-aspnet-web-application-to-aws-elastic-beanstalk-windows-web-application-migration-assistant-wwama-kd4)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7041507714153201664-mmqn?utm_source=share&utm_medium=member_desktop)
