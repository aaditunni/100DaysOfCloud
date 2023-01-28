# Hosting a static website with S3 & CloudFront

## Introduction

 Serve a static page through S3 that is able to be cached at a Content Delivery Location for cost optimization. Need to first create an S3 bucket to host a static website and serve the end-users through CloudFront endpoint.

## Prerequisite

AWS free tier account.

## Use Case

- Using AWS S3 facilitates highly-scalable, secured and low-latency data storage from the cloud. With its simple web service interface, it is easy to store and retrieve data on AWS S3 from anywhere on the web.
- AWS CloudFront is a data/content caching technology that resides at the defined edge locations. If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately. If the content is not in that edge location, CloudFront retrieves it from an origin that you’ve defined — such as an Amazon S3 bucket, that you have identified as the source for the definitive version of your content.

## Services Covered

- S3
- AWS CloudFront

## Try yourself

### Step 1 — S3
- Create S3 Bucket.
    - Search S3.
    - Click Create Bucket.
    - Give a unique bucket name and ensure the region is set to US East (N.Virginia) us-east-1 .
    - Uncheck Block all public access.
    - Tick the box under Turning off block all public access might result in this bucket and the objects within becoming public.
    - Create Bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.1.JPG)

- Make it publicly accessible using a bucket policy.
    - Go into the bucket you just created. 
    - Click the Permissions tab. 
    - Scroll down and in the Bucket Policy area, click Edit and in the box, paste the code below :
        ```    
        {
            "Version":"2012-10-17",
            "Statement":[
            {
                "Sid":"PublicRead",
                "Effect":"Allow",
                "Principal": "*",
                "Action":["s3:GetObject"],
                "Resource":["REPLACEME_BUCKET_ARN/*"]
            }
            ]
        }
        ```
    - Replace REPLACEME_BUCKET_ARN with your bucket ARN and save changes.


![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.6.JPG)

- Enable Static Hosting.
    - Click on the Properties Tab.
    - Scroll down and locate Static website hosting and click Edit.
    - Select Enable Select Host a static website.
    - For both Index Document and Error Document enter index.html.
    - Click Save Changes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.5.JPG)

- Download and unzip the [index.html](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/index.html) file
- Return to the S3 console Click on the Objects Tab.
    - Click Upload
    - Select the index.html file. 
    - Click Upload and wait for it to complete.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.3.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.4.JPG)

### Step 2 — CloudFormation
- Go to the CloudFront console.
- Click Create a CloudFront distribution.
- Click on the Origin Domain Name field and select the S3 bucket you created earlier.
- Notice that Name is already pre-filled.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.7.JPG)

- For Origin Access, select Origin access control settings(recommended).
-In Origin access control, click Create control setting.
- Click Create on the popup window.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.9.JPG)

- Scroll down to until you see “Viewer”, For “Viewer Protocol Policy”, select “Redirect HTTP to HTTPS”.
- Leave the rest all as default configurations.
- Click Create distribution.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.10.JPG)

### Step 3 — 
- Once the distribution is created, there will be a notification in the console asking to update the S3 bucket policy. Click on Copy policy from the notification.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.11.JPG)

- Go to your S3 bucket and replace the old policy with the new one copied.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.12.JPG)

- Now you cannot access the static website using the bucket website endpoint and only through the CloudFormation distribution access point.
- Go to your CloudFront distribution and in the general tab, under Details, copy the Distribution domain name - link and paste it in another tab and add /index.html to the end of the link and hit enter.
- The static website is available and accessible.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/028/day28.13.JPG)

Cleanup:
- Disable and Delete the CloudFormation Distribution.
- Delete Origin access control by navigating to Origin access under Security on left side menu in the CloudFormation console.
- Empty and Delete S3 bucket.


![Screenshot](https://via.placeholder.com/500x300)


## ☁️ Cloud Outcome

Hosted a static website with S3 & CloudFront.

## Social Proof

[Blog](https://dev.to/aaditunni/hosting-a-static-website-with-s3-cloudfront-29km)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7025074387246587904-NP7J?utm_source=share&utm_medium=member_desktop)