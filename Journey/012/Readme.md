# Create a text transcript using Amazon Transcribe

## Introduction

Create a text transcript of a recorded audio file using Amazon Transcribe.

## Prerequisite

AWS free tier account.

## Use Case

Amazon Transcribe enables voice to text at scale. Use Amazon Transcribe for a wide range of audio or videos files, such as customer service calls, business meetings, broadcast TV, and on-demand videos.

## Services Covered

- S3
- Amazon Transcribe

## Try yourself

### Step 1 — 
Select a region that has Amazon Transcribe.
Search S3 and click Create bucket.
- Give an unique name, select the region and leave everything as default options.
- Create bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/day12.JPG)

### Step 2 — 
- Download the [audio file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/transcribe-sample.5fc2109bb28268d10fbc677e64b7e59256783d3c.mp3). Make sure you unzip the folder.
- Go to your bucket and to the Objects tab and click upload.
- Upload the audio file that you downloaded.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/day12.1.JPG)

### Step 3 — 
Search Transcribe.
- On left side, click Transcription jobs.
- Click Create job.
- Enter Name and leave everything else as default.
- In Input data, browse to your S3 bucket and select your audio file(object).
- Leave Output data as default and click Next.
- Leave everything else as default choice and create the job.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/day12.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/day12.3.JPG)

### Step 4 — 
Once it is created, wait for the Status to show Complete.
Open it and go to the Transcription preview to see the output.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/day12.4.JPG)

Empty the S3 bucket, then delete it and delete the Transcription job to cleanup.

## ☁️ Cloud Outcome

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/012/day12.5.JPG)



## Social Proof

[Blog](https://dev.to/aaditunni/create-a-text-transcript-using-amazon-transcribe-234l)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7019280905206706176-hQgs?utm_source=share&utm_medium=member_desktop)