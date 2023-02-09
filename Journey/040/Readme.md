# Create a search domain in Amazon CloudSearch, upload a sample data and test the application

## Introduction

Create a search domain with indexes of a movie list and then upload the movie list so that we can search the data from the list.

## Prerequisite

- AWS free tier account.
- Download [data.json file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/data.json) and unzip it.

## Use Case

Search solution for your website or application.

## Services Covered

CloudSearch

## Try yourself

### Step 1 — Create a search domain.
- Go to the CloudSearch console.
- Create a new search domain.
- Give it a name.
- Choose one search.small as instance type. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.JPG)

- On the next tab choose Analyze sample file(s) from my local machine and use the data.json file.
- This file includes 5000 movie entries to play with.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.1.JPG)

- Leave the suggested index field configuration as default and continue.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.2.JPG)

- Set my policy to Allow open access to all services.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.3.JPG)

- Review and Confirm.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.4.JPG)

- It will take around 10 minutes to be created and become ACTIVE.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.5.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.6.JPG)

### Step 2 — 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.7.JPG)

- Once the Status is ACTIVE, upload a the JSON data.
- Click on the Upload button and navigate to the data.json file which you have downloaded previously.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.7.5.JPG)

- Upload documents.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.8.JPG)

- Test the Search Application. In the textbox enter Prisoners and click on the Go button.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.9.JPG)

- This will return all the details of the movie Prisoners as well as details of other movies that has the Prisoners in it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.10.JPG)

- Now test the search endpoint by copying the URL and adding some parameters to it for example (Replace <your-search-endpoint> with the Search Endpoint of your search domain): 
    ```
    <your-search-endpoint>/2013-01-01/search?q=Hanks&return=_all_fields
    ```
- This will return a JSON with all the entries of Tom Hanks.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/040/day40.11.JPG)

## ☁️ Cloud Outcome

Created a search domain with indexes of a movie list and then uploaded the movie list so that we can search the data from the list.

## Social Proof

[Blog](https://dev.to/aaditunni/create-a-search-domain-in-amazon-cloudsearch-upload-a-sample-data-and-test-the-application-2gno)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7029401885589581824-cOR_?utm_source=share&utm_medium=member_desktop)