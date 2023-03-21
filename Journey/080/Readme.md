# FREE AWS Cloud Project Bootcamp Week 5 Part 1

## Cloud Research

Today, I started the Week 5 of the FREE AWS Cloud Project Bootcamp by Andrew Brown.

I watched Ashish's Week 5 - Security Considerations video - How to use Amazon DynamoDB for security and speed. He talks about Non-Relational Databases or NoSQL Databases namely DynamoDB. NoSQL DB gives the same performance level irrespective of the workload. Then he talks about the various use cases like banking and finance, gaming, media and entertainment, etc.
Then he talks about the Business use case of it and shows a demo on creating a DynamoDB Database and connect to it securely. He even implements DynamoDB Accelerator (DAX). 
He later talks about the Network Access Types to DynamoDB. Using Internet Gateway or NAT Gateway but it is not recommended best practice. Other ways are using VPC/Gateway endpoint which doesn't need to go outside to the Internet for other services to access the DB, DAX, cross account with IAM role, etc

A few of the security best practices are:
- Use VPC to create a private network from your application or Lambda to a DynamoDB. It helps to prevent unauthorized access to our instance from public internet.
- DynamoDD should only be in AWS regions that you are legally allowed to be holding user data in.
- Enable CloudTrail to monitor alerts on malicious DynamoDB behavior.
- Don't let the DynamoDB be internet accessible.
- Use appropriate IAM authentications.
- Use Organizations SCP to manage DynamoDB Table deletion, creation, region lock, etc

I still have an error in Week 4 with creating activities. Tried to solve it today and spent quite a lot of time on it but still could'nt figure it out. In the process, I came across a few silly errors and corrected them. Need to solve this as soon as possible to get on with the rest of Week 5

You can watch the Bootcamp through this YouTube playlist : [FREE AWS Cloud Project Bootcamp playlist](https://youtube.com/playlist?list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv)


## Social Proof

[Blog](https://dev.to/aaditunni/free-aws-cloud-project-bootcamp-week-5-part-1-1kfb)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7044030934878162944-fl-P?utm_source=share&utm_medium=member_desktop)