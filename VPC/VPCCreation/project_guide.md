# Creation and Configuration of a Custom VPC End to End

In this lesson, we will step through the creation and configuration of a custom multi-tier VPC. The VPC will be populated with a number of subnets which will have access to the public internet via a VPC router and, next, an Internet Gateway located in the AWS Public Zone. This custom architecture is important because it reflects a best-practice design for secure, scalable, and high-performing applications in the cloud.<br/>

1 - As always, make sure you are logged in as the admin user in the N. Virginia region (us-east-1). <br/>

2 - Next, go to the VPC console, select 'Your VPCs', and click on 'Create VPC'. From here, we can create the VPC only or create it along with other resources. For now, pick the 'VPC only' option under 'Resources to create' (during the next lessons we will be implementing the remaining components within the VPC). Next, give a name to the VPC and choose the 'IPv4 CIDR manual input' option so we can manually define the IP range for the subnets within our VPC. In the 'IPv4 CIDR' box, provide a reasonable range such as '10.16.0.0/16' (remember that IPv6 supports both private and public IP addresses, while by default IPv4 only supports private). <br/>

3 - Select 'Amazon-provided IPv6 CIDR block' under 'IPv6 CIDR block' to support public IP addresses as well. Under 'Tenancy', select 'Default' (instances run on shared hardware with other AWS customers — choose 'Dedicated' if they must run on hardware physically isolated at the host level from instances that belong to other AWS accounts). Once you've done that, leave everything else as default and click on 'Create VPC' at the bottom of the page. <br/>

4 - Every VPC comes with a unique identifier called a VPC ID, which can be found under 'Details' on the VPC panel of the one we have just created. Now, let's configure the DNS settings (DNS settings in custom VPCs are crucial because they control how resources inside the VPC resolve domain names). Click on 'Actions' and then on 'Edit VPC settings'. Next, make sure under 'DNS settings' that both DNS resolution and DNS hostnames are checked. Once you've done that, click on 'Save'. <br/>

5 - Next, we will step through the creation of 4 subnets in each AZ: the Web subnet, the App subnet, the Database subnet, and the Reserved subnet. We will add all 4 to 3 different AZs (AZ A, B, and C). Attached to this repo you will find a document specifying the configuration of each subnet. It should look like this: <br/>

![pic2](vpc_state1.png)

The name of each subnet is stated at the beginning of each line, followed by its IPv4 CIDR range, then the AZ it belongs to, and at the end of the line the unique identifier of the IPv6 CIDR range (this last value will make sense at the subnet configuration step). Once you've opened that, go to the VPC console, click on 'Subnets' in the menu on the left, and then click on 'Create subnet' at the top right of the page. AWS allows you to create multiple subnets at once, so we will just need to repeat the same process 3 times (once for each AZ available in the VPC). Let's get started with AZ A. Under 'VPC ID', select the VPC ID of the VPC we created earlier from the dropdown menu. Once you've done that, we will start with 'Subnet 1 of 1'. Go back to the config.txt document attached to the repo (we will start with the reserved subnet), copy the subnet name and paste it into the 'Subnet name' box. Next, in the 'Availability Zone' box, pick the us-east-1a AZ (this will be our AZ A). Then, set 'IPv4 CIDR block' to 'Manual' and paste the IP range copied from the subnet configuration file. In the panel below, pick the IPv6 VPC CIDR block of your VPC and in 'IPv6 subnet CIDR block', paste the same CIDR range as above. With the arrows below, you can adjust the range of the subnet, so click two times on the down arrow (this means your subnet will be /64 sized while your VPC will be /56 — so the subnet fits inside the VPC). Also, to provide a unique IPv6 range for each subnet, change the last two digits of the number before the /<number> at the end of the address and paste the two digits found at the end of the subnet line in the configuration file (this will make each subnet's IPv6 range unique). Once you've done that, scroll to the bottom and click on 'Add new subnet'. Repeat the same steps for the remaining subnets in AZ A. Once all 4 subnets have been created in AZ A, scroll down and click on 'Create subnet'.<br/>

6 - Follow the same steps for the other 2 AZs, setting 'Availability Zone' to us-east-1b for AZ B and us-east-1c for AZ C. <br/>

7 - All 3 AZs have now been populated with 4 subnets each. There is one final step we need to complete for all of these subnets: each of them has an allocated IPv6 range. However, they are not set to auto-assign IPv6 addresses to resources created within them. To fix that, select each subnet, click on 'Actions', and then on 'Edit subnet settings'. Once there, check the box labeled 'Enable auto-assign IPv6 addresses'. Scroll down and click 'Save'. Repeat this step for all subnets within the VPC. In production environments, all these manual steps should be automated. So far, we end up with the following architecture sketched below. <br/>

![pic1](config.PNG)

8 - At this point, all subnets are private and do not support communication with the AWS Public Zone. To allow public access for the WEB subnets, we need to implement an Internet Gateway, Route Tables, and appropriate Routes within the VPC. Once the WEB subnets are public, we can create a bastion host with a public IPv4 address and connect to it for testing. To begin, go to the VPC console. Create an Internet Gateway (a highly available object that provides public routing to and from the internet). In the left menu, click on 'Internet gateways' and then click on 'Create internet gateway' in the top right corner. Assign a name to the Internet Gateway and click on 'Create internet gateway'. By default, it is not attached to any VPC, so we need to do that manually. Click on 'Actions', then 'Attach to VPC', and in the 'Available VPCs' box, select your VPC ID. Click on 'Attach internet gateway' to make it available within your VPC (this enables the VPC to communicate with the public internet). <br/>

9 - Next, we need to create a Route Table within the VPC to ensure traffic from the public subnets to the internet and vice versa is routed successfully. Two routes will be defined: one that allows traffic between the private and public subnets (IPv4), and another that allows communication between the public subnets and the public internet (IPv6). The latter route will point directly to the Internet Gateway we just created. Go to 'Route tables' in the left menu of the VPC console and click 'Create route table'. Choose your VPC ID and assign a name to the route table. Click 'Create route table'. You should now see the newly created object. Expand the overview section at the bottom of the page and go to 'Subnet associations'. Currently, it is not associated with any subnets. Click on 'Edit subnet associations' and select the three web subnets (intended for direct public access): sn-web-A, sn-web-B, and sn-web-C. Click 'Save associations'. Next, go to the 'Routes' tab. You will see two default routes: one for IPv6 and one for IPv4. These cannot be modified or deleted because they are catch-all routes. However, we can add additional routes. Click 'Edit routes' and add the following:<br/>

Destination: 0.0.0.0/0 (default IPv4 route)<br/>
Target: Internet Gateway (select the one you created)<br/>

Click 'Add route' again and add:<br/>

Destination: ::/0 (default IPv6 route)<br/>
Target: Internet Gateway (select the same one)<br/>

This ensures that any unknown traffic associated with this route table will be routed to the Internet Gateway.<br/>

10 - Before launching any resources, we need to ensure that everything created in subnets sn-web-A, sn-web-B, and sn-web-C can be assigned a public IPv4 address. To do that, go to 'Subnets' in the left menu, locate and select sn-web-A, sn-web-B, and sn-web-C, then click on 'Actions' > 'Edit subnet settings'. This time, under 'Auto-assign IP settings', check the box labeled 'Enable auto-assign public IPv4 address'. Click 'Save'. This ensures that every resource created within these subnets will be allocated a public IPv4 address.<br/>

11 - Now it's time to test this configuration by deploying and EC2 instance within the sn-web-A subnet, so click on 'Services' and move across the EC2 console. Once we are on the EC2 console click on 'Launch Instance'. In the name box give a name for the instance, scroll down and pick 'Amazon Linux' under 'Application and OS Images'. On the dropdown below go ahead and select the latest version of Amazon Linux. Assuming everything is all good, specify the 'Instance type' (make sure you're in the free tier so no costs are charged to your bill). Scroll down and under 'Key pair' click on 'Create key pair' if if does not exist yet. Enter a name on the name box, select 'RSA' under 'Key pair type', select '.pem' on the 'Private key file format' panel and then click on 'Create key pair'. This will download the key pair to your local machine. Now that it has been created, select it on the Key pair dropdown, scroll down and on 'Network settings' click on 'Edit'. Next, pick the VPC as well as the subnet (sn-web-A) the instance will be deployed within and make sure both 'Auto-assign public IP' and 'Auto-assign IPv6 IP' are enabled. Also, we will create a new security group for the instance, so in the 'Firewall' panel select 'Create security group', next enter a name for the security group, leave the rest as default and click on 'Launch instance'. <br/>

12 - Next cick on instances at the top left of the screen and wait until the instance has 2/2 status check (ready to go). We will connect to this instance using a local SSH client so right click on the instance and next on 'Connect'. See that there is a number of different methods to access to the instance, but most of the cases we will connect using the 'EC2 isntance connect' method since it is faster (the instance is opened on a web console) and does not require an SSH client connection. However, this time pick the 'SSH client' connection and a list of instructions will be displayed on how to proceed with the connection. The prompt run on our terminal to connect to the instance is shown below: <br/>

```
ssh -i "<your key pair>" <your ec2 user>@<public DNS of the instance>
```
An example would look like this: <br/>

```
ssh -i "A4L.pem" ec2-user@ec2-34-203-233-236.compute-1.amazon.com
```

So copy into your clipboard this prompt, fulfill it with your instance parameters, move to the directory the downloaded key pair is stored, paste into the therminal de prompt above and press enter. That should connect us to the EC2 instance. Below you can see how should be done in case your key pair is stored within the 'Downloads' folder: <br/>

![instance connect using ssh client](instance_connectssh.PNG)

See that the terminal returns an error as the terminal does not hold the permissions to access the key pair. Grant the appropriate permissions by running the following command to restrict access to the private key file: <br/>

```
chmod 400 <your key pair>
```
Rerun the prompt displayed above earlier and access to the instance should be granted. Regarding alternative methods to access the instance, it is worthwhile to highlight the 'Session Manager' method, which is particularly useful in scenarios where the instance does not have a public IPv4 address. <br/>

13 - We don’t need to manually go through all these steps because CloudFormation allows us to deploy the entire architecture using a YAML template file in most of the cases. AWS used to provide the CloudFormer service which provided the capability of translating your AWS infrastructure to a YAML file ready to be deployed, it has been no longer maintained. Currently Former2 surges as the replacement of this tool. Attached to the repo you will find a YAML file containing the AWS VPC built so far in this repo so it can be easily deployed using CloudFormation. It may take some time until the process has moved to the completed state. <br/>

14 - During the steps below we will implement a highly-available regionally resilient NAT Gateway solution within the VPC. 3 Nat Gateways will be created along Route tables and Default Routes with the Nat Gateway as a target. Finally, we will associate those Route Tables with the Reserved, App and DB subnets in AZ A, B and C before testing the solution using the internal instance. The main reason to deploy NAT gateways on each AZ is to grant to acess the public Internet from the perspective of the private EC instance. Also, doing it for every AZ leads to a AZ-resilient architecture (tolerant to AZ disasters). We will start from the a stack creation using CloudFormation and the YAML file attached to the repo. Once the stack and all resources within it has moved to the completed state, move to the EC2 console, right click on the 'A4L-INTERNAL-TEST' (this is a private test instance, could be created manually but we skip this part and deploy it straightforwardly via CloudFormation stack deployment), click on 'Connect' and pick the 'Session Manager' method to stablish a connection (commonly used for instances with no public internet access, VPC endpoints are used to connect). Test that it actually does not have public Internet connectivity by attempting to ping a public IPv4 address. Try this on the Session Manager. <br/>

```
ping 1.1.1.1
```
Once you've done that, let's fix that out by moving back to the main AWS console, moving across VPC and opening the console on a new tab. Next, on the menu on the left click on 'NAT Gateways' and 'Create a NAT Gateway'. Once there you need to specify a few things: first give a name for the NAT gateway and next pick a public subnet to go into (pick the sn-web-A subnet). Next, in order to privision an Elastic IP to the NAT gateway, scroll down a little bit and click on 'Allocate Elastic IP' which gives it automatically, leave the rest as default and click on 'Create NAT gateway' at the bottom of the screen. This process may take some time, so repeat this process for the other public subnets (sn-web-B and sn-web-C).<br/>

15 - Once all NAT gatways have moved away from the pending state, we will step through the route table implementation so private instances communicate with NAT gateways. Let's do that by click on 'Route tables' on the menu on the left, open it on a new tab, and click on 'Create a route table'. First, we need to choose a VPC for the route table so click on the 'VPC' dropdown and select it, name the route table (make sure the name is consistent), enter that and click on 'Create route table'. Create 2 additional route tables (in total 3, one for each NAT gateway (one for each AZ)). <br/>

16 - What we need to do now is to create a default route within each route table which will point to the NAT gateway in the same AZ, so select the first route table created above and then click on the 'Routes' tab, once there click on 'Edit routes' and add a new route. The destination will be set to '0.0.0.0/0' and in the 'Target' box we will select the NAT gateway of AZ A. Once you've done that, click on 'Save changes' and repeat the same process for AZs B and C. <br/>

17 - On more thing: we need to associate each subnet to its respective route table within each AZ. For that matter, pick the AZ A route table, next click on the 'Subnet associations' tab annd next on 'Edit subnet associations'. Once there, select all those subnets within the same AZ and click on 'Save changes'. Repeat the same process for the rest of available AZs (B and C). Now our private instance should be able to ping a public IPv4 address since we have the routing infrastructure in place (now the NAT gateway is providing service to each of the private subnets in all of the 3 AZs). <br/>

18 - One final step: tidy up the resources created throughout this project: remove route tables, NAT gateways, subnets, terminate instances, delete the subnets and finally the VPC. <br/>


