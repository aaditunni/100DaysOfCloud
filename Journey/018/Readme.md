# Send Fanout Event Notifications with SQS and SNS

## Introduction

 Implement a fanout messaging scenario using Amazon Simple Notification Service (SNS) and Amazon Simple Queue Service (SQS). In this scenario, messages are pushed to multiple subscribers, which eliminates the need to periodically check or poll for updates and enables parallel asynchronous processing of the message by the subscribers.

 Assume that it is a supermarket delivery application that sends an Amazon SNS message to a topic whenever an order is placed.. The Amazon SQS queues that are subscribed to that topic will each receive identical notifications for the new order.

## Prerequisite

AWS free tier account.

## Services Covered

- Amazon Simple Notification Service (SNS).
- Amazon Simple Queue Service (SQS).

## Try yourself

### Step 1 — Create a SNS Topic
- Go to SNS console.
- Type Supermarket_Order as the Topic name and click Next step.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.JPG)

- Keep everything else as default and Create Topic

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.2.JPG)

### Step 2 — Create a SQS Queue
When you subscribe multiple queues to a topic, each queue receives identical notifications every time a message is pushed to the topic. Services attached to those queues can then process the orders asynchronously and in parallel.
Assume that there are services in the backend connected to the SQS Queues. Order_Inventory Queue is for the the processing of orders/Inventory management and Order_Analytics is for analysis of the orders.
- Go to SQS console.
- Click Create queue.
- Give a name (Order_Inventory), leave rest as default and Create queue.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.3.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.4.JPG)

- Similarly, create another queue with name (Order_Analytics).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.6.JPG)

### Step 3 — Subscribe the SQS Queues to the SNS Topic
- Go to the SQS Queues list and select each of the queues, click on Actions and click SUbscribe to Amazon SNS topic.
- Choose the topic created earlier and Save.
    - Your SNS topic appears in the list because you created it from the same account that you used to create your Amazon SQS queues. If the SNS topic was made by another account, you could subscribe to it by using the Topic ARN. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.7.JPG)

### Step 4 — Publish a Message to the Topic
- Go back to the SNS console.
- Go to topics and select the topic.
- In the Details page, click Publish message.
- Give a subject (Order No. 121).
- In the message field, enter text such as supermarket items to represent a sample order.
- Publish the message.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.8.JPG)

### Step 5 — Verify
Once a new message is published, Amazon SNS will deliver that message to every endpoint that is subscribed to the topic. In a fanout scenario like this one, the Amazon SQS queues are the endpoint.
- Go back to the SQS console.
- Select the Order_Inventory queue, click on Send and receive messages.
- Under Receive messages, click on Poll for messages.
- You'll notice a message received.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.9.JPG)


- Click on it and it will open the message and you can see it in the Body tab.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/018/day18.10.JPG)


- You have confirmed the order and can assume that the message has been processed and can now safely delete the message from the Queue.
- Select the message and click Delete.
- Repeat the same steps to see the message in Order_Analytics queue as well. Delete the message after confirming.

- Delete the SNS topic and SQS Queues to cleanup.

## ☁️ Cloud Outcome

Implemented a fanout scenario using Amazon SNS and Amazon SQS. 

## Social Proof

[Blog](https://dev.to/aaditunni/send-fanout-event-notifications-with-sqs-and-sns-44a)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7021403209105563648-bNLk?utm_source=share&utm_medium=member_desktop)
