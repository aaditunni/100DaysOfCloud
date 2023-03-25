# Lambda X-Ray

## Introduction

Create a Lambda function that can be called via a Function URL, that will query a Dog photo API, it will then save the image to an S3 bucket, and then show the image in the browser. This is a mini project by Adrian Cantrill which I followed.

The Dog API we’re using is: https://dog.ceo/dog-api/

## Prerequisite

- AWS account.
- Download the [function.zip file](). Unzip it once.

## Services Covered

- S3
- Lambda
- IAM
- CloudWatch/X-Ray

## Try yourself

### Step 1 — S3
Go to the S3 console.
- Create a bucket with a unique name and rest all configurations as default.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.JPG)

### Step 2 — IAM
Go to the IAM console.
- Click on Roles on the left side menu.
- Click Create role.
- Set the trusted identity to AWS Service and select Lambda.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.1.JPG)

- Click Next.
- On the Add permissions page, search for and select AmazonS3FullAccess.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.2.JPG)

- Then search for and select CloudWatchFullAccess.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.3.JPG)

- Click Next.
- Set the Role name to dog-photo-function-role.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.4.JPG)

- Click Create role.

### Step 3 — Lambda
Go to the Lambda console.
- Click on Functions, then click Create function.
- Set the Function name to dog-image-scraper.
- Change the Runtime to Python 3.9.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.5.JPG)

- Under Permissions, expand the Change default execution role pane, and select Use an existing role.
- Search for and select the role create earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.6.JPG)

- Click Create function.
- On the function page, in the Code tab, click Upload from and select .zip file.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.7.JPG)

- Click Upload, select the zip file you downloaded earlier, and click Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.8.JPG)

- Go to the Configuration tab, then Function URL, then click Create function URL.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.9.JPG)

- On the next page, change the Auth type to NONE.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.10.JPG)

- Then click Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.11.JPG)

- This leaves our Lambda open to the public, to be called unlimited times.
- Go to the Configuration tab, then Environment variables, and click Edit.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.12.JPG)

- The Lambda function gets the name of our S3 bucket from an environment variable, so set that here.
- The variable name is BUCKET_NAME, and the value needs to be the name of the bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.13.JPG)

- Click Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.14.JPG)

- Go to the Configuration tab, then Monitoring and operations tools, and click Edit.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.15.JPG)

- Enable Active tracing under AWS X-Ray and click Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.16.JPG)

- Go to the Configuration tab, then General configuration, and click Edit.
- Change the Timeout to 0 min 15 sec. If the Dog API takes longer than the default 3 seconds to reply, don’t want the Lambda timing out.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.17.JPG)

- Click Save.

### Step 4 — Testing
- Go to the Lambda console and then to the function.
- Click on the Function URL to visit the function in your browser.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.18.JPG)

- You will see a photo of a dog.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.19.JPG)

- Check the S3 bucket and you'll see an image in the bucket which if you open it will be the same picture.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.20.JPG)

- Go to the CloudWatch console.
- Go to X-Ray traces and then Service map on the left side menu.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.21.JPG)

- This shows us a map of all of the services / resources that our function calls.
- Click on any of the resources to see metrics about it, such as latency, number of requests, faults, etc.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.22.JPG)

- Head to the Traces page, you can see that same information but in more detail.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.23.JPG)

- Click on one of the traces at the bottom of the page. Each trace is a function execution.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.24.JPG)

- Now lets break the function, and see what changes in X-Ray.
- Go to the Lambda console and click on the function created.
- Go to Configuration then Environment variables and click Edit.
- Change the BUCKET_NAME variable to a bucket you don’t have access to. For example, day84-cats.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.25.JPG)

- Click Save.
- Click on the Function URL again.
- You will get an Internal Server Error.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.26.JPG)

- Go back to the CloudWatch console, then go to X-Ray traces and then click Traces.
- Click Run query to get the latest traces.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.27.JPG)

- Under Traces you can see that there was a trace from ~1 minute ago, that returned a response code of 502 (500-599 response codes mean there was an error on the server side, we want to see response code’s of 200-299 which mean success).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.28.JPG)

- Click on the trace ID, and you can see quite a few errors and faults.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.29.JPG)

- Click on S3 and you will see more information. CLicking on the Exceptions Tab will show you what the error is.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/084/day84.30.JPG)

### Step 5 — Cleanup
- Empty and delete the bucket.
- Delete the Lambda function.
- Delete the IAM role.
- Delete the CLoudWatch logs group.
- CloudWatch X-Ray traces are deleted automatically after 30 days.

## ☁️ Cloud Outcome

Created a Lambda function that can be called via a Function URL, that queries a Dog photo API, then saves the image to an S3 bucket, and then shows the image in the browser.

## Social Proof

[Blog](https://dev.to/aaditunni/lambda-x-ray-1mpb)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7045463116972736512-1uRI?utm_source=share&utm_medium=member_desktop)
