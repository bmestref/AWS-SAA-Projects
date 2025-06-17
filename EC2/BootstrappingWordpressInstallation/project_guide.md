# Bootstrapping Wordpress Installation

In this project we will bootstrap two EC2 instances with WordPress. The first, directly using User Data via the console UI
and the second, using CloudFormation. User data will be used, employing the hability to run a script during the provisioning process of an EC2 instance and automatically add another layer of configuration to that instance during the build process (alternative method to creating an AMI, providing the capability fo changing the instance configuration during launch time). <br/>

1 - First, make sure you are logged in as the admin user in your general AWS account within the us-east-1 AWS region. <br/>

2 - Next, to speed up the project attached to the repo you will find a CloudFormation template which deploys the VPC and its corresponding subnets (created during previous projects). So, move to the CloudFormation service in the AWS console and deploy the stack, wait until it has moved to the complete state. <br/>

3 - Move to the EC2 console, next click on 'Launch instance'.  Enter a name for the instance, scroll down and make sure the 'Amazon Linux 2023 AMI' is selected under the 'Application and IOS Images' panel. Pick any elegible free tier instance type according to the AWS region we are located within. Under 'Key pair' click on 'Proceed without a key pair (Not recommended)', then scroll down to 'Network settings', click on 'Edit'. In the panel, select manually the VPCwe have just launched via CloudFormation in the 'VPC' box, next in the 'Subnet' box pick  the 'sn-web-A'subnet. Then, make sure 'Auto-assign public IP' is enabled, also use an existing security group by selecting 'Select existing security group' under 'Firewall' panel and in the dropdown select the 'BOOTSTRAP-InstanceSecurityGroup-...' which was created in the stack deployment. Leave the rest on default except 'Advanced details'. Expand this and scroll down to the 'User data' box. Attached to the repo you will find a TXT file containing all command line prompts which where executed at the manual deployment of Wordpress on a previous project. This will impressively speed up things, so copy into your clipboard all the content of the TXT file and paste it on the User data box. By default, the user data needs to be provided in base64 encoding format. If not (our case), AWS automatically encodes it so ensure that if the user data provided is not encoded the 'User data has already been base64 encoded' is not checked (check it in the other way aorund). The user data will be run as the instances launches (this is what will bootstrap the instance). Finally, click on 'Launch instance'. <br/>

4 - Wait until the instance has moved to the completed state. After that, right click on the Instance and click on 'Connect', pick the 'EC2 Instance Connect' option and click on 'Connect'. We should be able to interact with the instance. Also, if we copy into the clipboard the public IPv4 address of the instance and paste it into a new browser tab we will come across the Wordpress webpage since the user data has performed the full installation of MariaDB and additional required resources behind the scenes. <br/>

5 - It is relevant to stress out User data does is not provided by the Metadata service. This fact can be checked by moving back to the Instance Connect terminal and typing the prompt below: <br/>

```
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
```
The prompt above will create a TOKEN which will be used to authenticate ourselves to the Metadata service. Run the prompt and next run this: <br/>

```
curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/
```
This will get the Metadata of the instance. The 169.254.169.254 is the Metadata IP address. To get the user-data of the instance, replace ```meta-data``` by ```user-data``` in the http address above. This will allow us to see the user-data of the instance.  <br/>

6 - We can also check the logs folder of the instance by typing in the terminal: <br/>

```
cd /var/log
ls -la
```
There are two log files which are relevant when tracking aby boostrapping problem: these files are cloud-init-output.log and cloud-init.log. The role the first file plays is crucial: it contains every command and its output run in the instance (database creation, permissions management, Wordpress installation files, etc.) (use it to determine the source of any error within the instance bootstrapping). The content of any of these files can be retrieved by typing the command below:<br/>

```
sudo cat <file_name>.log
```

7 - Alternatively, we can provide the EC2 Instance user-data in advance in the CloudFormation template. Attached to this repo, you can find a second CloudFormation template which already includes user-data in the instance launch. Open it on any code editor and scroll down until you come across the User-data block: <br/>
```
<...>
UserData:
  Fn::Base64: !Sub |
  #!/bin/bash -xe
  # STEP 2 - Install system software - including Web and DB
  dnf install wget php-mysqlnd httpd php-fpm php-mysqli mariadb105-server php-json php php-devel cowsay -y
  # STEP 3 - Web and DB Servers Online - and set to startup
  systemctl enable httpd
  systemctl enable mariadb
<...>
```
These are exactly the same User-data prompts as used in the earlier steps, so it is expected to deploy the same resources as we did manually. <br/>

8 - Finish up the project by terminating manually any instance deployed via CloudFormation as well as any additional resource. <br/>

