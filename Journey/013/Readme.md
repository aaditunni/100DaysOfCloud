# Create and Query a NoSQL Table with Amazon DynamoDB

## Introduction

Create a simple DynamoDB table, add data, scan and query the data, and delete the table.

## Prerequisite

AWS free tier account.

## Use Case

DynamoDB is a fully managed NoSQL database that supports both document and key-value store models. Its flexible data model, reliable performance, and automatic scaling of throughput capacity make it a great fit for mobile, web, gaming, ad tech, IoT, and many other applications.

## Services Covered

DynamoDB

## Try yourself

### Step 1 — 
Create DynamoDB table.
- Search DynamoDB and choose Create table.
- Enter Table name (Football).
- Enter Partition key (Country) and Sort key (Players) names. 
- In Table Settings, select Customize settings ad scroll down to the Auto Scaling and select On.
- Create the table.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/013/day13.JPG)

### Step 1 — 
Add data to the DynamoDB table.
- On left side menu, select Explore items and select the table.
- Under Items returned, select Create item.
- Enter the details (Country and Player name).
- Create Item.
- Repeat the process to add few more items to the table.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/013/day13.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/013/day13.2.JPG)

### Step 3 — 
Query the DynamoDB table.
- Click on the arrow next to Scan/Query Item.
- Select Query.
- You can query the table in many ways.
- In Country boc (Partition key), enter Argentina (or the name of the country you added) and choose Run. All the players from Argentina in the table will be listed.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/013/day13.3.JPG)

- Similarly, enter France (or another name of the country you added) and choose Run. All the players from France in the table will be listed.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/013/day13.4.JPG)

- We can narrow down the query results. In Country box, enter France and in Players boc (Sort key), select Begin with from the dropdown list and enter Z .
- Run the query an you'll notice that only players from France whose name starts with the letter Z is displayed.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/013/day13.5.JPG)

Go to Tables on the left side menu in DynamoDB console, select the table and click Delete to delete the table for cleanup.

## ☁️ Cloud Outcome

Created my first DynamoDB table, added items to the table, and then queried the table to find the items I wanted. I also learned how to visually manage my DynamoDB tables and items through the AWS Management Console.

## Social Proof

[Blog](https://dev.to/aaditunni/create-and-query-a-nosql-table-with-amazon-dynamodb-2ao1)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7019614343293997056-krbI?utm_source=share&utm_medium=member_desktop)
