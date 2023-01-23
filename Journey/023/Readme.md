# Extract raw text, table, and forms from scanned documents using Amazon Textract.

## Introduction

Extract raw text, forms, and table cells from a sample document and then download the results in various formats.

## Prerequisite

AWS free tier account.

## Use Case

Many companies today extract data from scanned documents, such as PDFs, tables and forms, through manual data entry (that is slow, expensive and prone to errors), or through simple OCR software that requires manual configuration which needs to be updated each time the form changes to be usable. To overcome these manual processes, Textract uses machine learning to instantly read and process any type of document, accurately extracting text, forms, tables and, other data without the need for any manual effort or custom code.

## Services Covered

Amazon Textract

## Try yourself

### Step 1 — Sample document.
- Go to Amazon Textract console.
- Click on Try Amazon Textract.
- You will have a default paystub document loaded on the screen.
- In Results section, Under raw text tab, you can select Segment by word to get each words from the document or Segment by line to get each line.
- CLicking on a word or line will highlight that in the document.
- There are other tabs such as Forms (extracts forms), Tables (extracts tables), Queries and Signatures (extracts signatures).

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/023/day23.3.JPG)

- Click on Download results to get a zip file which if you unzip, you will get data in different formats like csv, json, etc  

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/023/day23.4.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/023/day23.5.JPG)

### Step 2 — Upload document
- We can upload our own document to est it out. 
- Download this handwritten document by clicking [here](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/023/day23-pexels-skylar-kang-6044974.jpg)
- Unzip the file.
- In the Amazon Textract Console, scroll down and click Choose document.
- Upload the file that you downloaded.
- Under Data Output, select Forms and Apply configurations.
- In Results, you can select Segment by word to get each words from the document or Segment by line to get each line.
- CLicking on a word or line will highlight that in the document.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/023/day23.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/023/day23.2.JPG)

- Since it is a handwritten data, there are chances of getting mistakes as some handwriting might not be clear enough to extract efficiently.

## ☁️ Cloud Outcome

Extracted raw text, forms, and table cells from a sample document and then downloaded the results in various formats.

## Social Proof

[Blog](https://dev.to/aaditunni/extract-raw-text-table-and-forms-from-scanned-documents-using-amazon-textract-49m2)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7023268517017755648-blyH?utm_source=share&utm_medium=member_desktop)