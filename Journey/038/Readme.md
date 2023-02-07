# Image Pixelation using Lambda and S3 Events

## Introduction

Create a simple event-driven image processing pipeline. The pipeline uses two S3 buckets, a source bucket and a destination bucket. When images are added to the source bucket a lambda function is triggered based on the PUT. When invoked the lambda function receives the event and extracts the bucket and object information. Once those details are known, the lambda function, using the PIL module pixelates the image with 5 different variations (8x8, 16x16, 32x32, 48x48 and 64x64) and uploads them to the processed bucket.

## Prerequisite

AWS free tier account.

## Services Covered

- S3
- IAM
- Lambda

## Try yourself

Make sure you use the name I mentioned as the code is based on the names I provided. 

### Step 1 — Create source and destination S3 buckets.
Source bucket.
- Go to S3 console.
- Click Create Bucket.
- Give an unique name (day38-source) and select the region (us-east-1) you want your bucket to be in.
- Leave the remaining options as default configuration.
- Create bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.JPG)

Destination bucket.
- Go to S3 console.
- Click Create Bucket.
- Give an unique name (day38-destination) and select the region (us-east-1) you want your bucket to be in.
- Leave the remaining options as default configuration.
- Create bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.1.JPG)

### Step 2 — Create Lambda role
- Go to the IAM Console.
- Click Roles, then Create Role.
- For Trusted entity type, pick AWS service.
- For the service to trust pick Lambda then click Next , Next again. Don't add any permissions policies.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.2.JPG)

- For Role name put day38-role then Create the role.
- Click day38-role.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.3.JPG)

- Under Permissions Policy we need to add permissions and it will be an inline policy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.4.JPG)

- Click JSON and delete the contents of the code box entirely.
- Copy the below code and paste into the permissions policy code editor box :
    ```
    {
        "Version": "2012-10-17",
        "Statement": 
        [
        {
            "Effect":"Allow",
            "Action":[
            "s3:*"
            ],
            "Resource":[
                "arn:aws:s3:::REPLACEME-destination",
                "arn:aws:s3:::REPLACEME-destination/*",
                "arn:aws:s3:::REPLACEME-source/*",
                "arn:aws:s3:::REPLACEME-source"
            ]
            
        },
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "arn:aws:logs:us-east-1:YOURACCOUNTID:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:us-east-1:YOURACCOUNTID:log-group:/aws/lambda/day38-image_pixelator:*"
            ]
        }
        ]
    }
    ```
- Replace the term REPLACEME with the name you picked for your buckets above (day38-destination/day38-source).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.6.JPG)

- Replace the term YOURACCOUNTID with your account id. To get that, click the account dropdown at the top right
click the small icon to copy down the Account ID.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.7.JPG)

- Click Review Policy.
- For name put day38-inline and create the policy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.8.JPG)

### Step 3 — Create Lambda Function
- Go to the lambda console.
- Click Create Function.
- Select Authoring from Scratch.
-  For Function name enter day38-image_pixelator.
- For Runtime select Python 3.9.
- For Architecture select x86_64.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.9.JPG)

- For Permissions expand Change default execution role pick Use an existing role and in the Existing role dropdown, pick day38-role.
- Create Function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.10.JPG)

- Download this [file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/my-deployment-package.zip) and unzip it. It will contain a .zip file. Don't unzip it.
- Click Upload from and select .zip file.
- On the lambda screen, click Upload locate and select that .zip, and then click the Save button.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.11.JPG)

### Step 4 — Configure the Lambda Function & Trigger
Add an environment variable telling the lambda function which destination bucket to use.
- Click Configuration tab and then Environment variables.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.12.JPG)

- Click Edit then Add environment variable, under Key put destination_bucket and for Value put the bucket name of your destination bucket (day38-destination). 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.13.JPG)

- Make sure to put the destination bucket here and NOT the source bucket. If you use the source bucket here, the output images will be stored in the source bucket, this will cause the lambda function to run over and over again and incur a large bill.
- Click Save

Increase timeout as sometimes the file can be large to be processed in the default 3 seconds.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.14.JPG)

- Click General configuration then click Edit and change the timeout to 1 minutes and 0 seconds, then click Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.15.JPG)

Add a trigger for the lambda function to be triggered when an object is uploaded or PUT in the source bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.16.JPG)

- Click Add trigger.
- In the dropdown pick S3.
- Under Bucket pick your source bucket (day38-source). 
- Make sure this is the source bucket and NOT the destination bucket and NOT any other bucket. Only pick the SOURCE bucket here.
- Check the Recursive invocation acknowledgment box, this is because this lambda function is invoked every time anything is added to the source bucket, if you configure this wrongly, or configure the environment variable above wrongly ... it will run the lambda function over and over again for ever. 
- Leave everything else as default configurations.
- Once checked, click Add.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.17.JPG)

### Step 5 — Testing
- Open the source S3 bucket (day38-source).
- Click on the Objects Tab.
- Click Upload
- Choose Add files.
- Choose any picture you want.
- Leave the rest of the options on the default settings, and choose Upload.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.18.JPG)

- Go to the CloudWatch Logs tab.
- Click the Refresh icon, locate and click /aws/lambda/day38-image_pixelator.
If there is a log stream in there, click the most recent one, if not, keep clicking the Refresh icon and then click the most recent log stream
Expand the line which begins with {'Records': [{'eventVersion': and you can see all of the event information about the lambda invocation, you should see the object name listed as well.
- Open the destination bucket (day38-destination).
- You will notice that there are 5 objects i.e the pixelated images.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/day38.19.JPG)

- Select each of the pixelated versions of the image. You should have 5 (8x8, 16x16, 32x32, 48x48 and 64x64).
- Click Open.
- You browser will either open or save all of the images.
- Open them one by one, starting with 8x8 and finally 64x64 in order and notice how they are the same image, but less and less pixelated.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/pixelated-8x8-pexels-aleksandar-pasaric-3629227.jpg)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/pixelated-16x16-pexels-aleksandar-pasaric-3629227.jpg)

![Screenshot](https://https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/pixelated-32x32-pexels-aleksandar-pasaric-3629227.jpg)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/pixelated-48x48-pexels-aleksandar-pasaric-3629227.jpg)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/pixelated-64x64-pexels-aleksandar-pasaric-3629227.jpg)

Original Image :

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/038/pexels-aleksandar-pasaric-3629227.jpg)

### Step 6 — Cleanup
- Delete the lambda function.
- Delete the IAM role.
- Empty both the source and destination S3 buckets and then delete them both.

## ☁️ Cloud Outcome

Created a simple event-driven image processing pipeline where the source image is pixelated into 5 different variations.

## Social Proof

[Blog](https://dev.to/aaditunni/image-pixelation-using-lambda-and-s3-events-ic2)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7028681390397263872-6XxJ?utm_source=share&utm_medium=member_desktop)
