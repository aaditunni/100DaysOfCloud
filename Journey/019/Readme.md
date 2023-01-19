# Create a Redis Cluster for scalability and high availability

## Introduction

 Create and configure a Redis Cluster with ElastiCache. With cluster mode enabled, your Redis Cluster gains enhanced scalability and high availability. You can start small and easily scale your Redis data as your application grows, and by setting up replicas in different availability zones you can also increase your read capacity. 

## Prerequisite

AWS free tier account.

## Use Case

It can offer frequently requested items at sub-millisecond response times. Popular uses for ElastiCache for Redis include the caching of database query results, persistent session caching, and full-page caching.

## Services Covered

AWS ElastiCache for Redis

## Try yourself

### Step 1 — Create EC2 instance and install Redis client.
- Go to EC2 console.
- Create an EC2 instance that is free tier eligible (AMI - Amazon Linux, Instance type - t2.micro, create a Key Pair, a security group that allows ssh).
- Connect to the instance using EC2 Instance connect.
- Run the following commands :
    ```
    sudo yum install gcc
    ```
     ```
    wget http://download.redis.io/redis-stable.tar.gz
    tar xvzf redis-stable.tar.gz
    cd redis-stable
    make
    ```

### Step 2 — Create your Redis Cluster
- Go to Elasticache dashboard.
- Select the region you want to launch the Redis Cluster.
- Click on Get started.
- Click on Create Cluster and select Create Redis Cluster.
- Select Configure and create a new Cluster under Cluster creation method.
- In Cluster Mode, select Enabled.
- Give a name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.JPG)

- Select AWS Cloud as location.
- Enable Multi - AZ (Each master node will be created in a different availability zone, and each of its replicas will also be allocated to a different availability zone. This is a best practice for improved reliability).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.1.JPG)

- Under Cluster setting, 
    - Choose node type as t2.micro (for a production cluster the size of the node should depend on your workload and you should start with the m5 or r5 instance families).
    - In Number of Shards, select 3. It means the data will be partitioned in three different master nodes. 
    - In Replicas per Shard, select 2. It means each master node will have two replicas. In case of a failure, an automatic failover will be triggered and one of the replicas will take over the role of the master node.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.2.JPG)

   - Create a new Subnet group.
- Leave everything else as default configuration.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.3.JPG)

- Open EC2 console in an another tab, go to Security groups and create a security group with the below inbound rule.
    - Allow incoming TCP connection on Port 6379 with source as the security group of the EC2 instance created earlier.
- Now, go back to the Redis cluster creation page,click on Manage security group and refresh to select the newly created security group.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.5.JPG)

- Uncheck Enable automatic backups.
- Leave everything else as default configurations.
- Click Next and Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.6.JPG)

### Step 3 — Connect to Redis
- Click on the Redis cluster.
- Under Details, copy the Configuration Endpoint.
- Run this command to connect to your Redis node:
    ```
    src/redis-cli -c -h endpoint -p 6379
    ```
    - Replace endpoint with your Configuration Endpoint.
    - If it doesn't connect then remove ':6379' from the endpoint and run the above command.
- Test the connection with a PING.
    - Type PING and run it to get PONG as reply.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.7.JPG)

### Step 4 — Failover
- Now you have a working and healthy Redis Cluster. One of the features of the cluster mode is the fact that if a node dies, the cluster can heal itself. In order to test that claim, you can trigger a manual failover and the following will happen: a read-replica will be selected to take over the the role of the master, and once the failover is executed you will be able to connect to the new master. In the meantime, a new replica will be added automatically so that the cluster will still have one master and two replicas.
- Check the role of the endpoint by entering and running ROLE .
- You want to connect to the master. If you didn't connect to a master, try with a different endpoint. You have three chances to get it right. You can know if it is the Master or not when you run ROLE command and it will list Master as the 1st.
- If it is not the Master then connect to another redis node from the 3 shards created.
- Once you have located the master node, select any of the nodes, click on “Action” and select “Failover primary”.
- One of the replicas will become the new master, and the number of replicas per master will be restored. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.8.JPG)

You can run the CLUSTER NODES command to verify what is happening.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/019/day19.9.JPG)

- Delete the Redis cluster (select No for Create backup) and terminate the EC2 instance to cleanup.

## ☁️ Cloud Outcome

Created a Redis Cluster with cluster mode enabled. The nodes were spread across availability zones and configured with automatic failovers. 

## Social Proof

[Blog](https://dev.to/aaditunni/create-a-redis-cluster-for-scalability-and-high-availability-7f4)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7021789470492401665-RURn?utm_source=share&utm_medium=member_desktop)
