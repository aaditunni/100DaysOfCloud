# Finding vulnerabilities on EC2 instance using Amazon Inspector

## Introduction

Amazon Inspector allows us to find vulnerabilities on configured EC2 instances. There are 2 types of assessment runs are performed, Network assessment and Host assessment. 
Network assessment has Network Reachability package rule while Host assessment has three types of package rule i.e. Common vulnerabilities and exposures, Center for Internet Security (CIS) Benchmarks, Security best practices for Amazon Inspector. There are mainly three types of Severity levels for rules in Amazon Inspector i.e. High, Medium, and Low. Informational severity of findings is just best practices recommended by Amazon Inspector.

## Prerequisite

AWS account.

## Services Covered

- EC2
- Amazon Inspector

## Try yourself

### Step 1 — EC2
- Create a Security Group EC2 
    - Choose Type: SSH Source: Anywhere (IPv4).
    - Choose type: Custom TCP Rule Port Range: 20 Source: Anywhere (IPv4).
    - Choose type: Custom TCP Rule Port Range: 21 Source: Anywhere (IPv4).
    - Choose type: Custom TCP Rule Port Range: 23 Source: Anywhere (IPv4).
- Launch an free tier eligible EC2 instance and use the above security group.
- SSH into the EC2 instance and install the AWS Agent.
- Download the agent installation script by running the following commands:
    ```
        wget https://inspector-agent.amazonaws.com/linux/latest/install
    ```
- To install the agent, run the following command:
    ```
        sudo bash install
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.JPG)

### Step 2 — Inspector
- Go to the inspector console (classic version). Click on the Cancel button present on the right bottom corner.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.1.JPG)

- On the left side menu, click on the Assessment targets.
- Click on the Create button.
- Give a name.
- Select Include all EC2 instances in this AWS account and region.
- Install Agents is selected by default.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.2.JPG)

- Click OK on the popup.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.3.JPG)

- Click Assessment template and create.
    - Select the target created earlier for target name.
    - For Rules packages, select all from dropdown list.
    - Create.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.4.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.5.JPG)

- Select the template and click Run.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.6.JPG)

- To see the Assessment Run and its result, click on the Assessment runs present on the left side menu.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.7.JPG)

- Click on the number of findings to know about the vulnerabilities found by Inspector on the EC2 instance.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.8.JPG)

- Click on the Assessment runs, present on the left side menu and choose the Download report button to download the report as PDF.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/057/day57.9.JPG)

## ☁️ Cloud Outcome

Ran Amazon Inspector and found vulnerabilities on EC2 instance.

## Social Proof

[Blog](https://dev.to/aaditunni/finding-vulnerabilities-on-ec2-instance-using-amazon-inspector-5h95)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7035615319255564288-T-vf?utm_source=share&utm_medium=member_desktop)
