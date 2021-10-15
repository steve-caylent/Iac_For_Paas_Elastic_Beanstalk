# IaC For PaaS - Elastic Beanstalk

## What is Elastic Beanstalk

AWS Elastic Beanstalk is an user-friendly service for deploying both web applications and services. Elastic Beanstalk can use several different programming languages (Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker). These can be used on Apache, Nginx, Passenger, and IIS services.<br />
Once your application code is uploaded to Elastic Beanstalk, deployment (including load balancing, auto-scaling, etc). After Elastic Beanstalk creates the resources, you can access the application and underlying resources whenever you like. This is made one step easier by using CloudFormation to create the Elastic Beanstalk environment.
## Benefits of Elastic Beanstalk (EB)
Elastic Beanstalk is a quick and easy way to deploy and test applications in AWS. Developers can easily upload an application and EB will handle the deployment and provisioning. This allows for infrastructure to be provisioned quickly, freeing up time for developers.

**Elastic Beanstalk is a Platform as a Service. It auto deploys resources on your behalf**

![AWS Elastic Beanstalk High Level](https://user-images.githubusercontent.com/90650872/137396722-76a74bf0-224d-4901-b718-1d8dfd6f8bfd.png)


## Elastic Beanstalk VPC
The solution will create a VPC to test EB environments. Elastic Beanstalk Web Tier environments require an Internet Gateway as well as a public subnet in order to be created successfully. This VPC environment can be used by your organization for testing/development of web tier applications.

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

### Elastic Beanstalk VPC Environment Outputs
The CloudFormation template used above will produce several outputs that can be used in cross-stack references when creating EB environments. This will ensure EB applications use the VPC created specifically for EB Web Tier environments, and connect to the RDS endpoint. This ensures that the RDS instance will remain independent of all EB applications created with EB CloudFormation templates and will persist beyond their lifespan of the EB Environment.

<img src="https://user-images.githubusercontent.com/90650872/137389767-76728fdb-4f20-4dc5-8a91-30cd45ebcd57.png" width="600">

## Creating Elastic Beanstalk Web Tier Environment with CloudFormation Template
This solution will create a sample application (running 64bit Amazon Linux 2 v4.2.6 running Tomcat 8.5 Corretto 11) - The sample application could easily be replaced with any solution by referencing another S3 bucket and key.


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
  - will connect to an RDS instance in EB VPC using the RDS endpoint.


## Setting up the environment for Elastic Beanstalk

Use the CloudFormation template below to deploy EB.<br />
* EB-Demo-EB-CF.yml l *- for creating EB applications/environments*
<br />


![EB-DEMO-w_EB](https://user-images.githubusercontent.com/90650872/137389728-9a495a59-c48a-49c6-a75f-0ea60fd405b7.png)


As usual, wait out the setup...

## CloudFormation Parameters
You will be given the option to customize the EB Web Tier Environment. This will give your organization's developers the ability to customize a temporary application environment with ease.

<img src="https://user-images.githubusercontent.com/90650872/137389783-ed31ec56-5ee5-408d-9db2-832fafb85bc3.png" width="800">

### Connecting to a RDS Endpoint
This solution is using cross stack references to use the RDS endpoint in the EB VPC. Alternatively, you could use SSM Parameter Store parameters and reference a RDS endpoint (seen below in commented code).

<img src="https://user-images.githubusercontent.com/90650872/137389760-7e8198a6-3ab3-4c3a-a575-f3706c30d026.png" width="600">

### Elastic Beanstalk using CloudFormation
Elastic Beanstalk creates an environment on your behalf by utilizing CloudFormation under the hood! The EB CloudFormation template you uploaded will itself create a CloudFormation stack. Upon deleting the stack you created, the EB provisioned stack will also be deleted.

<img src="https://user-images.githubusercontent.com/90650872/137389790-cd5262d6-ab76-4105-9c9a-3edfb5c06f4f.png" width="350">

### Elastic Beanstalk Output
Once the CloudFormation template has finished an output will have been created in order for you to access the sample application! Nice now let's see if EB created the resources properly and connected to the external RDS!

<img src="https://user-images.githubusercontent.com/90650872/137389817-154451f6-d289-4f44-a400-bad095774efa.png" width="600">


## Using the EB URL Link:
Click on the output to test the application. Nicely done! Works like a charm! Now your organization's developers can spend more time developing and less time worrying about provisioning resources for testing!

![SuccessRDSconnection](https://user-images.githubusercontent.com/90650872/137389828-64891153-083d-490d-99ff-4168a830fdd4.png)


## Finished!
Using Elastic Beanstalk can be a real time saver for the developers in your organization. Simply delete the CloudFormation template you used to create the EB environment and retain the EB VPC with the RDS instance to continue testing/developing. Enjoy!

## References
[AWS Elastic Beanstalk Documentation - Getting Started](https://aws.amazon.com/elasticbeanstalk/getting-started/)

## Author
Caylent Inc.







