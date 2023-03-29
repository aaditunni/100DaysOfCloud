# Building a REST API Gateway with path and query string parameters

## Introduction

Creating an API Gateway with path and query string parameters that will be integrated with Lambda function.

## Prerequisite

AWS free tier account.

## Services Covered

- Lambda
- API Gateway

## Try yourself

### Step 1 — Lambda
Go to the Lambda console.
- Create a Lambda Function.
    - Give a name.
    - Choose Python 3.9 as runtime language.
    - Create a new execution role from AWS policy template.
        - Give a name for the role.
        -Choose Basic Lambda@Edge permissions.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/088/day88.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/088/day88.1.JPG)

- Replace the code with the following and the deploy:
    ```
        import json

        
        def lambda_handler(event, context):

            #Path parameter passed to Lambda function

            servername = event["params"]["path"]["servername"]

            

            #Query string parameter passed to Lambda function

            Email_id = event['params']['querystring']['emailid']

            

            return {

                "Path Parameter" : servername,

                "Query string Parameter" : Email_id

            }
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/088/day88.2.JPG)

### Step 2 — API Gateway
Go to the API Gateway console.
- Build a New REST API Gateway. Give it a name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/088/day88.3.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/088/day88.4.JPG)

- Then under Actions choose Create Resource, give it a name and type that name as Resource Path then click on Create Resource.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/088/day88.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.6.JPG)

- Next create a Method in Actions and in drop down menu choose GET.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.7.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.8.JPG)

- As Integration type choose Lambda Function and then type the earlier created functions name.
- Check Use Lambda proxy integration.
- Save.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.9.JPG)

- On the Method Execution screen, choose Method Request and in the settings change the Request Validator field to Validate body, query string parameters and headers. Then under URL Query String Parameters add query string, enter emailid and make it Required.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.11.JPG)

- Go back to Method Execution page.
- Go to the Integration Request and under Mapping templates for the Request body passthrough choose When there are no templates defined and Add mapping template with value application/json. Then generate template Method Request passthrough.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.12.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.13.JPG)

- Go back, select the GET method resource and in Actions choose Deploy API.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.14.JPG)

- Choose the development stage and give it a name and deploy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/087/day88.15.JPG)

- Then under Stages choose the name resource and GET method and copy the URL. Now if you replace name with Production and add emailid (?emailid=anyemailid@whatever.com) you will receive GET request's response from the API.

### Step 3 — Cleanup

- Delete the Lambda function and the API.

## ☁️ Cloud Outcome

Created an API Gateway with path and query string parameters integrated with the Lambda function.

## Social Proof

[Blog](https://dev.to/aaditunni/building-a-rest-api-gateway-with-path-and-query-string-parameters-1kol)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7046935606022615040-uoCD?utm_source=share&utm_medium=member_desktop)
