# AWS Secret Manager

## Introduction

AWS Secret Manager helps you to store, rotate, manage and retrieve access to secrets such as database credentials, API keys and other secrets throughout their lifecycle. It also provides built-in integration for secret rotation for MySQL, PostgreSQL and Aurora on RDS. Rotation for other types of secrets can easily be done using Lambda function. In order to retrieve secrets you need to call a Secret Manager API. It uses versioning so that applications don’t break when secrets are rotated. 

## Prerequisite

- AWS free tier account.

## Services Covered

AWS Secret Manager

## Try yourself

### Step 1 — 
- Go to the secret manager console.
- Click on Store a new secret. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.JPG)

- There are five options for storing secret. 
- For this, I am choosing “Credentials for Amazon RDS database” but feel free to choose based on your requirement. - In the credentials field give User name and Password. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.1.JPG)

- You can encrypt your Credentials either using default KMS encryption key or you can create your own key. 
- Select the database that you want to implement the secret.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.2.JPG)

- Click Next.
- Give your secret some name and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.3.JPG)

- In Secret rotation, you can configure automatic rotation. 
- I have configured it to rotate it after every 30 days. If you turn on automatic rotation, the first rotation will happen immediately when you store this secret.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.4.JPG)

- Give a name for the lambda function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.5.JPG)

- Click on Next and Click Store. 

In the backend, its going to use CloudFormation to create resources such as Lambda function(to rotate secrets)and IAM Role(to interact with resources)

- Can use the sample code provided to use it in your application to retrieve the secret. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.6.JPG)

- You can retrieve the secrets from the Secret Manager console by clicking on Retrieve secret value under Secret value.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.11.JPG)

### Step 2 — 
- Go to CloudTrail and search for Event name GetSecretValue to get the detailed information when someone try to get the secret.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.7.JPG)

- You can also configuration CloudWatch rule to get notified when someone try to delete the secret. 
- Go to CloudWatch console.
- Click on Events → Rules and Create rule. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.8.JPG)

- Under Service Name select Secrets Manager and Event Type as AWS API Call via CloudTrail.
- For Specific operation(s) add DeleteSecret.
- Under Targets select the SNS topic (Make sure you have a SNS topic created which is subscribed to email or sms to get alerts).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.9.JPG)

- Give your rule some name and click on Create rule.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/039/day39.10.JPG)

- If you try to delete the secret, you will get an alert in whatever you subscribed in the SNS topic.

Go to the secret and click on Actions and click Delete secret and schedule the deletion for the secret to be deleted.

## ☁️ Cloud Outcome

Stored a secret in AWS secret Manager and then created a CloudWatch rule to get an alert when the secret is tried to be deleted.

## Social Proof

[Blog](https://dev.to/aaditunni/aws-secret-manager-24a)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7029032883306520577-1l1v?utm_source=share&utm_medium=member_desktop)
