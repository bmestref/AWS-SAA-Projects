# Using EC2 Instance Roles 
In this project we will create an EC2 Instance Role, apply it to an EC2 instance and learn how to interact with the credentials this generates within the EC2 instance metadata. <br/>

1 - As always, make sure we are within the us-east-1 AWS region in your general management AWS account of your organization. <br/>

2 - Next, deploy the stack attached into the repo via CloudFormation, responsible of deploying the VPC we have been working in throughout the repo, some instances as well as an S3 bucket. <br/>

3 - Once the stack has moved to the completed state, move to the EC2 console, right click on the instance previously created and connect via EC2 Instance Connect as performed on previous projects. <br/>

4 - See that no roles have been attached to the instance therefore if we try to interact with AWS via the command line utilities such like ```aws s3 ls``` the CLI tools will tell us we do not hold any credentials to perform this task. In order to provide the right permissions, Access Keys need to be attached (they can be provided via ```aws configure``` in the CLI but it is not the best practice for an EC2 instance. What we are going to do instead is use Instance Roles). Move back to the AWS console and move to the IAM console. Next, click on 'Roles' and then click on 'Create Role'. Different types of roles are provided based on any escenario case: roles for AWS services, for AWS accounts, etc. In this case, we will select the 'AWS service' role type and below we will select the 'EC2' service as the service which permissions will grant access to. Click on 'Next', and in the searchbook look for the 'AmazonS3ReadOnlyAccess' policy, which will grant access to perform only read tasks (associated with this role). Next, click on 'Next', enter a name for the role and click on 'Create role'. <br/>

5 - In order to attach this role to our instance, go back to the EC2 console, right click on the instance recipient of the role, click on 'Security' and select 'Modify IAM role'. We can create a new role directly from the screen but as we created one earlier we will just select the latter (select it on the dropdown). Click on 'Save' to save changes. <br/>

6 - Check the role was attached sucessfully by connecting again via EC2 Instance Connect to the instance, and once the CLI shows up type ```aws s3 ls```. Now, as we have the instance role associated with this EC2 instance the command line tools use the temporary credentials created earlier. These credentials are provided actually via the Meta-data service. This can be checked by typing in the terminal ```curl http://169.254.169.254/latest/meta-data/iam/security_credentials/```. If at the end of the previous prompt add the name of the role, we will be able to see the explicit credentials of the role, as well as the expiration datetime when they will be no longer valid. These temporary credentials will be automatically renewed as long as services keep using the EC2 Instance. <br/>

7 - One last thing to stress out, Configuration Settings do follow a specific order. It can be checked in the image below: <br/>
(image.PNG)[Image of Configuration Settings precedence]
