# DynamoDB Triggers using Streams and Lambda

## Introduction

 A DynamoDB table with data of cats that are waiting to be adopted. When the adopted field is updated to true, a Lambda will be triggered that will send an email through SNS telling them that the particular has been adopted.

## Prerequisite

AWS free tier account.

## Services Covered

- DynamoDB
- SNS
- IAM
- Lambda 

## Try yourself

### Step 1 — DynamoDB
- Go to the DynamoDB console.
- Click on Create table.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.JPG)

- Give a name for the table.
- For Partition key, enter catID (String).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.1.JPG)

- Select Customize settings for Table settings.
- Select DynamoDB Standard for Table class.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.2.JPG)

- Select On-demand as Capacity mode.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.3.JPG)

- For Secondary Index, click Create global index.
- Give Partition key as adopted. Leave the rest all as default configurations and Create index.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.4.JPG)

- Leave the rest all as default configurations and Create table. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.6.JPG)

- Go to the dashboard of the table created.
- Select the table, click on Actions and select Create item.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.7.JPG)

- Give a catID such as cat0001.
- Add another String Attribute and label it catName and give it a name.
- Add another string attribute and label it adopted and enter false. This means that the cat is not adopted.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.8.JPG)

- Similarly Create Items for two or more cats.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.9.JPG)

- You can run a Query by selecting the adopted-index and entering true for partition key. This will not give any result as none of the cats are adopted. If entered false, it will give all the cats as the result since the value of adopted is false for every cats in the table.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.11.JPG)

### Step 2 — SNS
- Go to the SNS console.
- Enter a topic name and click Next step.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.12.JPG)

- Select the Standard type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.13.JPG)

- In Access policy, select Basic method and select Only the specified AWS accounts for both publishers and subscribers and enter your account ID in the text fields.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.14.JPG)

- Create Topic.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.15.JPG)

- Create a Subscription for the same Topic.
- Select Protocol as Email and enter your email and Create the subscription.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.16.JPG)

- Go to your email and confirm the subscription.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.17.JPG)

### Step 3 — Lambda
- Go to the Lambda console and create a function.
- Give a function name.
- Select Python 3.9 as Runtime.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.18.JPG)

- For Permissions, select Create a new role from AWS policy templates.
- Give a policy name.
- Select Amazon SNS publish policy from the list.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.19.JPG)

- Create function.
- Delete all the code form the Code space and paste the below code:
    ```
    import boto3

    def lambda_handler(event, context):
        sns = boto3.client('sns')
        accountid = context.invoked_function_arn.split(":")[4]
        region = context.invoked_function_arn.split(":")[3]
        for record in event['Records']:
            message = record['dynamodb']['NewImage']
            catName = message['catName']['S']

            response = sns.publish(
                TopicArn=f'arn:aws:sns:{region}:{accountid}:Adoption-Alerts',
                Message=f"{catName} has been adopted!",
                Subject='Cat adopted!',
            )
    ```
- This code basically iterates through all the records that DynamoDB passes to the Lambda function, and then publishes a message to SNS.
- Click Deploy to save the function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.20.JPG)

Go to the Configurations tab, click on Permissions from the left side and click on the Execution role. this will take you to the role role in the IAM console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.21.JPG)

- Click on Add Permissions and then Attach Policies.
- Search for “AmazonDynamoDBFullAccess” and select it.
- Click Attach Policies.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.22.JPG)

- This is required so the Lambda function can read from the DynamoDB Stream. In the real world this should be locked down a lot further, but in this case, we’re okay with the Lambda having full DynamoDB permissions to all tables.

### Step 4 — DynamoDB Streams
- Go back to the DynamoDB table created earlier.
- Click on the Export and streams tab, then under DynamoDB stream details click Enable.
- On the next page, under View type, select New image. We only care about the new data, we don’t need the previous record data from before it was changed.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.23.JPG)

- Click Enable stream.
- Click Create trigger.
- Select the lambda function created earlier and Create the trigger.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.24.JPG)

### Step 5 — Testing and Cleanup
- In the DynamoDB table, select Scan and click Run.
- This will give result of all the cats in the table.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.25.JPG)

- Change one of the cat's adopted field to true. Basically what this does it we're implying that someone adopted the cat.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.26.JPG)

- Check the email to receive the notification.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/045/day45.27.JPG)


Delete the DynamoDB table, SNS Subscription and Topic, Lambda function, IAM role and CloudWatch logs (log name will be your lambda function name) to cleanup.

## ☁️ Cloud Outcome

Created a DynamoDB table and triggered it using Streams and Lambda to send notification using SNS. 

## Social Proof

[Blog](https://dev.to/aaditunni/dynamodb-triggers-using-streams-and-lambda-18p3)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7031187308804485120-kjJL?utm_source=share&utm_medium=member_desktop)