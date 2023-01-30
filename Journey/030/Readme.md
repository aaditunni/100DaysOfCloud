# Analyze and Compare images using Amazon Rekognition

## Introduction

Use Amazon Rekognition to analyze an image and then compare it to other images to see if the faces are the same.

## Prerequisite

AWS free tier account.

## Use Case

Identifying persons of interest, cataloging a digital library, creating a face-based employee verification system, or performing sentiment analysis.

## Services Covered

Amazon Rekognition

## Try yourself

### Step 1 — Analyzing faces
- Go to the Amazon Rekognition console.
- Select Facial analysis on the left side menu. 
    - This feature allows you to analyze faces in an image and receive a JSON response.
- Download [this picture](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/pic1.jpg) and unzip it.
- Click the Upload button and select the image you just downloaded.
- Under the Results dropdown, click through and see quick results for each face that was detected.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/day30.1.JPG)

- Click on the Response dropdown to see the JSON results. Notice that under the emotions results, there are numerous detected emotions.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/day30.2.JPG)

### Step 2 — Comparing different faces
- Select Face comparison in the left side menu.
- Download [this picture](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/pic2.jpg) and unzip it.
- Click on the Upload button for the reference face and select the image you just downloaded.
- Click on the Upload button for the comparison face and select the first image that was downloaded in Step 1.
- In Results dropdown,  see that the reference wasn’t a match for any of the detected faces in the comparison faces image.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/day30.3.JPG)

- Click on the Response dropdown to see the JSON results. Notice that the “Similarity” score for each of the detected faces never exceeds 1. The similarity score ranges from 1-100 and the threshold can be adjusted when using the API.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/day30.4.JPG)

### Step 3 — Comparing same faces
- Download [this picture](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/pic3.jpg) and unzip it.
- Click on the Upload button for the reference face and select the image you just downloaded.
- You can notice that the reference face that was compared to the other photo detected a 99% similarity score and detected that all other faces were not a match.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/day30.5.JPG)

- Click on the Response dropdown to see the details of each comparison.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/030/day30.6.JPG)

## ☁️ Cloud Outcome

 Analyze and compared faces using Amazon Rekognition.

## Social Proof

[Blog](https://dev.to/aaditunni/analyze-and-compare-images-using-amazon-rekognition-3h06)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7025757716392337408-qT10?utm_source=share&utm_medium=member_desktop)
