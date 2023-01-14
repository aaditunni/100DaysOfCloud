# Deploy Docker Containers on Amazon ECS

## Introduction

Run a Docker-enabled sample application on an Amazon ECS cluster behind a load balancer and test the sample application.

## Prerequisite

AWS account.

## Use Case

Amazon Elastic Container Service (Amazon ECS) is the AWS service you use to run Docker applications on a scalable cluster.

## Services Covered

- Amazon Elastic Container Service (Amazon ECS)

## Try yourself

### Step 1 — 
Create container and task definition
- Search ECS (disable the New ECS experience) and click Get started.
- In Container definition field, select sample-app.
- Leave the Task definition with default configuration and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.JPG)

### Step 2 — 
Define your service
- In the Define your service section, select Application Load Balance under Load Balancer type.
- Leave everything else as default configuration and click Next.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.1.JPG)

### Step 3 — 
Configure your cluster
- Enter a Cluster name and click Next.
- Select Create to create.
- Wait for it to launch and after the launch is complete, click View service. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.2.JPG)

### Step 4 — 
Under the Details tab, click on the entry under Target Group Name. It will redirect you to the Target group page.
- Click on the target group name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.3.JPG)

In the Details section, click on the Load Balancer link. This link will take you to the Load Balancer that the container uses.
- Click and open the Load balancer.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.4.JPG)

In the Details tab, copy the DNS name, paste it into a new browser tab and hit enter to view the sample application.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.5.JPG)

Go back to the ECS console, select the cluster name and click Delete to cleanup.

## ☁️ Cloud Outcome

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/014/day14.6.JPG)

Learned how to configure and deploy your Docker-enabled application to Amazon ECS

## Social Proof

[Blog](https://dev.to/aaditunni/deploy-docker-containers-on-amazon-ecs-ljk)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7020092564041084928-mljc?utm_source=share&utm_medium=member_desktop)
