# Predicting time series data with Forecast

## Introduction

Future power consumption will be predicted based on time series data uploaded to S3 in form of csv file.

## Prerequisite

- AWS account.
- Download and unzip [electricityusagedata.csv file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/electricityusagedata.csv)

## Services Covered

- S3
- Forecast

## Try yourself

### Step 1 — S3
Create a S3 bucket and upload the electricityusagedata.csv file to S3 bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.JPG)

### Step 1 — Forecast
Go to the Forecast console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.1.JPG)

- Create new dataset group.
- Give it a name. 
- Choose Custom forecasting domain.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.2.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.3.JPG)

- On the next tab specify the frequency of data to 1 hour and for data schema use the following JSON:
    ```
        {
            "Attributes": [
                {
                    "AttributeName": "timestamp",
                    "AttributeType": "timestamp"
                },
                {
                    "AttributeName": "target_value",
                    "AttributeType": "float"
                },
                {
                    "AttributeName": "item_id",
                    "AttributeType": "string"
                }
            ]
        }
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.4.JPG)

- For the data location input the S3 buckets URL.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.5.JPG)

- After the import completes start the predictor training.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.6.JPG)

- For Forecast horizon: Enter 36 and Forecast frequency: Select 1 hour from the drop-down menu. Training will take some time to complete.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.7.JPG)

- Back in the dashboard click Start to generate forecast. Use the predictor created in previous step. This will also take a while.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.8.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.9.JPG)

- When forecast finally get created you can go and query it. Specify start date as 2014/12/31 and end date: 2015/01/02 and key to client_1.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.10.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.11.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/086/day86.12.JPG)

The actual energy usage is shown in grey on the left and has no quantiles shown because there is no uncertainty. The forecast predictions are shown by three lines. The numbers P10, P50, and P90 correspond to the 10%, 50%, and 90% quantiles, respectively. The actual value has an 80% chance of being between the P90 and P10 range (90 - 10 = 80). The larger the number of PXX, the higher the probability that the value will fall within that range. The value of P50 line is the midpoint of the range.


### Step 3 — Cleanup
- Delete the dataset.
- Empty and delete S3 bucket.

## ☁️ Cloud Outcome

Predicted future power consumption based on time series data uploaded to S3 in form of csv file.

## Social Proof

[Blog](https://dev.to/aaditunni/predicting-time-series-data-with-forecast-4ch1)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7046260613542469632-kYye?utm_source=share&utm_medium=member_desktop)
