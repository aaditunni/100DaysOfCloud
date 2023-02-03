# Create business intelligence dashboards with Amazon QuickSight

## Introduction

Create a dataset, prepare the data, create an analysis, create a visual, modify the visual, add more visuals, publish as dashboard and delete your AWS resources.

## Prerequisite

AWS free-tier account.

## Use Case

Amazon QuickSight is fast, cloud-powered business intelligence service that can help you to build visualizations, perform ad hoc analysis, and get business insights from your data.

## Services Covered

Amazon QuickSight

## Try yourself

### Step 1 — Amazon QuickSight account
- Open the Amazon QuickSight landing page and choose Sign up for QuickSight.
- On the Create your QuickSight account screen, for Edition, choose Standard and choose Continue.
- Type a QuickSight account name and email address and keep the remaining default selections. Choose Finish.
- Go to Amazon QuickSight to open Amazon QuickSight in the AWS Management Console.

### Step 2 — Create the dataset
Download this [file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/tree/main/Journey/034/Sales%20Orders) and unzip it.
- Click on Datasets on left side menu.
- Click on New Dataset.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.JPG)

- On the Data sources dashboard, select Upload a file.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.1.JPG)

- Select the Sales Order.xlsx file that was downloaded.
- In the Choose your sheet dialog box, select orders and then choose Edit/Preview data.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.2.JPG)

### Step 3 — Prepare the data
- Under Fields, click on Row ID options and select exclude field.
- Then choose Add calculated field.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.3.JPG)

- In the Edit calculated field dialog, in the Functions list, choose dateDiff and double click it.
- In the Field list, choose Order Date by double clicking it, type a comma, and choose Ship Date by double clicking it. 
- Give the name for the Calculated field as Processing Time. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.4.JPG)

- CLick Save.
- Click Save and Publish.

### Step 4 — Create an analysis
- Goto the Datasets in QuickSight dashboard.
- Click on the Sales  Order Dataset option and click Create analysis.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.5.JPG)

- Click Create.


### Step 5 — Create a visual
- In the Fields list, choose Customer ID. The AutoGraph generates showing a Count of Records by Customer ID.
- Resize the window if needed.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.6.JPG)

- In the Fields list, choose Order Priority. Now, you have a count of records by priority placed by each customer.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.7.JPG)

- In the left menu bar, choose Filter, then choose Create one and select Country.
- Select the Country filter to edit it. In the Edit filter dialog, select the check box for United States and choose Apply. Then, choose Close. Now, the count of records by priority and customer ID are displayed only for the United States.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.9.JPG)

- Click on + icon next to Sheet 1 and click Add to create a new visual for the same analysis.
- For Visual types, choose Pie chart, and in the Fields list, choose Region.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.10.JPG)

- Click on + icon to create a third visual. For Visual type, choose Stacked area line chart. In the Fields list, choose Processing Time. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.11.JPG)

### Step 6 — Publish as a dashboard
- On the Sales Orders analysis workspace, choose Share, then choose Publish dashboard.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.12.JPG)

- Give the dashboard a name and choose Publish dashboard.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.13.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/034/day34.14.JPG)

CLeanup :
- Delete the dashboard.
- Delete the analysis.
- Delete the dataset.


## ☁️ Cloud Outcome

 Created data analyses, visualized the data, and shared the analyses through data dashboards using Amazon QuickSight.

## Social Proof

[Blog](https://dev.to/aaditunni/create-business-intelligence-dashboards-with-amazon-quicksight-pni)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7027195784169172992-QqX0?utm_source=share&utm_medium=member_desktop)
