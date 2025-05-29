# EC2 SSH vs EC2 Instance Connect

Amazon EC2 Instance Connect provides a simple and secure way to connect to your Linux instances using Secure Shell (SSH). With EC2 Instance Connect, you use AWS Identity and Access Management (IAM) policies and principals to control SSH access to your instances, removing the need to share and manage SSH keys. Both methods are addressed to access instances via public IPv4 addressing and IPv6. In this project the main benefits and drawbacks of both methods will be exposed. <br/>

1 - Make sure you're logged in the us-east-1 AWS region. <br/>

2 - Move across the EC2 console. For the SSH connection method a key pair is required, so on the menu on the left scroll down and click on 'Key Pairs', next click on 'Create key pair'. Enter a name for the key pair, select 'RSA' under 'Key pair type', next select the '.pem for the key pair and click on 'Create key pair'. Once you've done that, save the key on your local machine. <br/>

3 - In order to proceed with this lesson, a CloudFormation template is attached to the project so the deployment of the instance is inmediate (relatively) and takes no manual effort to implement it. Also, additional resources are deployed within the template such that the VPC created in the previous repo (populated with a series of subnets, NAT gateway + Internet gateway providing public Internet access to some of the subnets). <br/>

4 - 
