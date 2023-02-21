# Restricting users to specific AWS region

## Introduction

Restricting users to specific AWS region by:
- Deactivate unused region endpoint.
- Only allow specific services in a region.

## Prerequisite

AWS free tier account.

## Services Covered

- IAM
- Organizations

## Try yourself

### Step 1 —
Deactivate unused region endpoint.
- AWS provides you a functionality through which you can disable user to generate STS credentials for unused region. - Go to the IAM console, click on Account settings and as you can see there is an option to Deactivate.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.JPG)

Only allow specific services in a region.
- Go to Organizations console.
- go to Policies on right side menu and Enable Service Control Policies (SCP).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.1.JPG)

- Click on Service Control Policies and Create a new Policy.
    ```
    {
    "Version": "2012-10-17",
    "Statement": [
    {
    "Sid": "DenyAllOutsideOregonandVirginia",
    "Effect": "Deny",
    "NotAction": [
        "ec2:*",
        "s3:*"
    ],
    "Resource": [
        "*"
    ],
    "Condition": {
        "StringNotEquals": {
        "aws:RequestedRegion": [
        "us-west-2",
        "us-east-1"
        ]
        }
    }
    }
    ]
    }
    ```
    - In this policy, we are restricting the user to access EC2 and S3 only in us-west-2(Oregon) and us-east-1(N Virginia). 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.3.JPG)

- Save and click on the Policy and Attach the policy to the Organization or an account and they will only be able to access EC2 and S3 in those two regions.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.4.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.5.JPG)

## ☁️ Cloud Outcome

Created a Service Control policy to restrict users to specific regions. 

## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[Blog](https://dev.to/aaditunni/restricting-users-to-specific-aws-region-5ebm)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7033903589399871489-Dq1w?utm_source=share&utm_medium=member_desktop)