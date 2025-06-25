# Logging and Metrics with CloudWatch Agent

In this mini-project we will download and install the CloudWatch Agent and configure it to capture 3 log files from an EC2 instance. Also, we will configure an instance role allowing the agent to store the above config into parameter store and allow the agent to inject the logging and metric data into CW and CW Logs.<br/>

1 - As always, make sure you logged in to the general AWS account of your account as well as us-east-1 region selected. <br/>

2 - Next, deploy the CoudFormation template attached to the repo to create from the beggining all resources required to complete the project. <br/>

3 - Once the CloudFormation stack deployment has been completed, we will step through the attachment of a CloudWatch agent on a EC2 instance to monitor and track its performace. In that way, move across the EC2 console, locate the instance created by the CoudFormation template, right click on it and select 'Connect'. Next, connect to the instance using 'EC2 Instance Connect'. <br/>

4 - Once you're connected, install the CloudWatch agent typing into the terminal the prompt below. <br/>

```
sudo dnf install amazon-cloudwatch-agent
```

Before starting the CloudWatch agent, we need to create the config file but as we also want to store that config file into Parameter Store service, we also want to give to this instance permissions to interact with CloudWatch Logs. In that way, we need to attach to this instance an IAM role, so move back to the AWS console and move across the IAM AWS service, click on 'Roles' on the menu on the left, click on 'Create Role', select the 'EC2' option, click on 'Next'. Now, we will attach two managed policies to the role: the first is 'CloudWatchAgentServerPolicy' and the second is 'AmazonSSMFullAccess'. Once these two policies are selected, scroll down and click on 'Next', enter a name and finish up the creation of the role by clciking on 'Create role'. Now, move back to the EC2 console, right click on the instance, select 'Modify IAM role' and pick the role previously created. <br/>

5 - Now, reconnect to the instance restarting the connection. As mentioned before, we will step through the config file, so type into the terminal the command below: <br/>

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

That will start the config file of the instance. You may need to press enter on some config options to deploy the config file (set an aggregation interval, port of usage, etc.). For this project, on all of them select the default option so we will keep things simple.
At some point we will be asked to enter a log file path where the file will be stored. We will track the behaviour of three log files. As the CloudWatch agent is set, we will introduce the 3 log files paths attached below: <br/>

```
/var/log/secure
/var/log/hhtpd/access_log
/var/log/hhtpd/error_log
```
For this project, you can leave the rest on default. <br/>

6 - Once the configuration file has been created successfully, the terminal will yield the path the file was stored as: ```/opt/aws/amazon-cloudwatch-agent/bin/config.json```. Inmediately after that, we will be asked wether we rather store the config in the SSM parameter store or not. In this project, we will choose to store it so enter 'Yes'. Also, we will be asked for the credentials the instance requires to send the config to parameters store. If the role attached earlier was successful, its credentials will be used by default. Once you've done that, the config is ready, so check that out moving back to the AWS console and move to the AWS System Manager service, click on 'Parameter Store'. There, a JSON file containinf the configuration of the CloudWatch Agent should be visible, so now we can use this file to configure this agent into this instance as well as any instances we aim to deploy. This is a key benefit of storing configuration in Parameters Store: CloudWatch Agents configuraiton can be set at scale, no need to manually set it instance by instance. <br/>

7 - Go back to the instance conneection. Now, the CloudWatch Agent expects a piece of software (a non Linux-Amazon software not installed by default) which needs to be manually installed, so first we will create a directory where the Agent expects this software to exist: <br/>

```
sudo mdkir -p /user/share/collected/
```

Next, create a database file, we can do that by running the command below: <br/>

```
sudo touch /usr/share/collected/types.db
```

8 - Finally, the last step is to startup the CloudWatch Agent with the configuration stored within Parameter Store, and by doing that the Agent will be able to download the configuration and inject all the logging data for the webserver into CloudWatch Logs. That can be completed by running into the terminal the command below: <br/>

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-Linux -s
```
Essentially, what the command above performs is to start the Agent and pull the config from the Parameter Store to the instance. After that, logs will be injected to CloudWatch Logs. <br/>

9 - Check the Agent runs as expected by moving back to the CloudWatch console, clicking on 'Log groups' below 'Logs' in the menu on the left. All 3 log groups created earlier should be visible there, where we can check either error, warnings and access logs. <br/>

10 -  This is the end of this mini-project. As always, clear up all the infrastructure created throughout the project so we are not mistakenly billed on unused resources.l <br/>
