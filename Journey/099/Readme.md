# Create a Lambda function and upload code using Ansible

## Introduction

Create a Lambda function and upload code of Arithmetic Operations using Ansible.

## Prerequisite

- AWS free tier account.
- Ansible installed along with python, boto/boto3 and botocore.
- Configure AWS credentials using aws configure.
- Create a Role with AWSLambdaBasicExecutionRole policy.
- Download the [day99.zip file](https://downgit.github.io/#/home?url=https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/099/day99.zip) into the same directory as your ansible playbook yaml file

## Services Covered

- Lambda
- Ansible

## Try yourself

### Step 1 — 
- Create an ansible playbook (yaml file) in the terminal.
    ```
        vim day99.yml
    ```
- Paste the following in the yml file.
- Replace the lambda_role_arn with the ARN of your Lambda role.
    ```
        - name: Create Lambda Function
        hosts: localhost
        gather_facts: false
        vars:
            lambda_function_name: day99
            lambda_handler_name: lambda_handler
            lambda_runtime: python3.9
            lambda_role_arn: "arn:aws:iam::123456789712:role/day99-role" # Replace with your existing IAM role ARN
        tasks:
            - name: Create Lambda Function
            amazon.aws.lambda:
                name: "{{ lambda_function_name }}"
                runtime: "{{ lambda_runtime }}"
                handler: "{{ lambda_handler_name }}"
                role: "{{ lambda_role_arn }}"
                zip_file: ./day99.zip
                description: "My Lambda Function"
                state: present

    ```
- Execute the ansible playbook:
    ```
        ansible-playbook day99.yml
    ```
- The resources will be created. A Lambda function with the code deployed will be created.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/099/day99.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/099/day99.1.JPG)

- To delete the resources, create a new ansible playbook (day99-delete.yml) with he code below and run it. Make sure the values you changed in the create file is the same here also:
    ```
        - name: Delete Lambda Function
        hosts: localhost
        gather_facts: false
        vars:
            lambda_function_name: day99
        tasks:
            - name: Delete Lambda Function
            amazon.aws.lambda:
                name: "{{ lambda_function_name }}"
                state: absent

    ```
- Run :
    ```
        ansible-playbook day99-delete.yml
    ```

- This will cleanup all the resources created earlier.

## ☁️ Cloud Outcome

Created a Lambda function and uploaded code of Arithmetic Operations using Ansible.

## Social Proof

[Blog](https://dev.to/aaditunni/create-a-lambda-function-and-upload-code-using-ansible-5g4i)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7051236840174985216-zJuK?utm_source=share&utm_medium=member_desktop)
