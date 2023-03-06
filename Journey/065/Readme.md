# Create an S3 Multi-Region Access Point (MRAP)

## Introduction

Amazon Simple Storage Service (S3) Multi-Region Access Points provide a global endpoint for routing Amazon S3 request traffic between AWS Regions.

## Prerequisite

AWS account.

## Services Covered

- S3

## Try yourself

### Step 1 — S3 buckets
Create two or more S3 buckets in different regions. Here, I created two buckets. One in N. Virginia (us-east-1) and the other in Singapore (ap-southeast-1).
- Make sure Bucket Versioning is enabled while creating the buckets and leave the rest all configurations as default.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.1.JPG)


### Step 2 — S3 Multi-Region Access Point
- Click on the Multi-Region Access Points from the left side menu in S3 console and click Create Multi-Region Access Point.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.2.JPG)

- Give a name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.3.JPG)

- Click Add buckets, select your buckets, and click Add buckets.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.4.JPG)

- Leave the rest all configurations as default.
- Create Multi-Region Access Point.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.6.JPG)

- Go to the Replication and failover tab.
- Under Replication rules, click on Create replication rules.
- Select Replicate objects among all specified buckets and check all of the buckets in this access point.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.7.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.8.JPG)

- Under Scope, select Apply to all objects in the bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.9.JPG)

- Leave everything else as default configurations.
- Create replication rules.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.11.JPG)

### Step 3 — Testing and Cleanup
- Select another region other than the ones the bucket is created in.
- Go to the CloudShell console.
- Run these commands to create a file of 10mb and upload it using the Multi-Region Access Points ARN. Replace YOUR_MRAP_ARN with the ARN of your Multi-Region Access Points.
    ```
        dd if=/dev/urandom of=test1.file bs=1M count=10
        aws s3 cp test1.file s3://YOUR_MRAP_ARN/
    ```
- This will create a file and copy to the bucket closest to the location you are in and then replicate to the other bucket(s) within a few minutes. You can also do this from your local machine and it will upload to the bucket closest to the location of your ISP.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.12.JPG)

- Here, I did it from the region Mumbai(ap-south-1) and the file was uploaded to the bucket in the region Singapore as it was the closest. The bucket in N. Virginia was empty and within 20 mins, the files were replicated to this bucket as well.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.13.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.14.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/065/day65.15.JPG)

- Delete the Multi-Region Access Point.
- Empty the buckets and then delete them.

## ☁️ Cloud Outcome

Created an S3 Multi-Region Access Point with two S3 buckets and uploaded a file which was routed to the closest bucket to the location I uploaded from, and then changes were replicated between all buckets.

## Social Proof

[Blog](https://dev.to/aaditunni/create-an-s3-multi-region-access-point-mrap-466j)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7038620861532114944-Y-aa?utm_source=share&utm_medium=member_desktop)
