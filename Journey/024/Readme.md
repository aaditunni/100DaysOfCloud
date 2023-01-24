# Setting up a Document Database with Amazon DocumentDB (with MongoDB compatibility) and AWS Cloud9

## Introduction

 Create a DocumentDB and learn how to connect to your Amazon DocumentDB cluster from your AWS Cloud9 environment with a mongo shell and run a few queries.

## Prerequisite

AWS free tier account (For new DocumentDB customers, this should not cost anything).

## Use Case

Amazon DocumentDB (with MongoDB compatibility) is a fast, scalable, highly available, and fully managed document database service that supports MongoDB workloads, and makes it easy to store, query, and index JSON data.

## Services Covered

- Amazon DocumentDB (with MongoDB compatibility)
- AWS Cloud9

## Try yourself

### Step 1 — Create an AWS Cloud9 Environment
- Go to the AWS Cloud9 console.
- Choose Create environment.
- Give a name.
- In environment type, select New EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.JPG)

- Select t2.micro as instance type.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.1.JPG)

- Leave the rest as default configurations and Create environment.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.2.JPG)

### Step 2 — Create a security group for DocumentDB
- Go to the EC2 console, under Network & Security, choose Security groups.
- Choose Create security group.
- Give a security group name.
- For Description, enter a description.
- For VPC, select the default VPC
- In the Inbound rules section, choose Add rule.
- For Type, choose Custom TCP Rule.
- For Port Range, enter 27017.
- For source, select the security group for the AWS Cloud9 environment you just created.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.3.JPG)

### Step 3 — Create an Amazon DocumentDB cluster
- Go to the DocumentDB console, under Clusters, choose Create.
- Under Cluster type, select Instance based CLuster.
In Configuration, select db.t3.medium (free tier eligible) under Instance class, and then choose 1 for Number of instances. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.4.JPG)

- Leave other settings at their default.
- Under Authentication, enter a username and password.
- Turn on Show advanced settings.
- Under Network settings, for VPC security groups, choose the security group we created earlier for the DocumentDB.
- Choose Create cluster.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.5.JPG)

### Step 4 — Install the mongo shell
- Go to the AWS Cloud9 console, under Your environments, choose the environment created earlier.
- Choose open IDE.
- At the command prompt, create the repository file with the following command:
    ```
    echo -e "[mongodb-org-3.6] \nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/amazon/2013.03/mongodb-org/3.6/x86_64/\ngpgcheck=1 \nenabled=1 \ngpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc" | sudo tee /etc/yum.repos.d/mongodb-org-3.6.repo
    ```
- Install the mongo shell with the following command:
    ```
    sudo yum install -y mongodb-org-shell
    ```
- To encrypt data in transit, download the CA certificate for DocumentDB by running this command:
    ```
    wget https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem
    ```
### Step 5 — Connect to your Amazon DocumentDB Cluster
- Go to the cluster that was created and click on it.
-  Copy the connection string provided under “Connect to this cluster with the mongo shell”.
- Omit < insertYourPassword> so that you are prompted for the password by the mongo shell when you connect. This way, you don’t have to type your password in cleartext.
- When you enter your password and can see the rs0:PRIMARY> prompt, you are successfully connected to your Amazon DocumentDB cluster.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.6.JPG)

### Step 6 — Insert and query data
 Run a few queries to get familiar with using a document database.
- To insert a single document, enter the following code:
    ```
    db.collection.insert({"hello":"DocumentDB"})
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.7.JPG)

- You can read the document that you wrote with the findOne() command :
    ```
    db.collection.findOne()
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.8.JPG)

- To perform a few more queries, consider a gaming profiles use case. Insert a few entries into a collection entitled profiles :
    ```
    db.profiles.insertMany([

    { "_id" : 1, "name" : "Tim", "status": "active", "level": 12, "score":202},

    { "_id" : 2, "name" : "Justin", "status": "inactive", "level": 2, "score":9},

    { "_id" : 3, "name" : "Beth", "status": "active", "level": 7, "score":87},

    { "_id" : 4, "name" : "Jesse", "status": "active", "level": 3, "score":27}

    ])    
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.9.JPG)

- Use the find() command to return all the documents in the profiles collection :
    ```
    db.profiles.find()
    ```
- Use a query for a single document using a filter :
    ```
    db.profiles.find({name: "Jesse"})
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.10.JPG)

- A common use case in gaming is finding a profile for a given user and increment a value in the user’s profile. In this scenario, you want to run a promotion for the top active gamers. If the gamer fills out a survey, you increase their score by +10. To do that, use the findAndModify command. In this use case, the user Tim received and completed a survey. To give Tim the credit to their score, enter the following code:
    ```
    db.profiles.findAndModify({

    query: { name: "Tim", status: "active"},

    update: { $inc: { score: 10 } }

    }) 
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.11.JPG)

- You can verify the result with the following query:
    ```
    db.profiles.find({name: "Tim"})
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/024/day24.12.JPG)

- Delete the resources to cleanup.
- Disable deletion termination for the DocumentDB. Under scheduling of modification, select Apply immediately.
- Delete and select No for Create final snapshot.
- Delete security group of DocumentDB.
- Delete the CLoud9 environment.

## ☁️ Cloud Outcome

Able to install the mongo shell, create an Amazon DocumentDB cluster, connect to your cluster, and perform a few queries, seeing how easy it is to insert and query JSON documents within Amazon DocumentDB.

## Social Proof

[Blog](https://dev.to/aaditunni/setting-up-a-document-database-with-amazon-documentdb-with-mongodb-compatibility-and-aws-cloud9-55ma)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7023628082229972992-Be-Y?utm_source=share&utm_medium=member_desktop)
