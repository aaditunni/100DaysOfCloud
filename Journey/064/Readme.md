# Scan S3 bucket to find Sensitive data with Macie

## Introduction

Create an S3 bucket and add a text file containing sensitive data and then use Macie to scan it.

## Prerequisite

- AWS free tier account.
- A text file containing sensitive/personally identifiable information.

## Services Covered

- S3
- Macie

## Try yourself

### Step 1 — S3
- Go to S3 console.
- Click Create Bucket.
- Give an unique name and select the region.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.JPG)

- Leave the remaining options as default configuration.
- Create bucket.
- Click on the Objects Tab in the S3 bucket.
- Click Upload.
- Select the sensitive data text to upload.
- Click Upload and wait for it to complete.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.1.JPG)

### Step 2 — Macie
- Go to the Macie console.
- Click Getting started.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.2.JPG)

- Click Enable Macie.
- Once enabled, wait a couple of minutes, and refresh a few times until Macie is ready.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.3.JPG)

- Go to the S3 Buckets section on the left side menu, select the bucket you have created and added files to, and then click Create job.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.4.JPG)

- On the Review S3 buckets page, click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.5.JPG)

- On the Refine the scope page, select One-time job and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.6.JPG)

- Leave Managed data identifier options on All and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.7.JPG)

- There are no Custom data identifiers so click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.8.JPG)

- On the Select allow lists page, click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.9.JPG)

- Set a Job name and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.10.JPG)

- Click Confirm.
- Once the status shows complete, click on Show results then Show findings to view the results.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.11.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/064/day64.12.JPG)

### Step 3 — Cleanup
- Go to Macie Settings and Disable Macie.
- Empty the S3 bucket and delete it.

## ☁️ Cloud Outcome

Created an S3 bucket and added a text file containing sensitive data and then used Macie to scan it.

## Social Proof

[Blog](https://dev.to/aaditunni/scan-s3-bucket-to-find-sensitive-data-with-macie-kd6)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7038241014888685568-EoZR?utm_source=share&utm_medium=member_desktop)
