# Create a new Customer Master Key (CMK) in Key Management Service (KMS) and encrypt an object

## Introduction

Create a new Customer Master Key (CMK) in Key Management Service (KMS), create a S3 bucket, upload an object an encrypt it.

## Prerequisite

AWS account.

## Services Covered

- Key Management Service (KMS)
- S3

## Try yourself

### Step 1 — Create a CMK
- Go to Key Management Service (KMS) console.
- Click on Create a Key.
- Select Symmetric as Key type.
- Select Encrypt and decrypt as key usage and click Next

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.JPG)

- Give an Alias or Name for the key.
- Give a Description (optional) and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.1.JPG)

- Choose the IAM user in Key administration permissions.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.2.JPG)

- Choose the IAM user in Key usage permissions.
- Click Next and then click Finish to create the key.

### Step 2 — Create S3 Bucket 
- Go to S3 console.
- Click Create Bucket.
- Give an unique name and select the region.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.3.JPG)

- Under Default encryption, select AWS Key Management Service (SSE-KMS).
- Click Choose from your AWS KMS Keys and select the key that was created earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.4.JPG)

- Leave the rest as default configurations and Create bucket.


### Step 3 — Upload Object to S3 Bucket
- Go to the bucket and under Objects tab, click on Upload to upload an object to the bucket.
- Click on Add files.
- Upload any file you want.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.5.JPG)

- Click on the image and in Properties tab, Scroll down to server side encryption and you will notice that since the bucket is encrypted by KMS-CMK, all the objects uploaded to it are encrypted by default.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/020/day20.6.JPG)

### Step 4 - Cleanup
- Empty the bucket and then delete it.
- Select the key that was created and disable it.
- Select the key and click on Key actions and click Schedule Key deletion.
- Enter waiting period as 7, confirm and schedule the deletion. The key will be automatically deleted after 7 days. 

## ☁️ Cloud Outcome

Created a new Customer Master Key (CMK) in Key Management Service (KMS) and encrypted an S3 object.

## Social Proof

[Blog](https://dev.to/aaditunni/create-a-new-customer-master-key-cmk-in-key-management-service-kms-and-encrypt-an-object-193f)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7022187176264118272-UeJv?utm_source=share&utm_medium=member_desktop)
