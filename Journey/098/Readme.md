# Create security group and Key pair using Ansible

## Introduction

Create a security group with SSH access and a Key pair and save the Key pair on your local system using Ansible.

## Prerequisite

- AWS free tier account.
- Ansible installed along with python, boto/boto3 and botocore.
- Configure AWS credentials using aws configure.

## Services Covered

- EC2
- Ansible

## Try yourself

### Step 1 — 
- Create an ansible playbook (yaml file) in the terminal.
    ```
        vim day98.yml
    ```
- Paste the following in the yml file.
- Change the region, vpc_id (your VPC id), group_name (name of the security group you want to create), key_name (name of the Key Pair you want to create). The Key Pair after creation will save in the same director as this yml file.
    ```
        - name: Create security group with SSH access and key pair
        hosts: localhost
        gather_facts: false
        vars:
            region: "us-east-1" 
            vpc_id: "vpc-0c9f789f2c7dbd07b"
            group_name: "day98"
            group_description: "Allow SSH access to EC2 instances"
            key_name: "day98-kp"
            private_key_file: "private_key.pem"
            private_key_path: "{{ playbook_dir }}/{{ private_key_file }}"
        tasks:
            - name: Create security group
            ec2_group:
                name: "{{ group_name }}"
                description: "{{ group_description }}"
                region: "{{ region }}"
                vpc_id: "{{ vpc_id }}"
                rules:
                - proto: tcp
                    from_port: 22
                    to_port: 22
                    cidr_ip: 0.0.0.0/0

            - name: Create key pair
            ec2_key:
                name: "{{ key_name }}"
                region: "{{ region }}"
            register: key_pair

            - name: Save private key
            copy:
                content: "{{ key_pair.key.private_key }}"
                dest: "{{ private_key_path }}"
                mode: "0600"
    ```
- Execute the ansible playbook:
    ```
        ansible-playbook day98.yml
    ```
- The resources will be created. A Security group that allows SSH access and a Key Pair will be created and then the Key Pair will be saved in your local machine.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/098/day98.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/098/day98.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/098/day98.2.JPG)

- To delete the resources, create a new ansible playbook (day98-delete.yml) with he code below and run it. Make sure the values you changed in the create file is the same here also:
    ```
        - name: Delete security group and key pair
        hosts: localhost
        gather_facts: false
        vars:
            region: "us-east-1"
            vpc_id: "vpc-0c9f789f2c7dbd07b"
            group_name: "day98"
            key_name: "day98-kp"
            private_key_file: "private_key.pem"
            private_key_path: "{{ playbook_dir }}/{{ private_key_file }}"
        tasks:
            - name: Delete security group
            ec2_group:
                name: "{{ group_name }}"
                region: "{{ region }}"
                vpc_id: "{{ vpc_id }}"
                state: absent

            - name: Delete key pair
            ec2_key:
                name: "{{ key_name }}"
                region: "{{ region }}"
                state: absent

            - name: Remove private key
            file:
                path: "{{ private_key_path }}"
                state: absent
    ```
- Run :
    ```
        ansible-playbook day98-delete.yml
    ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/098/day98.3.JPG)

- This will cleanup all the resources created earlier.

## ☁️ Cloud Outcome

Created a security group with SSH access and a Key pair and saved the Key pair on your local system using Ansible.

## Social Proof

[Blog](https://dev.to/aaditunni/create-security-group-and-key-pair-using-ansible-3ma8)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7050786535943659520-q9UO?utm_source=share&utm_medium=member_desktop)
