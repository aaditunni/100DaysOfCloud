# Stop/Start EC2 instance on a scheduled basis

## Introduction

Shut down all the EC2 instance on a scheduled basis. To achieve that, we use Lambda in the combination of CloudWatch Events.

## Prerequisite

- AWS free tier account.
- EC2 instances.

## Use Case

Start or stop instances on a scheduled basis to save costs.

## Services Covered

- IAM
- Lambda
- CloudWatch/EventBridge

## Try yourself

### Step 1 — IAM Role
Create IAM Role so that Lambda can interact with CloudWatch Events.
- Go to IAM console.
- Go to Roles on left side menu.
- Click Create Role.
- Select AWS Service and Lambda as the Use case.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.JPG)

- Click Next.
- Click Create policy, go to the JSON editor and paste the below code:
    ```
    {
    "Version": "2012-10-17",
    "Statement": [
    {
    "Effect": "Allow",
    "Action": [
    "logs:CreateLogGroup",
    "logs:CreateLogStream",
    "logs:PutLogEvents"
    ],
    "Resource": "arn:aws:logs:*:*:*"
    },
    {
    "Effect": "Allow",
    "Action": [
    "ec2:Start*",
    "ec2:Stop*"
    ],
    "Resource": "*"
    }
    ]
    }
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.1.JPG)

- Give a name and create a policy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.2.JPG)

- Back on the role creation page, select the policy that was created and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.3.JPG)

- Give an name for the role and Create Role.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.4.JPG)

### Step 2 — Lambda
- Go to the Lambda console.
- Click Create function.
- Select Author from scratch.
- Name: Give your Lambda function any name.
- Runtime: Select Python 3.9 as runtime.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.5.JPG)

- Role: Choose the existing role we created earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.6.JPG)

- Click on Create function.
- Delete the existing code and paste the below code:
    - Change the Value of region.
    - In the instance field specify instance id.
    ```
    import boto3
    # Enter the region your instances are in. Include only the region without specifying Availability Zone; e.g., 'us-east-1'
    region = 'XX-XXXXX-X'
    # Enter your instances here: ex. ['X-XXXXXXXX', 'X-XXXXXXXX']
    instances = ['X-XXXXXXXX']
    ec2 = boto3.client('ec2', region_name=region)

    def lambda_handler(event, context):
        ec2.stop_instances(InstanceIds=instances)
        print 'stopped your instances: ' + str(instances)
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.7.JPG)

- Go to Configuration tab, click General configuration on left side menu and change the Timeout to 10 seconds.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.9.JPG)

- Deploy the code.


### Step 3 — CloudWatch Events
- Go to the Amazon CloudWatch console.
- Choose Events, and then choose Create rule.
- Give a name, choose Schedule under Event Source.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.10.JPG)

- Click Next.
- Select the first option in Schedule pattern which lets us create a [cron expression](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) to run at a specific time.
- Enter the required schedule to stop the instances and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.11.JPG)

- Choose Add target, and then choose Lambda function that you created earlier to stop the instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/046/day46.12.JPG)

- Create rule.


## ☁️ Cloud Outcome

Created a scheduled stop for the EC2 instances using a  combination of Lambda and CloudWatch Events.

## Next Steps

Create a scheduled start for the EC2 instances.
Lambda function for start:

    
    import boto3
    region = 'XX-XXXXX-X'
    instances = ['X-XXXXXXXX', 'X-XXXXXXXX']
    ec2 = boto3.client('ec2', region_name=region)

    def lambda_handler(event, context):
        ec2.start_instances(InstanceIds=instances)
        print('started your instances: ' + str(instances))
    

## Social Proof

[Blog](https://dev.to/aaditunni/stopstart-ec2-instance-on-a-scheduled-basis-26pa)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7031724943855759360-88iD?utm_source=share&utm_medium=member_desktop)
