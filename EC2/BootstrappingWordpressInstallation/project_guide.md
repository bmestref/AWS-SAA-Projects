# Bootstrapping Wordpress Installation

In this project we will bootstrap two EC2 instances with WordPress. The first, directly using User Data via the console UI
and the second, using CloudFormation. User data will be used, employing the hability to run a script during the provisioning process of an EC2 instance and automatically add another layer of configuration to that instance during the build process (alternative method to creating an AMI, providing the capability fo changing the instance configuration during launch time). <br/>

1 - First, make sure you are logged in as the admin user in your general AWS account within the us-east-1 AWS region. <br/>

2 - Next, to speed up the project attached to the repo you will find a CloudFormation template which deploys the VPC and its corresponding subnets (created during previous projects). So, move to the CloudFormation service in the AWS console and deploy the stack, wait until it has moved to the complete state. <br/>

3 - 
