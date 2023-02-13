# RDS Backup and Restore using AWS Backup

## Introduction

Set up automated backups of select AWS services using Amazon Relational Database Service (Amazon RDS), restore a backup, and clean up our resources to avoid unexpected costs.

## Prerequisite

- AWS free tier account.
- RDS DB - My SQL

## Use Case

 Regulatory compliance obligations, business policies for data protection, and business continuity goals.

## Services Covered

- AWS Backup
- RDS

## Try yourself

### Step 1 — On-demand AWS Backup job
- Go to the AWS Backup console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.JPG)

- On the left side menu, under My account, choose Settings.
- On the Service opt-in page, choose Configure resources.
- On the Configure resources page, use the toggle switches to enable or disable the services used with AWS Backup
- Choose Confirm when your services are configured.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.1.JPG)

- Under My account on the left side menu, select Protected resources.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.2.JPG)

- From the dashboard, select the Create on-demand backup button.
- Select resource type as RDS and select the RDS DB name.
- Select Create backup now.
- For Retention period, put 1 Day.
- Select the Default Backup Vault.
- Select the Default IAM role.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.3.JPG)

- Select the Create on-demand backup button.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.4.JPG)

### Step 2 — Automating AWS Backup job
- Select Backup plans on the left navigation pane under My account, and then Create backup plan.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.5.JPG)

- Select Build a new plan.
- Give a unique backup plan name. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.6.JPG)

- Give a backup rule name. 
- Select the Default Backup vault.
- Select Daily for Backup frequency.
- Select the default Backup Window.
- For Retention period, put 1 Day.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.7.JPG)

- Select any other Region for Copy to destination. This will create a backup copy in another region.
- Select the Default Backup vault.
- For Retention period, put 1 Day.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.9.JPG)

- Create plan.

Assign resources to the backup plan
- Give a resource assignment name.
- Choose the default IAM role.
- For Define Resource selection, choose include specific resource types and select RDS as resource type and select the RDS DB name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.11.JPG)

- Assign resources.

### Step 3 — Restore from Backup
- Navigate to the backup vault that was selected in the backup plan and select the latest completed backup.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.12.JPG)

- Click on the Recovery point ID.
- Select Restore.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.13.JPG)

- Select the DB engine, License Model, and DB instance class (db.t3.micro).
- Select the storage type (gp2).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.14.JPG)

- Do not create a standby instance.
- Give a name for the new RDS DB instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.15.JPG)

- Select the VPC, subnet and select No for Public accessibility.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.16.JPG)

- For Database options, Enable IAM DB authentication.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.17.JPG)

- Enable encryption.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.18.JPG)

 - Leave the rest all as default configurations.
- Restore backup.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.19.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.20.JPG)

Once the restore job is completed, navigate to the Amazon RDS console and you can notice that a RDS database is created from the backup.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.21.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/044/day44.22.JPG)

### Step 4 — Cleanup
- Delete both the RDS DB instances.
- Delete Recovery point from Backup Vault.
- Delete Resource assignment from backup plans and then delete the Backup plan.

## ☁️ Cloud Outcome

Created an automated backup of Amazon Relational Database Service (Amazon RDS) and restored the backup.

## Social Proof

[Blog](https://dev.to/aaditunni/rds-backup-and-restore-using-aws-backup-el0)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7030842056512024577-YOMK?utm_source=share&utm_medium=member_desktop)
