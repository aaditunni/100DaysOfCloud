![placeholder image](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/003/day3main.JPG)

# Create an EFS shared file system

## Introduction

Create an Elastic File System (EFS), mount it to two instances in two separate Availability Zones in the same Region, create a text file on one instance and access it from the other instance.

## Prerequisite

AWS free tier account.

## Use Case

EFS allows persistent storage and secure sharing of data. When you have applications or services running on multiple instances, EFS can serve as a common data source for the instances to access data securely.

## Services Covered

- Amazon EC2
- Amazon EFS

## Try yourself


### Step 1 —
- Create two EC2 instances in two seperate AZs in the same region.
    - AMI and Instance type - default (make sure it is free tier eligible).
    - Create a new Security Group that allows SSH inbound rules.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/003/day3.JPG)

### Step 2 — 
- Create a Security Group for NFS.
    - Give a name, description and for inbound rules select NFS as type and security group of the EC2 instance as the source.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/003/day3.1.JPG)

### Step 3 — 
- Search EFS and select create file system.
    - Give a name and click customize.
    - Deselect enable automatic backups.
    - Choose None in lifecycle management - Transition into IA and click Next.
    - Remove everything and keep only the Availability Zones that your EC2 instances are in and remove secuirty groups and select the security group you made for EFS.
    - No need to change anything else. Click Next till you click Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/003/day3.2.JPG)

### Step 4 — 
- Go to your EFS and choose the EFS you created, then click Attach. Copy the EFS mount helper command and save it for later.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/003/day3.3.JPG)

### Step 5 — 
- Go to your first EC2 instance and connect it using the EC2 Instance Connect and run these commands in the terminal :
    -   ```
        sudo yum install -y amazon-efs-utils
        ```
    -   ```
        mkdir data
        ```
    Paste the command you copied from the EFS console and replace efs with data    
    -   ```
        sudo mount -t efs -o tls fs-081089f63df60162f:/ data
        ```
        You can also create the directory 'efs' instead of 'data' if that's easier.
- Your EFS is mounted to the first EC2 Instance.
- Connect your second EC2 instance using EC2 Instance Connect and repeat the same commands as above to mount the EFS to your second EC2 instance.


## ☁️ Cloud Outcome

- Follow these commands to create a text file in one EC2 instance and access it from the second EC2 instance:
    
    - Give all permisions to the directory 'data' and it's contents.
      ```
        chmod 777 data/
      ``` 
    - Move to the directory 'data'.
      ```
        cd data
      ``` 
    - Create a file and add a line to it.
       ```
        sudo echo "This is a sample text" > filename.txt
       ``` 
    - Shows the content inside the textfile.
       ```
        cat filename.txt
       ``` 
    - Go to your second EC2 instance and move to the 'data' directory.
       ```
        cd data
       ```
    - List the contents in the directory.
       ```
        ls
       ``` 
    - Shows the content inside the textfile.
       ```
        cat filename.txt
       ``` 
This outcome shows that you were able to access a file you created on one EC2 instance through another EC2 instance hence showing that the EFS was mounted to both EC2 instance.

    

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/003/day3.5.JPG)

## Next Steps

Try creating a third EC2 instance and mounting the EFS to that.

## Social Proof

[Blog](https://dev.to/aaditunni/create-an-efs-shared-file-system-3abi)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7016108067880636416-lHft?utm_source=share&utm_medium=member_desktop)
