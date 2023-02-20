# Launch and connect Windows-EC2 using RDC

## Introduction

Create and connect to an EC2 Instance running the latest Windows free tier AMI and then connect to it from your machine using RDC. Install Internet Information Services (IIS), create a custom webpage and host it on that EC2 Instance.

## Prerequisite

AWS free tier account.

## Services Covered

EC2

## Try yourself

### Step 1 — 
- Go to the EC2 console.
- Launch an EC2 Instance. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.JPG)

   - Windows Server 2022 Base AMI of type t2.micro.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.1.JPG)

   - Under Security Group section create a new SG with RDP (Remote Desktop Connection) from anywhere.
   - Create a RSA(.pem) key pair and download it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.2.JPG)

- When the instance's state changes to running, go to the instance details and click on Connect.
- Choose RDP client.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.3.JPG)

- Click on Get password.
- Click on Upload private key and upload the key pair that was downloaded earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.4.JPG)

- Click on Decrypt password.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.5.JPG)

- Click on Download remote desktop file and open it.
- A popup will appear. Click on Connect.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.6.JPG)

- Enter the password that was decrypted earlier.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.7.JPG)

- Click on yes if it shows that it cannot be authenticated with its security certificate to proceed anyways. 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.8.JPG)

- You will be logged in to you Windows EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.9.JPG)

- Go to command prompt (cmd) and run the below command to install  an IIS server on the EC2 instance so that it can launch a web page.
    ```
    DISM /online /enable-feature /featureName:IIS-DefaultDocument /All
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.10.JPG)

- In the start menu open the IIS. Then continue to Default Web Site and click on *Browse:80 (http) this will open the default web page in localhost mode.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.11.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.12.JPG)

- Create a Custom Web Page. Open notepad and create a sample html website. Save it as index.html.
    ```
    <html>

    <body>

    <h1>Welcome to Day 51!</h1>

    <h2>You have successfully created a custom web page.</h2>

    </body>

    </html>
    ``` 

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.13.JPG)

- Save it on disc C in folder inetpub > wwwroot.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.14.JPG)

- Now when you refresh the locally hosted website you will see the content of index.html in the browser.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/051/day51.15.JPG)

## ☁️ Cloud Outcome

Created and connected to an EC2 Instance running the latest Windows free tier AMI and then connected to it from your machine using RDC. Installed Internet Information Services (IIS), created a custom webpage and hosted it on that EC2 Instance.

## Social Proof

[Blog](https://dev.to/aaditunni/launch-and-connect-windows-ec2-using-rdc-612)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7033519442550501377-RK9j?utm_source=share&utm_medium=member_desktop)
