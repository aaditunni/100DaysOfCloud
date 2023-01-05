# Create a Lambda to do Arithmetic  Operations

## Introduction

Create an AWS Lambda with a language of your choice to do arithmetic operations on 2 numbers supplied as input and return the result.

## Prerequisite

AWS free tier account.

## Services Covered

- Lambda

## Try yourself

Search Lambda on your AWS console and select create function. 

### Step 1 — 
- Click Author from scratch.
- Give the function a name.
- Choose a language (you can choose Python and copy the code which I used).
- Click create function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/002/day2.JPG)

### Step 2 — 
- Clear the default code and write your own code or copy-paste the following code:
```
from __future__ import division
def lambda_handler(event, context):
   number1 = event['Number1']
   number2 = event['Number2']
   num1=int(number1)
   num2=int(number2)
   sum = num1 + num2
   product = num1 * num2
   difference = abs(num1 - num2)
   quotient = num1 / num2
   return {
       "Number1": num1,
       "Number2": num2,
       "Sum": sum,
       "Product": product,
       "Difference": difference,
       "Quotient": quotient
   }
```
- Once you insert your code, click on the dropdown next to Test and click Configure test event

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/002/day2.1.JPG)

### Step 3 — 
- Give and event name and paste the following (You can change the integer values as you want):
```
{
  "Number1": "10",
  "Number2": "20"
}
```
- Click create
- Deploy and click Test to see the results.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/002/day2.2.JPG)

## ☁️ Cloud Outcome

Created a lamda function to do arithmetic operation of two numbers.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/002/day2.3.JPG)


## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[Blog](https://dev.to/aaditunni/create-a-lambda-to-do-arithmethic-operations-38a0)

[Linkedin](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7015763513314799616-scJ3?utm_source=share&utm_medium=member_desktop)
