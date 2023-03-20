# Build an Event-Driven Media Conversion Pipeline

## Introduction

Create a Video on Demand ("CatTube") backend that will take any uploaded video format to a source bucket, convert it to the format we require, and place the output video into another bucket ready to be served by Cloudfront. This is a micro project by Adrian Cantrill which I followed.

## Prerequisite

- AWS free tier account.

## Services Covered

- S3
- MediaConvert
- Lambda
- IAM
- CloudFront

## Try yourself

### Step 1 — S3
Go to the S3 console.
Create Source bucket.
- Give an unique name (day79-cattube-source) and leave the rest all as default configuration and create the bucket. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.JPG)

Create Destination bucket.
- Give an unique name (day79-cattube-destination) and leave the rest all as default configuration and create the bucket. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.2.JPG)

### Step 2 — MediaConvert
Go to the MediaConvert console.
- Click on Create queue on left side menu.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.3.JPG)

- Set the name to catqueue
- Click on Create queue

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.4.JPG)

### Step 3 — Lambda
Go to the Lambda console.
- Click Create function.
-Leave Author from scratch selected.
- Set the Function name to cattube-converter-function.
- Set the Runtime to Python 3.9.
- Leave the Architecture as x86_64.
- Click Create function.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.5.JPG)

- Under the Code tab, replace the default code with the following code (Replace the destination_bucket variable with the destination bucket name you chose):
    ```
        import json
        import boto3

        def lambda_handler(event, context):
            mediaconvert = boto3.client('mediaconvert')
            mediaconvert_endpoint = mediaconvert.describe_endpoints(MaxResults=1)
            mediaconvert = boto3.client('mediaconvert', endpoint_url=f"{mediaconvert_endpoint['Endpoints'][0]['Url']}")
            for message in event['Records']:
                        # REPLACE ME #
                destination_bucket = 'day79-cattube-destination'
                        ##############
                source_bucket = message['s3']['bucket']['name']
                object = message['s3']['object']['key']
                accountid = context.invoked_function_arn.split(":")[4]
                region = context.invoked_function_arn.split(":")[3]

                with open("job.json", "r") as jsonfile:
                    job_config = json.load(jsonfile)

                job_config['Queue'] = f"arn:aws:mediaconvert:{region}:{accountid}:queues/catqueue"
                job_config['Role'] = f"arn:aws:iam::{accountid}:role/MediaConvert_Default_Role"
                job_config['Settings']['Inputs'][0]['FileInput'] = f"s3://{source_bucket}/{object}"
                job_config['Settings']['OutputGroups'][0]['OutputGroupSettings']['FileGroupSettings']['Destination'] = f"s3://{destination_bucket}/"

                response = mediaconvert.create_job(**job_config)
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.6.JPG)

- Right click on the directory pane → New File.
- Set the file name to job.json
- Add the following config to the newly created file:
    ```
        {
        "Queue": "arn:aws:mediaconvert:region:account_id:queues/Default",
        "UserMetadata": {},
        "Role": "arn:aws:iam::account_id:role/MediaConvert_Default_Role",
        "Settings": {
            "TimecodeConfig": {
            "Source": "ZEROBASED"
            },
            "OutputGroups": [
            {
                "Name": "File Group",
                "Outputs": [
                {
                    "ContainerSettings": {
                    "Container": "MP4",
                    "Mp4Settings": {}
                    },
                    "VideoDescription": {
                    "CodecSettings": {
                        "Codec": "H_265",
                        "H265Settings": {
                        "MaxBitrate": 5000000,
                        "RateControlMode": "QVBR",
                        "SceneChangeDetect": "TRANSITION_DETECTION"
                        }
                    }
                    },
                    "AudioDescriptions": [
                    {
                        "AudioSourceName": "Audio Selector 1",
                        "CodecSettings": {
                        "Codec": "AAC",
                        "AacSettings": {
                            "Bitrate": 96000,
                            "CodingMode": "CODING_MODE_2_0",
                            "SampleRate": 48000
                        }
                        }
                    }
                    ],
                    "NameModifier": "-hd"
                },
                {
                    "ContainerSettings": {
                    "Container": "MP4",
                    "Mp4Settings": {}
                    },
                    "VideoDescription": {
                    "Width": 256,
                    "Height": 144,
                    "CodecSettings": {
                        "Codec": "AV1",
                        "Av1Settings": {
                        "RateControlMode": "QVBR",
                        "QvbrSettings": {},
                        "MaxBitrate": 100000
                        }
                    }
                    },
                    "AudioDescriptions": [
                    {
                        "CodecSettings": {
                        "Codec": "AAC",
                        "AacSettings": {
                            "Bitrate": 96000,
                            "CodingMode": "CODING_MODE_2_0",
                            "SampleRate": 48000
                        }
                        }
                    }
                    ],
                    "NameModifier": "-sd"
                }
                ],
                "OutputGroupSettings": {
                "Type": "FILE_GROUP_SETTINGS",
                "FileGroupSettings": {
                    "Destination": "s3://destination_bucket/"
                }
                }
            }
            ],
            "Inputs": [
            {
                "AudioSelectors": {
                "Audio Selector 1": {
                    "DefaultSelection": "DEFAULT"
                }
                },
                "VideoSelector": {},
                "TimecodeSource": "ZEROBASED",
                "FileInput": "s3://source_bucket/object.mp4"
            }
            ]
        },
        "AccelerationSettings": {
            "Mode": "DISABLED"
        },
        "StatusUpdateInterval": "SECONDS_60",
        "Priority": 0
        }
    ```
- This configuration will take a video input, and output two videos; an SD version (144p) and an HD version (as large as the original video), with the file names of each prepended with "-sd" and "-hd".

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.7.JPG)

- Click Deploy to save the function.

### Step 4 — IAM
Go to the IAM console.
- On the Roles page, click on Create role.
- Trusted entity type - AWS Service.
- Use cases for other AWS Services - select MediaConvert and check the radio button.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.8.JPG)

Click Next.
- AWS will have already attached two policies, AmazonS3FullAccess and AmazonAPIGatewayInvokeFullAccess

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.9.JPG)

- Click Next.
- Set the Role name to MediaConvert_Default_Role
- Click Create role. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.10.JPG)

- Next, we need to add a policy to our Lambda role. 
- Go back to the Lambda function you just created, go to the Configuration tab, then Permissions, then click on the Role name.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.11.JPG)

- Under Permissions Policies for that role, click Add permissions → Attach policies.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.12.JPG)

- Search for AWSElementalMediaConvertFullAccess, select it, and click Attach policies

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.13.JPG)

### Step 5 — CloudFront
Go to the CloudFront console.
- Click Create distribution.
- Set the Origin domain to the destination bucket we created earlier.
- Set the Name to cattube-origin.
- Set the Origin access to Origin access control settings (recommended).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.14.JPG)

- Click Create control setting.
- In the popup, leave all the options as default, and click Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.15.JPG)

- Scroll down to Viewer protocol policy and change this to Redirect HTTP to HTTPS.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.16.JPG)

- Leave all other options default and click Create distribution.
- On the next page, because we chose “Origin access control settings” on the previous configuration page, you will see a Copy policy button, click on that to copy the policy. We need to add this to the destination bucket policy.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.17.JPG)

- Go into the destination bucket we created earlier, then the Permissions tab, then down to Bucket policy. Click on Edit.
- Paste the Cloudfront policy into the Policy box.
- Save changes.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.18.JPG)

### Step 6 — S3 Event notifications
Go back to the S3 console.
- Go into the source bucket we created earlier, then to the Properties tab, down to Event notifications, and click Create event notification
- Set the Event name to cattube-notifications.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.19.JPG)

- Under Event types, select All object create events and leave the rest unchecked.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.20.JPG)

- Under Destination, select Lambda function.
- Select your cattube-converter-function function from the dropdown list.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.21.JPG)

- Save changes.

### Step 7 — Testing and Cleanup
- Upload some cat videos in to the S3 source bucket and make sure the filename has no spaces in it as the python script doesn't handle spaces.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.22.JPG)

- Make sure the the status of your job in the MediaConvert Jobs is COMPLETE.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.23.JPG)

- Go to the S3 destination bucket and note the name of the teo files present in it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/079/day79.24.JPG)

- Cloudfront distribution domain name and paste it in a browser tab followed by / and name of the output files. Do it for both the files.
- You will notice that one is a SD quality video while the other is a HD quality which shows that it has been converted to different formats.

Empty and delete both the source and destination buckets.
Delete the MediaConvert queue.
Delete the Lambda function.
Disable and delete the CloudFront distribution.
Delete both the IAM roles created.

## ☁️ Cloud Outcome

Created a Video on Demand ("CatTube") backend that will take any uploaded video format to a source bucket, convert it to the format we require, and place the output video into another bucket ready to be served by Cloudfront.

## Social Proof

[Blog](https://dev.to/aaditunni/build-an-event-driven-media-conversion-pipeline-1913)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7043670241750482944-tr3e?utm_source=share&utm_medium=member_desktop)
