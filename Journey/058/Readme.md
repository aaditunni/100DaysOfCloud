# Install Python modules for Lambda using Cloud9

## Introduction

For a Lambda function to work with some libraries it needs to be uploaded as a separate module as lambda doesn't isn't inbuilt with many python libraries. To do that a new Cloud9 environment will be started and in it a pip commands executed in terminal. Then modules will be packed and upload to lambda function.

## Prerequisite

- AWS account.

## Services Covered

- Lambda
- Cloud9

## Try yourself

### Step 1 — Lambda
- Go to the Lambda console and Create a new function.
- Give a function name.
- Select Python 3.9 as Runtime.
- Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.JPG)

- Delete the existing coe and paste the below code:
    ```
        import json

        import requests

        
        def lambda_handler(event, context):

            r = requests.get('https://github.com') 

            print(r.status_code)

            print(r.headers['Date'])

            print(r.headers['server'])

            return {

                'statusCode': 200,

                'body': json.dumps('success')

            }
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.1.JPG)

- Try to test it  and it will respond with error message because request package is not part of Lambda Python standard library.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.2.JPG)

### Step 2 — Cloud9
- Go to Cloud9 console and create a new environment.
- Give a name.
- Choose environment type as New EC2 instance. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.3.JPG)

- Leave everything for the instance configuration as default (free tier).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.4.JPG)

- Choose Connection as AWS System Manager (SSM).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.5.JPG)

- Create.
- On left side menu, click on the AWS logo and goto the Lambda function and right click and select download.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.6.JPG)

- Now, in your environment directory, select the downloaded Lambda function, right click and select Open Terminal Here.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.7.JPG)

- Run the below command in Terminal to install request library in (replace LAMBDA_NAME withe the name of your Lambda function) :
    ```
        pip install --target ~/environment/LAMBDA_NAME/ requests
    ```
- Then to zip the library run the below command to zip all the files:
    ```
        zip -r ../my-deployment-package.zip *
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.8.JPG)

- Right click on the newly created zip file and click Download to download that zip file.
- Back in Lambda upload that zip package and test your function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.9.JPG)

- You will get a success message.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/058/day58.10.JPG)

## ☁️ Cloud Outcome

Installed Python modules for Lambda using Cloud9.


## Social Proof

[Blog](https://dev.to/aaditunni/install-python-modules-for-lambda-using-cloud9-3kn5)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7036083024664379392-qsM4?utm_source=share&utm_medium=member_desktop)
