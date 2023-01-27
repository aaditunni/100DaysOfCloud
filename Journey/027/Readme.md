# Session Stickiness in Application Load Balancer

## Introduction

ALB session stickiness using an ALB managed session cookie AWSALB.

## Prerequisite

AWS free tier account.

## Use Case

Sticky sessions use cookies to help the client maintain a connection to the same instance over a cookie's lifetime. Using sticky sessions configures a load balancer to bind user sessions to a specific instance. This means that all requests from a user during a session are sent to the same instance.

## Services Covered
- AWS CloudFormation
- EC2


## Try yourself

### Step 1 — 
- Select the N. Virginia // us-east-1 region
- Use this CloudFormation template from the link below to automatically provision the resources.
    - https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://learn-cantrill-labs.s3.amazonaws.com/aws-simple-demos/aws-alb-session-stickiness/ALBSTICKINESS.yaml&stackName=ALB (source: Adrian Cantrill).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.JPG)

- Applying the cloudformation template will create a VPC, 3 Public Subnets, an ASG and LT which will bootstrap some simple web servers and an application load balancer which runs in each AZ and has no stickiness enabled.
- Check the box for capabilities: [AWS::IAM::Role] acknowledgment.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.1.JPG)

- Click Create Stack.
- Wait for the Status to show CREATE_COMPLETE.

### Step 2 — 
- Open and checkout all the instances. You can notice that each instance has a random cat picture with the instance id mentioned.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.3.JPG)

- Go to the Load Balancer by navigating it from the left side menu and click on the ALB that was created by the CloudFormation Stack to open it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.4.JPG)

- Under description tab, copy the DNS name and open it in another browser tab.
- It will open one of webpage of the EC2 instances created and upon refreshing, they keep cycling between them as the load is balanced between the instances.
- Session stickiness is not enabled so sessions are being distributed across all targets in the ALB target group.

### Step 3 — 
- Navigate to the target group from the left side menu and open the target group created by the CloudFormation Stack.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.5.JPG)

- Under Attributes tab, click Edit in Attributes section.
- Scroll down to Target selection configuration and turn on Stickiness, set the Duration to 1 minute and click Save changes.
- Stickiness is now enabled.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.6.JPG)

- Go back to the Load balancer DNS name and refresh it a few times.
- After a few refreshes it will lock onto one specific instance.
- It will be locked to this instance until either the cookie expires or the instance fails its health check.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.7.JPG)

- Note down the instanceID of the instance you are locked to.
- Go the the Instances and stop the Instance with the InstanceID you noted.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.8.JPG)

- Try refreshing the Load balancer DNS name link a few times and you will notice that it is locked onto another instance.
- Start the instance that was stopped and go back and refresh it again.
- You will notice that it is still locked on to the new instance and won't return to the old one.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/027/day27.9.JPG)

### Step 4 — 
- Navigate to the target group from the left side menu and open the target group created by the CloudFormation Stack.
- Under Attributes tab, click Edit in Attributes section.
- Scroll down to Target selection configuration and turn off Stickiness.
- Stickiness is now disabled.
- Go back to the Load balancer DNS name and refresh it a few times.
- You should start to move freely between all of the instances again.

- Delete the CloudFormation Stack to cleanup.

## ☁️ Cloud Outcome

Learned about Session Stickiness in Application Load Balancer.

## Social Proof

[Blog](https://dev.to/aaditunni/session-stickiness-in-application-load-balancer-1a0m)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7024662555587489793-VJeT?utm_source=share&utm_medium=member_desktop)