# IaC For PaaS - Elastic Beanstalk



**Elastic Beanstalk is a Platform as a Service. It auto deploys resources on your behalf**

![AWS Elastic Beanstalk High Level](https://user-images.githubusercontent.com/90650872/137396722-76a74bf0-224d-4901-b718-1d8dfd6f8bfd.png)

### What will be created with Elastic Beanstalk VPC environment

The initial environment will have the following:
* A VPC with IGW
* 3 public subnets
* Security Group for EC2 Web Tier EC2's 
* public route
* 3 private subnets
* RDS MySQL Instance
* RDS Subnet group across all private subnets
* Security Group for RDS 
* private route

## Setting up the environment

Use the CloudFormation template below to set up the initial VPC environment for Elastic Beanstalk deployments <br />
* EB-Demo-VPC-RDS.yml  *- for the creating VPC and RDS instance*
<br />

![EB-DEMO-VPC View](https://user-images.githubusercontent.com/90650872/137389717-be667c19-e1fe-424f-bd17-a2d26e7c2f7b.png)

As usual, wait out the setup...


<img src="https://user-images.githubusercontent.com/90650872/137389767-76728fdb-4f20-4dc5-8a91-30cd45ebcd57.png" width="600">


### What will be created with the Elastic Beanstalk template

The EB template will create the following:
* web roles for Web Tier EC2
* Elastic Beanstalk service role 
* Elastic Beanstalk Web Tier Environment with options:
  - EB Environment will have EC2 in auto-scaling group
  - single instance/load balanced
  - EC2 key pair for SSH
  - stream to CloudWatch logs
  - time to retain logs
  - will connect to RDS instance in EB VPC using RDS endpoint.


## Setting up the environment for Elastic Beanstalk

Use the CloudFormation template below to deploy EB.<br />
* EB-Demo-EB-CF.yml l *- for creating EB applications/environments*
<br />


![EB-DEMO-w_EB](https://user-images.githubusercontent.com/90650872/137389728-9a495a59-c48a-49c6-a75f-0ea60fd405b7.png)


As usual, wait out the setup...



<img src="https://user-images.githubusercontent.com/90650872/137389783-ed31ec56-5ee5-408d-9db2-832fafb85bc3.png" width="800">

<img src="https://user-images.githubusercontent.com/90650872/137389760-7e8198a6-3ab3-4c3a-a575-f3706c30d026.png" width="600">


<img src="https://user-images.githubusercontent.com/90650872/137389790-cd5262d6-ab76-4105-9c9a-3edfb5c06f4f.png" width="350">


<img src="https://user-images.githubusercontent.com/90650872/137389817-154451f6-d289-4f44-a400-bad095774efa.png" width="600">

![SuccessRDSconnection](https://user-images.githubusercontent.com/90650872/137389828-64891153-083d-490d-99ff-4168a830fdd4.png)


## Finished!


## References
[AWS Elastic Beanstalk Documentation - Getting Started](https://aws.amazon.com/elasticbeanstalk/getting-started/)

## Author
Caylent Inc.





