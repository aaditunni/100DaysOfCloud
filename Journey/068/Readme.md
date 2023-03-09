# Update CloudFormation template

## Introduction

Update the instance type of one EC2 instance, add an inbound security rule allowing HTTP access to the instances from anywhere exposing the instance id of one of the instances as a stack template output.

## Prerequisite

- AWS free tier account.
- Download and unzip [template.yaml file]().

## Services Covered

- CloudFormation

## Try yourself

### Step 1 —
- Go to the CloudFormation console.
- Create a Stack using the template.yaml file. 
- Give the stack name as day68
- This will create the resources.
- Once it is CREATED, click on Stack Actions and Edit termination protection an Enable it.
- Here, the EC2 instances are t2.micro type and security group attached to it has an inbound rule that allows SSH on port 22 from anywhere. We will update it so that one of the instance type is t3.micro and an inbound rule of HTTP on port from anywhere.
- Click on Update.
- In Update stack page, Select Edit template in designer and Click View in designer.
- In designer, replace the TestEc2Instance with this code:
    ```
        TestEc2Instance:
            Type: "AWS::EC2::Instance"
            Properties:
            ImageId: !Ref LatestAmiId
            InstanceType: t3.micro
            KeyName: !Ref "AWS::AccountId"
            NetworkInterfaces:
                - AssociatePublicIpAddress: "true"
                DeviceIndex: "0"
                GroupSet:
                    - Ref: Ec2InstanceSecurityGroup
                SubnetId: !Ref PublicSubnet
            Tags:
                - Key: Name
                Value: !Sub ${TagPrefix} Test Instance
    ```

- Add a security rule by pasting this code under the Security rules:
    ```
        Ec2InstanceSecurityGroup:
            Type: "AWS::EC2::SecurityGroup"
            Properties:
            GroupDescription: Security group for EC2 instances
            GroupName: ec2-instance-sg
            VpcId: !Ref VPC
            SecurityGroupIngress:
            - IpProtocol: tcp
                FromPort: 80
                ToPort: 80
                CidrIp: 0.0.0.0/0
    ```
- Add a Template Output:
    ```
        TestEc2Instance:
            Description: Instance Id
            Value: !Ref TestEc2Instance
    ```
- Click on Update Stack in the designer.
- Under Configure Stack option, select IAMRoleTwo in Permissions.
- Update.
- You will notice that the t2.micro in TestEc2Instance is updated to t3.micro and an inbound rule added to the existing security group.

Delete Stack.
Empty and delete the S3 bucket that was created automatically during update process. 

## ☁️ Cloud Outcome

Updated the instance type of one EC2 instance, added an inbound security rule allowing HTTP access to the instances from anywhere exposing the instance id of one of the instances as a stack template output.

## Social Proof

[Blog](https://dev.to/aaditunni/update-cloudformation-template-2fdm)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7039708899406454784-1EO2?utm_source=share&utm_medium=member_desktop)
