# Create a Cost Budget

## Introduction

Create a Cost Budget with  an alert threshold to send a notification to your email when it hits the threshold mark. 

## Prerequisite

AWS account.

## Use Case

You can use AWS Budgets to track and take action on your AWS costs and usage. You can use AWS Budgets to monitor your aggregate utilization and coverage metrics for your Reserved Instances (RIs) or Savings Plans.

## Services Covered

- AWS Budgets

## Try yourself

### Step 1 — 
- Search AWS Budgets and you will notice that you have no permissions to access it since you are using an IAM User. It is always recommended to use an IAM User instead of the root account.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/006/day6.JPG)

- Login to you root user and click on the dropdown menu next to your username and select Accounts.
- Scroll down to 'IAM User and Role Access to Billing Information' then check 'Activate IAM Access' and click Update.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/006/day6.1.JPG)

- Log back into the IAM User.

### Step 2 — 
Search AWS Budgets and select Create a Budget.
 - You can create a budget using a template provided which is configure according to some use cases. It's quite easy to set one up.
 - We are going to create a Budget using the 'Customize'.
 - Select Customize and select Cost Budget.
 - In Next, enter Budget name, select period as monthly, choose start month and enter the budgeted amount and leave everything else as default. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/006/day6.2.JPG)

### Step 3 — 
Add an alert threshold. Here we set two alert threshold.
 -  Alert 1 is when actual cost is greater than 80% of the budgeted amount.
 - Alert 2 is when forecasted cost is greater than 80% os the budgeted amount.
 - Enter the email you want to receive notifications of the Alerts.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/006/day6.3.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/006/day6.4.JPG)

If you creating a Budget using the Customize option then you'll have to specify the Alerts. You can change the values, period etc according to your needs.

If you creating a Budget using a template then your Alerts are pre configured but can be changed later. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/006/day6.5.JPG)

## ☁️ Cloud Outcome

Created a Cost Budget with an alert threshold to send a notification to your email when it hits the threshold mark.
## Social Proof

[Blog](https://dev.to/aaditunni/create-a-cost-budget-2jpc)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_create-a-cost-budget-activity-7017093569328898049-bDd-?utm_source=share&utm_medium=member_desktop)
