# Analyze Video and Extract Data using Amazon Rekognition

## Introduction

Analyze a sample video and extract metadata from it.

## Prerequisite

- AWS free tier account.
- A sample video to analyze. You can download this [sample video](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/066/pexels-yan-krukov-7791920.mp4) and use it too. Don't forget to unzip it.

## Use Case

Analyzing video automatically extracts rich metadata, which can be used to create a searchable video library, perform content moderation, or provide personalized VIP experiences.

## Services Covered

- Rekognition

## Try yourself

### Step 1 — Summary of Step
- Go to the Rekognition console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/066/day66.JPG)

- Click the down arrow under Choose a sample or upload your own, click Your own video, and select any video footage you have or the one you downloaded.
- It will take some time to analyze the video.
- Once analyzed, you will see the results.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/066/day66.1.JPG)

- Click on People to see the faces detected.
- Clicking on a face will show you where the face was shown throughout the video.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/066/day66.2.JPG)

- If they are celebrities then they will be shown under Celebrities tab.
- Clicking on Objects and Activities will let you choose from a list of  objects or activities from the video and selecting one will show you where all it is present in the video.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/066/day66.3.JPG)

Cleanup:
- Go to S3 console and you'll notice a bucket name with recognition in the name. Empty and then delete the bucket.


## ☁️ Cloud Outcome

Analyzed a sample video and extracted metadata using Amazon Rekognition.

## Social Proof

[Blog](https://dev.to/aaditunni/analyze-video-and-extract-data-using-amazon-rekognition-4j1m)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7038982238436270080-wMnx?utm_source=share&utm_medium=member_desktop)
