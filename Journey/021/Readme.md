# Replicate Data between AWS Regions Using Amazon S3 Replication

## Introduction

Create an S3 bucket in one region and upload an object to it and replicate it to another bucket in another region using the S3 Replication rule.

## Prerequisite

AWS account.

## Use Case

- Maintain a secondary copy of your data for data protection, or have data in multiple geographies to provide users with the lowest latency, S3 Replication gives you the controls you need to meet your business needs.
- Automatically replicate data between buckets within the same AWS Region to help aggregate logs into a single bucket, replicate between developer and test accounts, and abide by data sovereignty laws.
- Replicate objects into other AWS Regions for reduced latency, compliance, security, disaster recovery, and regional efficiency.

## Services Covered

- S3

## Try yourself 

### Step 1 — Create two S3 bucket (source and destination)
- Go to S3 console.
- Click Create Bucket.
- Give an unique name and select the region you want your bucket to be in.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.JPG)

- Enable Bucket versioning. S3 Replication requires Bucket Versioning to be enabled for both source and destination S3 buckets.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.1.JPG)

- Leave the remaining options as default configuration.
- Create bucket.
- Repeat the above steps to create another S3 bucket to serve as the destination bucket for replicating objects.
- Make sure to enable Bucket Versioning for the destination S3 bucket as well. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.2.JPG)

- Don't use the same bucket for source and destination of replication the objects as replications in the same bucket will make it an endless loop and incur a lot of charges.

### Step 2 — Create and configure S3 Replication on your source S3 bucket
- From the list of S3 buckets, choose the source S3 bucket that you want to configure as your source for replication.
- Go to the  Management tab of the replication source bucket and under Replication rules, Select Create replication rule.
- Give a Replication rule name.
- Under Status, select Enabled to enable the replication rule.
- Under choose a rule scope, select Apply on all objects in the bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.3.JPG)

- In Destination, click on Choose a bucket in this account and click on browse S3 and select the bucket you created for the objects to be replicated in.
- Under IAM role, click on Choose from existing IAM roles and from the list choose Create new IAM role.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.4.JPG)

- In Encryption, check the box for  replicate objects encrypted with AWS KMS.
- Under AWS KMS key for encrypting destination objects, select Choose from your AWS KMS keys and choose the default S3 KMS key in your account.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.5.JPG)

- Under Destination storage class, check the box for Change the storage class for the replication objects and select Standard storage class.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.6.JPG)

### Step 3 — Upload an object to the source S3 bucket
- Upload an object to the replication source bucket. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.7.JPG)

- Replication metrics can take a few minutes to show up in the S3 console.
- You can notice that the object is replicated to the destination bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/021/day21.8.JPG)

## ☁️ Cloud Outcome

Used S3 Replication to replicate objects from source to destination S3 buckets across one AWS Regions to meet compliance requirements, minimize latency, and increase operational efficiency.

## Social Proof

[Blog](https://dev.to/aaditunni/replicate-data-between-aws-regions-using-amazon-s3-replication-59km)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7022551619930103808-Jaos?utm_source=share&utm_medium=member_desktop)
