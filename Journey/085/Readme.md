# FREE AWS Cloud Project Bootcamp Week 5 Part 1

## Cloud Research

Today, I started the Week 6 of the FREE AWS Cloud Project Bootcamp by Andrew Brown.

I watched Ashish's Week 6 - Security Considerations video - Amazon ECS Security Best Practices. He talks about the Types of container services in AWS. One is using Virtual machine to run container and other is using AWS managed services like ECS, Fargate and EKS.
Then about business cases of it like ofr example running an application to a container using ECS. 
He then talks about how ECS works, Types of ECS Launches like using EC2 and Fargate.
There are shared risk with ECS which the user manages and AWS manages when it comes to ECS with EC@ and ECS with Fargate.

Some of the Security challenges with Fargate are:
- No visibility of Infrastructure.
- No file/network monitoring.
- Cannot run traditional security agents.
- User can run unverified container images.

Then he does a demo of walkthrough of ECs, and ECR scanning with Inspector Snyk.

A few of the security best practices are:
- Use VPC endpoints or Security groups with known sources only.
- Enable CloudTrail to monitor alerts on malicious ECS behavior.
- Choosing the right public or private ECS for images.
- ECR Scan Images to Scan on Push using basic or enhanced (Inspector + Snyk).
- Access Control -  Roles or IAM Users for ECS CLusters/Services/Tasks.
- No secrets/passwords in ECS Task Definitions/Containers.
- Use only trusted containers from ECR with no high/critical vulnerabilities.

I still have an error with Week 5 messaging part and haven't found the solution to it so left it there and will get back to it when I find what the issue is.

You can watch the Bootcamp through this YouTube playlist : [FREE AWS Cloud Project Bootcamp playlist](https://youtube.com/playlist?list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv)


## Social Proof

[Blog](https://dev.to/aaditunni/free-aws-cloud-project-bootcamp-week-6-part-1-lni)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7045862619831779328-VyFM?utm_source=share&utm_medium=member_desktop)