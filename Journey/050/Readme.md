#  Automate the EBS Snapshot and AMIs creation using Amazon Data Lifecycle Manager

## Introduction

Data Life Cycle Manager is an easy an automated way to backup and delete data stored on EBS. 

## Prerequisite

- AWS free tier account.
- EC2 instance.

## Use Case

- Define policies to enforce backup on regular schedule.
- Use of tag to identify volumes and instance defined in the policies.
- Manage cost by deleting snapshots. 

## Services Covered

EC2

## Try yourself



### Step 1 — 
- Go to the EC2 console.
- On the left side menu, under Elastic Block Store, select Lifecycle Manager.
- Select EBS Snapshot Policy from policy type dropdown menu. - Click on Next step.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.JPG)

- You have an option to take snapshot of specific Volume by specifying the tag or at instance level which will take snapshot of all the EBS volumes associated with the instance.
- Select instance and select your instance from the tags.
- Click on Add.
- Give a policy description and select the Default role.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.2.JPG)

- Make sure policy status is Enabled.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.3.JPG)

- Click on Next. 
- Give your Schedule name.
 - Select the Frequency(Daily, Weekly, Monthly, Yearly or Custom cron expression).
 - You can schedule this snapshot every (1,2,3,4,6,8,12,24 hour).
 - Specify the starting time(NOTE timing is in UTC, so please adjust it based on the requirement as you don’t want your snapshot to be happen during Production hour).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.4.JPG)

 - Specify how many copies of snapshot you want to maintain. - Check Copy tags from source option so that snapshot will have same tag as your instance tag.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.5.JPG)

 - In the next screen, click on Create policy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.6.JPG)

- You need to wait atleast 1 hour after configuring the snapshot for it to be listed under Snapshot dashboard.
- Automating the process of AMI creation is similar to EBS Snapshot. For AMI creation from the drop-down choose EBS-backed AMI policy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/050/day50.7.JPG)

## ☁️ Cloud Outcome

Created an EBS Snapshot Policy to automate the EBS Snapshot using Amazon Data Lifecycle Manager.

## Next Steps

Try automating the AMIs creation using Amazon Data Lifecycle Manager.

## Social Proof

[Blog](https://dev.to/aaditunni/automate-the-ebs-snapshot-and-amis-creation-using-amazon-data-lifecycle-manager-2gih)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7033200907253166080-ooco?utm_source=share&utm_medium=member_desktop)
