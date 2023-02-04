# Amazon S3 Intelligent-Tiering

## Introduction

In this, an S3 bucket is created an object is uploaded directly to the Amazon S3 Intelligent-Tiering storage class.
Then later a Lifecycle rule is created for the bucket so that any objects uploaded into the bucket will transition immediately to the Amazon S3 Intelligent-Tiering storage class. Also, an Intelligent-Tiering Archive configuration is created with tags and based on the tag value of an object created, objects in the S3 Intelligent-Tiering storage class can be archived to the Deep Archive Access tier after they haven’t been accessed for a time between six months and two years based on the time period we mention. 

## Prerequisite

AWS account.

## Use Case

Amazon S3 Intelligent-Tiering is an Amazon S3 storage class designed to optimize storage costs by automatically moving data to the most cost-effective access tier when access patterns change, without performance impact or operational overhead. S3 Intelligent-Tiering is the ideal storage class for data with unknown, changing, or unpredictable access patterns, independent of object size or retention period.

## Services Used

S3

## Try yourself

### Step 1 — Create an S3 bucket.
- Go to S3 console.
- Click Create Bucket.
- Give an unique name and select the region you want your bucket to be in.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.JPG)

- Leave the remaining options as default configuration.
- Create bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.3.JPG)

### Step 2 — Upload an object to S3 Intelligent-Tiering storage class. 
- Open the S3 bucket created.
- Click on the Objects Tab.
- Click Upload
- Choose Add files.
- Choose any file you want.
- In the Properties section, select Intelligent-Tiering

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.4.JPG)

- Leave the rest of the options on the default settings, and choose Upload.
- You have successfully uploaded your file to your bucket using the S3 Intelligent-Tiering storage class.

### Step 3 — Transition objects into S3 Intelligent-Tiering storage class using Lifecycle rule.
Transition objects that are already stored in the S3 Standard or in the S3 Standard-IA storage classes to the S3 Intelligent-Tiering storage class.
- Select the bucket name of the bucket you created.
- Select the Management tab and then select Create lifecycle rule in the Lifecycle rules section.
- Enter a Lifecycle rule name.
- Select Apply to all objects in the bucket for Choose a rule scope.
- Select the I acknowledge that this rule will apply to all objects in the bucket checkbox.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.5.JPG)

- In the Lifecycle rule actions, select the Move current versions of objects between storage classes checkbox.
- In the Transition current versions of objects between storage classes section, select Intelligent-Tiering as Choose storage class transitions, and enter 0 as Days after object creation.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.6.JPG)

- Create rule.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.7.JPG)

### Step 4 — Create Intelligent-Tiering Archive configurations.
- Select the bucket name of the bucket you created.
- Select the Properties tab.
- Navigate to the Intelligent-Tiering Archive configurations section and choose Create configuration.
- In the Archive configuration settings section, specify a Configuration name for your S3 Intelligent-Tiering Archive configuration.
- Under Choose a configuration scope, select Limit the scope of this configuration using one or more filters.
- In the Object Tags section, choose Add tag, and enter “opt-in-archive” as Key and “true” as Value of the tag.
- Make sure that the Status of the configuration is Enable.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.8.JPG)

- In the Archive rule actions section, select Deep Archive Access tier, enter 180 as number of consecutive days without access before archiving the objects to the Deep Archive Access tier.
- Choose Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.9.JPG)

Now if you upload an object with storage class as Intelligent-Tiering and add a tag with Key “opt-in-archive” and Value “true”, then after 180 days of objects not being accessed then it will be archived to the Deep Archive Access tier. To access the files after it has been archived we need to initiate the restore request and wait until the object is moved into the Frequent Access tier.
The Bulk retrieval typically completes within 48 hours while the Standard retrieval typically completes within 12 hours; both options are available at no charge.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.11.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.12.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/035/day35.13.JPG)


### Step 5 — Cleanup
Empty the bucket and delete the bucket.

## ☁️ Cloud Outcome

Created an Amazon S3 bucket, uploaded objects to the Amazon S3 Intelligent-Tiering storage class and activated the optional Deep Archive Access tier.

## Social Proof

[Blog](https://dev.to/aaditunni/amazon-s3-intelligent-tiering-1f93)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7027619153825906689-5P3X?utm_source=share&utm_medium=member_desktop)
