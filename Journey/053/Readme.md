# AWS UG Dubai Meetup #27

## Cloud Research

Today, I attended the AWS User Group Dubai Meetup #27 where two awesome speakers presented on rate limiting in a modern application world and Automation in Cloud.

In this session, I learnt 
 - Why rate limiting is significant in modern systems and why it’s important to design a better system that operates as a good neighbor to all the systems around it.
 To make sure an application is actually a good neighbor is by using limits so that the application is super modern and robust and can also process a lot of data but also doesn't cause damage to the other side. 
    - There are many reasons for using limits and one of them is the cost factor. For example Dynamo DB is a serverless NoSQL database that scales automatically but adding more an more data to the database without limits can incur a huge cost.
    - While using Lambda, we can limit the number of concurrent Lambda function by setting function concurrency. Another way is by using Event filtering in Lambda and it can filter the incoming messages before function invocation to reduce traffic to function and costs.
    - When handling RDS connections we can run into a problem like failure to get connection from the pool and for that we can use buffering, i.e is by using SQS or by using RS proxy which will reuse the connections to make sure the application is not exhausting all the connections.
    - Using API throttling to allocate capacity with quotas and request rates.
    - By using caching layer for frequently requested data.
    - Using asynchronous workloads.

- Introduction to Automation tools such as AWS CloudFormation, Ansible and other automation tools and with this we can automatically do repetitive tasks by using a few lines of code.
There was a demo on on cloud formation templates to create AWS VPC with resources in it, then using Ansible to create configuration files of AWS resources(Web/APPServer,DB) on on-premises systems.


## Social Proof

[Blog](https://dev.to/aaditunni/aws-ug-dubai-meetup-27-4nhl)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7034258184630382592-0SBC?utm_source=share&utm_medium=member_desktop)
