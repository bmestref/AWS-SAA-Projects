# EC2 SSH vs EC2 Instance Connect

Amazon EC2 Instance Connect provides a simple and secure way to connect to your Linux instances using Secure Shell (SSH). With EC2 Instance Connect, you use AWS Identity and Access Management (IAM) policies and principals to control SSH access to your instances, removing the need to share and manage SSH keys. Both methods are addressed to access instances via public IPv4 addressing and IPv6. In this project the main benefits and drawbacks of both methods will be exposed. <br/>

1 - Make sure you're logged in the us-east-1 AWS region. <br/>

2 - Move across the EC2 console. For the SSH connection method a key pair is required, so on the menu on the left scroll down and click on 'Key Pairs', next click on 'Create key pair'. Enter a name for the key pair, select 'RSA' under 'Key pair type', next select the '.pem for the key pair and click on 'Create key pair'. Once you've done that, save the key on your local machine. <br/>

3 - In order to proceed with this lesson, a CloudFormation template (YAML format) is attached to the project so the deployment of the instance is inmediate (relatively) and takes no manual effort to implement it. Also, additional resources are deployed within the template such that the VPC created in the previous repo (populated with a series of subnets, NAT gateway + Internet gateway providing public Internet access to some of the subnets). Wait until the stack is moved to the completed state. <br/>

4 - Apart from the VPC, the template will deploy a public EC2 instance called 'PublicEC2' which its own public IPv4 address. We will connect to it latter, but first move to the EC2 console, click on 'Instances' on the menu on the left, click on the 'PublicEC2' instance and next move to the 'Security' tab. We will then see three Ibound rules: the first is alocated in Port 22 using protocol TCP with source ::/0, which in other words will allow any incoming IPv6 addressing connection request via SSH Client. Below the other 2 are listed, providing both public access from any IPv4 address (one for each connection method, either Instance Connect or SSH Client). We can right click on the instance, next click on 'Connect' and choose for now the 'SSH Client' connection method. Here instructions on how to proceed with the connection will be listed. The prompt which needs to be run on our terminal to connect to the instance is shown below: <br/>

```
ssh -i "<your key pair>" <your ec2 user>@<public DNS of the instance>
```
An example would look like this: <br/>

```
ssh -i "A4L.pem" ec2-user@ec2-34-203-233-236.compute-1.amazon.com
```

So copy into your clipboard this prompt, fulfill it with your instance parameters, move to the directory the downloaded key pair is stored, paste into the therminal de prompt above and press enter. That should connect us to the EC2 instance. Below you can see how should be done in case your key pair is stored within the 'Downloads' folder: <br/>

![instance connect using ssh client](instance_connectssh.PNG)

See that the terminal returns an error as the terminal does not hold the permissions to access the key pair we created earlier. Grant the appropriate permissions by running the following command to restrict access to the private key file: <br/>

```
chmod 400 <your key pair>
```
Rerun the prompt displayed above earlier and access to the instance should be granted. It cannot be emphasized enough that the key pair must be locally downloaded and the SSH connection run on the same directory the key is stored within. This strategy presents scalability issues as every user is required to store the key locally (for a large group this would pose a scalability constraint). Also, another drawback arises from the network connectivity form the local perspective: it needs to be ensured that the security group the instance belongs to grants access to the local machine (as a route pointing to 0.0.0.0/0 range is added to the 'Inbound rules' table we will not get across this issue). <br/>

5 - As an alternative, move to the 'Instance connect' method back on the EC2 console. A pair of 'Connection Type's are displayed. For now, we will pick the shown on default. Generrally EC2 matches correctly the user name of the user attempting to connect to the instance so leave the 'User name' box with the default name and click on 'Connect'. That process will open the instance on a new web browser tab. It will take a few seconds to connect but once completed a terminal will be opened to manage the instance. Note that no key pair was required since this method grants access to the instance using internal AWS permissions (we have them since we are logged in as an AWS user). For external users, identity policies must be attached to them. <br/>

6 - There is one important thing which demands our attention: note that changing the IPv4 SSH source address to your local IP will not affect the performance of the SSH client connection but will do for the Instance connect service. This fact arises as connections via Instance connect are not initiated from the local machine but instead from AWS with a different IPv4 address. Actually, when the Instance Connect service is invoked a call between the local machine and AWS takes place, and then from AWS to the instance. To summarize, AWS acts as an intermediary with its own IPv4 address between the local machine and the EC2 instance. In order to address the stated connectivity issue there is one thing we can do: instead of providing access to any IPv4 address setting the source of the route to 0.0.0.0/0 (bad practice) we can alternatively grant access to the unique IPv4 address of the service within a specific region. Attached to this repo you can find a JSON file which lists the IPv4 addresses of any service within AWS for any region. In our case, we should look for the Instance Connect service IPv4 address within the us-east-1 region. Should look like this: <br/>

```
{
  "ip_prefix":"18.205.107.24/29",
  "region":"us-east-1",
  "service":"EC2_INSTANCE_CONNECT",
  "network_border_group":"us-east-1"
}
```
Try to change the 0.0.0.0/0 address to 18.205.107.24/29 in the Instance Connect route and you whould be able now to get access to the instance via Instance Connect. By doing this SSH Client connectivity will be cut since our local machine IPv4 address is no longer supported. Add both 18.205.107.24/29 and our local machine IPv4 address to support SSH Client and Instance Connect services following good practices. <br/>

7 - Lastly we will cover a unique feature from EC2: Termination Protection. It adds an attribute to EC2 instances meaning they cannot be terminated while the flag is enabled. It provides protection against unintended termination and also allows role separation, where junior admins can be allowed to terminate but ONLY for instances with no protection attribute set. This feature can be enabled by right clicking on the EC2 instance, moving our cursor on top of 'Instance settings' and then on the pop up menu clicking on 'Change termination protection'. Once there, click on 'Enable' and click on 'Save'. Now, no matter how many times we attempt to terminate the instance, it will be running as normally. This recommended practice (mainly applied to critical instances) present a number of advantages: first, as stated earlier, prevents critical instances to be shut down, but it also adds another layer of permissions filter in order to terminate the instance (further permissions are required to disable the Termination Protection feature). Also, in the same pop up menu we can click on 'Change shutdown behaviour' and select either stop or terminate the instance when a shut down is performed.<br/>








