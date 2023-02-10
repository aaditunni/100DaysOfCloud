# Notification alert when an object is deleted from S3 bucket

## Introduction

Sometimes our requirement is when anyone deletes any object in S3 bucket we should receive an alert.
The Amazon S3 notification feature enables you to receive notifications(SNS/SQS/Lambda) when certain events happen in your bucket.
The S3 events work at the object level, so if something happens to the object, in this case, maybe PUT, POST, COPY or DELETE then the event is generated and that event will be delivered to the target(SNS, SQS or LAMBDA).

## Prerequisite

AWS free tier account.

## Services Covered

- SNS
- S3

## Try yourself

Region - us-east-1

### Step 1 — SNS topic
- Go to SNS console.
- Give a Topic name and click Next step.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.JPG)

- Keep everything else as default and Create Topic.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.1.JPG)

- Create a subscription for the above topic with Protocol as Email and Endpoint as the email you want to receive the notification in.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.2.JPG)

- Go to your email to confirm the subscription.

### Step 2 — S3 bucket
- Go to S3 console.
- Click Create Bucket.
- Give an unique name and select the region.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.3.JPG)

- Leave the remaining options as default configuration.
- Create bucket.
- Click on the Objects Tab in the S3 bucket.
- Click Upload.
- Select any file to upload.
- Click Upload and wait for it to complete.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.7.JPG)

### Step 3 — SNS Access policy
- Go to the SNS topic created earlier.
- Select the SNS topic and click on Edit.
- Click on Access policy. Paste the below json policy. 
- Replace ACCOUNT_ID with your account id, TOPIC_NAME with your SNS topic name and S3_ARN with the ARN of your S3 bucket.
```
{
  "Version": "2008-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__default_statement_ID",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "SNS:GetTopicAttributes",
        "SNS:SetTopicAttributes",
        "SNS:AddPermission",
        "SNS:RemovePermission",
        "SNS:DeleteTopic",
        "SNS:Subscribe",
        "SNS:ListSubscriptionsByTopic",
        "SNS:Publish",
        "SNS:Receive"
      ],
      "Resource": "arn:aws:sns:us-east-1:ACCOUNT_ID:TOPIC_NAME"
      "Condition": {
        "ArnLike": {
          "AWS:SourceArn": "S3_ARN" 
        }
      }
    }
  ]
}
``` 
- This will give permission on SNS topic to allow S3 event system to deliver events to it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.2.5.JPG)

### Step 4 — S3 Event notification
- Go to S3 console.
- Click on the bucket and go to Properties.
- Scroll down you will see Event notifications and click on Create event notification.
- Give a name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.4.JPG)

- Under Event types, select Permanently deleted.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.5.JPG)

- Under Destination select the SNS topic.
- Click on Save changes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.6.JPG)

### Step 5 — Testing and cleanup
Testing
- Go to your S3 bucket and delete the file you uploaded.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.8.JPG)

- You will receive an email as notification alert.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/041/day41.9.JPG)

Cleanup
- Empty the bucket and then delete the bucket.
- Delete the SNS subscription and topic.

## ☁️ Cloud Outcome

Created an S3 event notification to send a notification alert to my email when an object from the S3 bucket is deleted.

## Social Proof

[Blog](https://dev.to/aaditunni/notification-alert-when-an-object-is-deleted-from-s3-bucket-508a)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7029767344398712832-8pYK?utm_source=share&utm_medium=member_desktop)
