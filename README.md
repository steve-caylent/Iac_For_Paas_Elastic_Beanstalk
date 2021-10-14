# IaC For PaaS - Elastic Beanstalk

AWS Systems Manager allows for Config Management of your EC2 Instances. AWS Systems Manager allows you to connect to EC2 instances to perform Run Commands (similar to Ansible) only with access via SSH. The benefit of using AWS SSM Session Manager is to give you access to EC2 instances in private subnets without having to use a Bastion Host. Session Manager allows you to access your EC2s within a private subnet directly from the AWS console or the AWS CLI. Using a Bastion Host, leaves your EC2 machines vulnerable to attack on port 22. Money is another factor to consider. Having an EC2 running as a Bastion Host can be costly. Reduce costs and tightening security are great reasons to consider using Session Manager.


**Elastic Beanstalk is a Platform as a Service. It auto deploys resources on your behalf**

![AWS Elastic Beanstalk High Level](https://user-images.githubusercontent.com/90650872/137396722-76a74bf0-224d-4901-b718-1d8dfd6f8bfd.png)



### Accessing EC2 in private subnets via a Bastion Host in a public subnet
The traditional way to access machines with private IPs is to first connect to a Bastion Host with a public IP and then forward into the machines with private IPs only. In this initial setup we will connect to the private subnet using a Bastion Host.

### What will be created with initial environment

The initial environment will have the following:
* A VPC with IGW
* 3 public subnets
* EC2 Bastion Host in public subnet
* Security Group for Bastion Host allowing port 22 inbound
* public route
* 3 NAT Gateways for High Availability
* 3 private subnets
* 3 EC2 Instances - 1 in each private subnet
* Security Group for EC2 instances allowing port 22 inbound from Bastion Host
* 3 Routes for private subnets/NAT Gateways


## Setting up the environment

Use the cloudformation template below to set up the initial environment with an EC2 Bastion Host in a public subnet<br />
* SSM-Bastion.yml *- for the initial setup environment*
<br />

![EB-DEMO-VPC View](https://user-images.githubusercontent.com/90650872/137389717-be667c19-e1fe-424f-bd17-a2d26e7c2f7b.png)

As usual, wait out the setup...


<img src="https://user-images.githubusercontent.com/90650872/137389767-76728fdb-4f20-4dc5-8a91-30cd45ebcd57.png" width="600">


### Accessing Bastion Host => Private EC2
From your remote computer make sure you have AWS CLI installed and configured as well as having the correct KeyPair .pem file.

<br />
Using the CloudFormation Outputs find the IPs for accessing the EC2 instances

<br />
First make sure SSH agent is running with your selected key pair
```
eval $(ssh-agent -s)
ssh-add ./PEMFILE.pem
```
Now access the bastion host with the pem file and IP address with agent forwarding
```
ssh -A -i PEMFILE.pem ec2-user@PUBLICIPOFBASTIONHOST
```
Great! Now you're connected to the Bastion Host EC2 instance in the public subnet.
<br />

### Connecting to EC2 in privates subnet via Bastion Host
Now that you've connected to the Bastion Host let's connect to a private EC2 instance

```
ssh ec2-user@PRIVATEIPofEC2
```



Fantastic! You've connected to the private EC2 instance. It took several steps, left an EC2 running and left port 22 open! Time to change that!

# Remote Access with AWS Systems Manager - Session Manager

### What will be created with Session Manager environment

The updated environment will have the following:
* A VPC with IGW
* 3 public subnets
* <strike>EC2 Bastion Host in public subnet</strike>
* <strike>Security Group for Bastion Host allowing port 22 inbound</strike>
* public route
* 3 NAT Gateways for High Availability
* 3 private subnets
* 3 EC2 Instances - 1 in each private subnet
* Security Group for EC2 instances <strike>allowing port 22 inbound from Bastion Host</strike> outbound port 443 only
* 3 Routes for private subnets/NAT Gateways
* 3 SSM Endpoints - 1 in each private subnet
* 3 SSM Messages Endpoints - 1 in each private subnet
* 3 SSM EC2 Messages Endpoints - 1 in each private subnet
* Security group for VPCE with inbound port 443 allowing the EC2 SG an outbound port 443 for the VPC.

### Requirements for using SSM Session Manager
* A role attached to the EC2 in a private subnet to access Session Manager.
* OS that supports SSM agent (using Linux in demo)
* SSM agent installed/started in instance (Amazon Linux 2 has by default)
  - To install SSM agent
  ``` install SSM agent
   curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
   sudo yum install -y session-manager-plugin.rpm
* 3 VPCE endpoints for linux EC2 (Windows EC2 requires 4)
* Security Group for VPC endpoints allowing EC2 to communicate with System Manager (port 443)

## Setting up the environment for Session Manager

Use the cloudformation template below to update the stack you created above.<br />
* SSM-Without-Bastion.yml *- for accessing EC2 via Session Manager*
<br />


![EB-DEMO-w_EB](https://user-images.githubusercontent.com/90650872/137389728-9a495a59-c48a-49c6-a75f-0ea60fd405b7.png)


As usual, wait out the setup...

## Accessing EC2 with Sessions Manager
Now that the VPCE’s have been created you can access the EC2 from the console or AWS CLI. To access from the CLI use the instance id to connect
```
aws ssm start-session --target INSTANCE ID
```




<img src="https://user-images.githubusercontent.com/90650872/137389783-ed31ec56-5ee5-408d-9db2-832fafb85bc3.png" width="800">

<img src="https://user-images.githubusercontent.com/90650872/137389760-7e8198a6-3ab3-4c3a-a575-f3706c30d026.png" width="600">


<img src="https://user-images.githubusercontent.com/90650872/137389790-cd5262d6-ab76-4105-9c9a-3edfb5c06f4f.png" width="350">


<img src="https://user-images.githubusercontent.com/90650872/137389817-154451f6-d289-4f44-a400-bad095774efa.png" width="600">

![SuccessRDSconnection](https://user-images.githubusercontent.com/90650872/137389828-64891153-083d-490d-99ff-4168a830fdd4.png)



And that's all it takes! Easy and secure!
## Finished!

And it's as easy as that! In a few steps you've created SSM Session Manager endpoints for access to EC2 in private subnets AND with no open ports!
The benefit of having the SSM VPCE's is that you can now use Systems Manager to Run Commands, install updates and scan using Inspector.<br />


## Author
Caylent Inc.





