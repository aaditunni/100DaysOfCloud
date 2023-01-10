# Create a Serverless Reminder App

## Introduction

In this, we will be implementing a serverless reminder application. The Web application will load from an S3 bucket as a static website and run in browser, communicate with Lambda and Step functions via an API Gateway Endpoint. Using the application you will be able to configure reminders to be send using email.

This was inspired by on eof the projects by Adrian Cantrill. You can check out his project which has detailed explanation: 
https://github.com/acantril/learn-cantrill-io-labs/tree/master/aws-serverless-pet-cuddle-o-tron 

## Prerequisite

AWS account.

## Services Covered

- Simple Email Service (SES)
- CloudFormation
- IAM
- Lambda
- Step Functions
- API Gateway
- S3  

## Try yourself

Make sure the Region is set to US East (N. Virginia) - us-east-1 while using all the services.

### Step 1 — Configure Simple Email Service (SES)
We need two email for this. One for the web app to send the email from and another for the reminder from the app to be sent to (user).
- Search SES. Click verified identities on left side.
- Create identity.
- Check the 'Email Address' checkbox.
- Enter the email address that you want the application to use to send the reminder.
- Create Identity.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.JPG)

Create one more Identity with the email you want the reminder to be received.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.2.JPG)

### Step 2 — Add a email Lambda function to use SES to send emails for the serverless application
In this stage, We need to create a Lambda function which will be used by the serverless application to create an email and then send it using SES. 
Before that we need to create an IAM role which the Lambda will use to interact with other AWS services.

- Create a Lambda Role in CLoudFormation using the [yaml file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/CloudFormation/lambdarolecfn.yaml).
    - Search CloudFormation and select Create Stack.
    - Select Template is Ready option and select upload a template file and choose the lambdarolecfn.yaml file and upload it and click Next.
    - Enter Stack name as LAMBDAROLE, click Next till you reach the last section.
    - In the last section, scroll down and check the 'I acknowledge that AWS CloudFormation might create IAM resources' box and then click Create Stack.

- Create the Lambda Function.
    - Search Lambda and click on Create Function.
    - Select Author from scratch.
    - For Function name enter email_reminder_lambda .
    - For runtime click the dropdown and select Python 3.9.
    - Expand Change default execution role, select Use an existing role, click the Existing role dropdown and pick LambdaRole.
    - Create the function.
    
    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.3.JPG)

    - In the lambda_function code box, select all the default code and delete it and paste the below code :
    
        ```  
                import boto3, os, json

                FROM_EMAIL_ADDRESS = 'REPLACE_ME'

                ses = boto3.client('ses')

                def lambda_handler(event, context):
                    # Print event data to logs .. 
                    print("Received event: " + json.dumps(event))
                    # Publish message directly to email, provided by EmailOnly or EmailPar TASK
                    ses.send_email( Source=FROM_EMAIL_ADDRESS,
                        Destination={ 'ToAddresses': [ event['Input']['email'] ] }, 
                        Message={ 'Subject': {'Data': 'What do you want me to REMIND you of?'},
                            'Body': {'Text': {'Data': event['Input']['message']}}
                        }
                    )
                    return 'Success!'
        ```   
        
    
    - Select REPLACE_ME and replace with the email we created to send the reminder from the application.
    - Click Deploy to configure the lambda function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.4.JPG)

### Step 3 — Implement and configure the state machine, the core of the application
In this stage, we need to create an IAM role which the state machine that we create will use to interact with other AWS services.

- Create a State Machine Role in CLoudFormation using the [yaml file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/CloudFormation/statemachinerole.yaml).
    - Search CloudFormation and select Create Stack.
    - Select Template is Ready option and select upload a template file and choose the statemachinerole.yaml file and upload it and click Next.
    - Enter Stack name as StateMachineRole, click Next till you reach the last section.
    - In the last section, scroll down and check the 'I acknowledge that AWS CloudFormation might create IAM resources' box and then click Create Stack.

- Create a the Step Function.
    - Search Step Function. On left hand side, select State Machines.
    - Click Create State Machines.
    - Select Write your workflow in code.
    - Scroll down for type select standard.
    - In Definition, select all the default code and paste the following code:
        ```
            {
            "Comment": "Remindo - using Lambda for email.",
            "StartAt": "Timer",
            "States": {
                "Timer": {
                "Type": "Wait",
                "SecondsPath": "$.waitSeconds",
                "Next": "Email"
                },
                "Email": {
                "Type" : "Task",
                "Resource": "arn:aws:states:::lambda:invoke",
                "Parameters": {
                    "FunctionName": "EMAIL_LAMBDA_ARN",
                    "Payload": {
                    "Input.$": "$"
                    }
                },
                "Next": "NextState"
                },
                "NextState": {
                "Type": "Pass",
                "End": true
                }
            }
            }
        ```
    - Replace EMAIL_LAMBDA_ARN with the ARN of the email_reminder_lambda function that we created earlier (it can be found in the function overview of that lambda function).

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.5.JPG)

    - Click next and for State machine name use Remindo.
    - Under Permissions, select Choose an existing role and select StateMachineRole from the dropdown (it should be the only one, if you have multiple select the correct one and there will be random which is fine as this was created by CloudFormation).
    - under Logging, change the Log Level to All.
    - Click Create state machine.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.6.JPG)

### Step 4 — Implement the API Gateway, API and supporting Lambda function
In this stage you will be creating the front end API for the serverless application. The front end loads from S3, runs in your browser and communicates with this API. It uses API Gateway for the API Endpoint, and this uses Lambda to provide the backing compute. First you will create the supporting Lambda function and then the API Gateway.

- Create the Lambda Function.
    - Search Lambda and click on Create Function.
    - Select Author from scratch.
    - For Function name enter api_lambda .
    - For runtime click the dropdown and select Python 3.9.
    - Expand Change default execution role, select Use an existing role, click the Existing role dropdown and pick LambdaRole.
    - Create the function.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.7.JPG)

    - In the lambda_function code box, select all the default code and delete it and paste the below code :

        ```
            # This code is a bit ...messy and includes some workarounds
            # It functions fine, but needs some cleanup
            # Checked the DecimalEncoder and Checks workarounds 20200402 and no progression towards fix

            import boto3, json, os, decimal

            SM_ARN = 'YOUR_STATEMACHINE_ARN'

            sm = boto3.client('stepfunctions')

            def lambda_handler(event, context):
                # Print event data to logs .. 
                print("Received event: " + json.dumps(event))

                # Load data coming from APIGateway
                data = json.loads(event['body'])
                data['waitSeconds'] = int(data['waitSeconds'])
                
                # Sanity check that all of the parameters we need have come through from API gateway
                # Mixture of optional and mandatory ones
                checks = []
                checks.append('waitSeconds' in data)
                checks.append(type(data['waitSeconds']) == int)
                checks.append('message' in data)

                # if any checks fail, return error to API Gateway to return to client
                if False in checks:
                    response = {
                        "statusCode": 400,
                        "headers": {"Access-Control-Allow-Origin":"*"},
                        "body": json.dumps( { "Status": "Success", "Reason": "Input failed validation" }, cls=DecimalEncoder )
                    }
                # If none, start the state machine execution and inform client of 2XX success :)
                else: 
                    sm.start_execution( stateMachineArn=SM_ARN, input=json.dumps(data, cls=DecimalEncoder) )
                    response = {
                        "statusCode": 200,
                        "headers": {"Access-Control-Allow-Origin":"*"},
                        "body": json.dumps( {"Status": "Success"}, cls=DecimalEncoder )
                    }
                return response

            # This is a workaround for: http://bugs.python.org/issue16535
            # Solution discussed on this thread https://stackoverflow.com/questions/11942364/typeerror-integer-is-not-json-serializable-when-serializing-json-in-python
            # https://stackoverflow.com/questions/1960516/python-json-serialize-a-decimal-object
            # Credit goes to the group :)
            class DecimalEncoder(json.JSONEncoder):
                def default(self, obj):
                    if isinstance(obj, decimal.Decimal):
                        return int(obj)
                    return super(DecimalEncoder, self).default(obj)
        ```

    - Select YOUR_STATEMACHINE_ARN and replace with the ARN of the State Machine we created (can be found in the Details of the State Machine created earlier).
    - Click Deploy to configure the lambda function.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.8.JPG)

- Create an API Gateway.
    - Click APIs on the menu on the left.
    - Locate the REST API box, and click Build.
    - Under Create new API ensure New API is selected.
    - For API name* enter remindo .
    - For Endpoint Type, pick Regional.
    - Click create API.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.9.JPG)

    - Click the Actions dropdown and Click Create Resource. 
    - Under resource name enter remindo .
    - Make sure that Configure as proxy resource is NOT ticked.
    - Tick Enable API Gateway CORS.
    - Click Create Resource.  
    
    <br>    

    - Ensure you have the /remindo resource selected, click Actions dropdown and click create method.
    - In the small dropdown box which appears below /remindo select POST and click the tick symbol next to it.
    - Ensure for Integration Type that Lambda Function is selected.
    - Make sure us-east-1 is selected for Lambda Region.
    - In the Lambda Function box, start typing api_lambda and it should autocomplete, click this auto complete (Make sure you pick api_lambda and not email reminder lambda).
    - Make sure that Use Default Timeout box is ticked.
    - Make sure that Use Lambda Proxy integration box is ticked.
    - Click Save

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.10.JPG)

    - Click Actions Dropdown and Deploy API. 
    - For Deployment Stage select New Stage.
     -For stage name and stage description enter prod.
     - Click Deploy.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.11.JPG)

### Step 5 — Implement the static frontend application and test functionality
In this stage, you will create an S3 bucket and static website hosting which will host the application front end. You will download the source files for the front end, configure them to connect to your specific API gateway and then upload them to S3. Finally, you will run some application tests to verify its functionality.

- Create S3 Bucket.
    - Search S3.
    - Click Create Bucket.
    - Give a unique bucket name and ensure the region is set to US East (N.Virginia) us-east-1 .
    - Uncheck Block all public access.
    - Tick the box under Turning off block all public access might result in this bucket and the objects within becoming public.
    - Create Bucket.

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


    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.12.JPG)

- Enable Static Hosting.
    - Click on the Properties Tab.
    - Scroll down and locate Static website hosting and click Edit.
    - Select Enable Select Host a static website.
    - For both Index Document and Error Document enter index.html.
    - Click ave Changes.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.13.JPG)

- Download and edit the [front end files](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/tree/main/Journey/010/Webpage).
    - Open the serverless.js in a code/text editor.
    - Locate the placeholder REPLACEME_API_GATEWAY_INVOKE_URL and replace it with your API Gateway Invoke URL(found in the API Gateway console) and at the end of this URL add /remindo - Save the file.
    - Return to the S3 console Click on the Objects Tab.
    - Click Upload
    - Drag the 4 files from the Webpage folder onto this tab, including the serverless.js file you just edited. 
    - Click Upload and wait for it to complete.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.14.JPG)      

- Testing.
    - Go to the S3 bucket, properties tab and scroll all the way down and click on the link (bucket website endpoint).
    - Enter the details. 
    - The email you should give is the email you created and verified in SES to receive the reminder.
    - Click Email.
    - Wait until the time period (seconds you mentioned) to receive the reminder as an email.
    - Check the email.

    ![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.15.JPG)

### Step 6 — Cleanup
- Move to the S3 console and select the bucket you created. Click Empty to empty the objects and then Click Delete to delete the S3 Bucket.

- Move to the API Gateway console. Check the box next to the remindo API. Click Actions and then Delete and then Click Delete.

- Move to the lambda console. Check the box next to email_reminder_lambda, click Actions, Click Delete and then Click Delete. Check the box next to api_lambda, click Actions, Click Delete and then Click Delete.

- Move to the Step Functions console. Check the box next to Remindo, Click Delete, then Delete state machine.

- Go to the SES console and verified identities. Select one of the indentities, Click Delete, then click Confirm. Pick the other verified identity, Click Delete, then click Confirm.

- Move to the CloudFormation console. Check the box next to StateMachineROLE , click Delete then Delete Stack, Check the box next to LAMBDAROLE , click Delete then Delete Stack.

## ☁️ Cloud Outcome
We have successfully created a Serverless Reminder App.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.16.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/010/Pictures/day10.17.JPG)


## Social Proof

[Blog](https://dev.to/aaditunni/create-a-serverless-reminder-app-4okj)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7018679945539284992--5hN?utm_source=share&utm_medium=member_desktop)
