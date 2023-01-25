# Check the Integrity of Data in Amazon S3 with Additional Checksums

## Introduction

Create an S3 bucket and upload an object to it with additional checksum so that it can verify the integrity of the data.

## Prerequisite

AWS account.

## Use Case

Verify that your files were not altered during data transfer or during the upload or download.

## Services Covered

S3

## Try yourself

### Step 1 — Create an Amazon S3 bucket
- Go to S3 console.
- Click Create Bucket.
- Give an unique name and select the region you want your bucket to be in.
- Leave the remaining options as default configuration.
- Create bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/025/day25.JPG)

### Step 2 —  Upload a file and turn on additional checksum
- Open the S3 bucket created.
- Click on the Objects Tab.
- Click Upload
- Choose Add files.
- Choose any file you want.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/025/day25.1.JPG)

- Navigate down the page to find the Properties section.
- Select Properties and expand the section.
- Under Additional checksums select the On option and choose SHA-256.
- If your object is less than 16 MB and you have already calculated the SHA-256 checksum (base64 encoded), you can provide it in the Precalculated value input box. To use this functionality for objects larger than 16 MB, you can use the CLI or SDK.
- You can calculate it using this command in your terminal:
    - open a terminal window and navigate to where your file is. Replace image.jpg with your filename and extension.
    ```
    shasum -a 256 image.jpg | cut -f1 -d\ | xxd -r -p | base64
    ```
- You can calculate it using this command in your powershell:
    ```
    param(
        [Parameter(Mandatory=$True,
                ValueFromPipeline=$True)]
        $filePath
    )

    $hasher = [System.Security.Cryptography.SHA256]::Create()
    $content = Get-Content -Encoding byte $filePath
    $hash = [System.Convert]::ToBase64String($hasher.ComputeHash($content))
    Write-Output ($filePath.ToString() + ": " + $hash)
    ```
    - You will get an input asking for filepath. 
-  If the checksum values that you entered in the Precalculated value do not match, Amazon S3 generates an error and rejects the upload, as shown in the screenshot.
- You can also leave the Precalculated value blank and upload the object and then check and verify it by running the above command and comparing it with the value S3 generates for you.
- Click Upload and wait for it to complete.


![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/025/day25.4.JPG)

### Step 3 — Verify
- Select the uploaded file by selecting the filename. This will take you to the Properties page.
- Navigate down the properties page and you will find the Additional checksums section. This section displays the base64 encoded checksum that Amazon S3 calculated and verified at the time of upload.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/025/day25.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/025/day25.6.JPG)

- Empty the bucket and delete the bucket to cleanup.

## ☁️ Cloud Outcome

Learned how to upload a file to Amazon S3, calculate additional checksums, and compare the checksum on Amazon S3 and my local file to verify data integrity.

## Social Proof

[Blog](https://dev.to/aaditunni/check-the-integrity-of-data-in-amazon-s3-with-additional-checksums-4p98)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7024062213845860352-id8U?utm_source=share&utm_medium=member_desktop)
