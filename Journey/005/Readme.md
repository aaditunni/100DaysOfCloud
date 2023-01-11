# Create a CloudWatch Alarm

## Introduction

A CloudWatch Alarm for an EC2 instance to send a notification when the Alarm triggers an Alert state.

## Prerequisite

AWS Free tier account.

## Use Case

Customers use Amazon CloudWatch to collect and track metrics, collect and monitor log files, set alarms, and automatically react to changes in AWS resources. CloudWatch provides system-wide visibility into resource utilization, application performance, and operational health.

## Services Covered

- EC2
- CloudWatch

## Try yourself

### Step 1 — 
- Launch an EC2 instance that is free tier eligible with a public IP address and supply the [provided bash script](https://github.com/100DaysOfCloud/100DaysOfCloudIdeas/blob/master/Projects/OPS/OPS04/OPS04-AWS200-userdata.sh) to install a simple website with an apache server in the UserData under Advanced details.
- Visit the the public IP so that you are generating NetworkIn. You need to do this so the Metric appears selectable when create your CloudWatch Alarm

### Step 2 — 
Search CloudWatch and under All Alarms select Create Alarm.
- Create a CloudWatch Alarm and click Select metric.
- Use EC2 NetworkIn as the metric. Make sure you select the EC2 that you created.


![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/005/day5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/005/day5.1.JPG)

### Step 3 — 
- Set your CloudWatch Alarm to use a 5 minute period
- Set your CloudWatch Alarm to a very low static threshold such as 5000
- Set the Datapoint to alarms to 3 of 4

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/005/day5.2.JPG)

### Step 4 — 
- For Notifications, select In alarm and create a new topic and give your email as the endpoint to recieve the notification when the Alarm is triggered.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/005/day5.3.JPG)

### Step 5 — 
- Try to get the Alarm to trigger an Alert state by visiting the website and generating NetworkIN. Wait for few minutes to see the Alarm trigger.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/005/day5.4.JPG)

- Delete the EC2 instance and the CloudWatch Alarm to cleanup.

## ☁️ Cloud Outcome

When the threshold NetworkIN in crossed, CloudWatch triggers an Alarm and sends a notification to your email.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/005/day5.5.JPG)


## Social Proof

[Blog](https://dev.to/aaditunni/create-a-cloudwatch-alarm-4oje)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7016709047886188544-e8JW?utm_source=share&utm_medium=member_desktop)
