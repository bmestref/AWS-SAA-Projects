#   Deploying a Container using Fargate

In this project lesson we will step through the creation of a Fargate Cluster, next we will create a task and container definition and deploy the container Application from Dockerhub into Fargate. <br/>

1 - Make sure you are logged in as the admin user of your Management account of your organization while you're located within the us-east-1 AWS region. <br/>

2 - Next, move to the ECS console. We will haed to the creation of a Fargate cluster, responsible of running the container we just created in the previous project, so in the menu on the left click on 'Clusters' and then 'Create cluster'. Enter a name for the cluster, select a VPC (you can use the VPC created in the previous projects) in the 'Networking' panel below. It should automatically select all subnets within the VPC by default. If not, select them manually. Scroll down and in the 'Infrastructure' panel we are able to select between using an EC2 instance or any external instance. For this project, we will leave this feature on default and click on 'Create' at the bottom of the page. <br/>

3 - If this is the first time creating a Fargate cluster an error may pop up. Repeat the steps above again and it should work fine. This error arises from the fact there is an approval process that needs to happen behind the scenes, so if this is the first time using ECS using this AWS account it is normal to happen. <br/>

4 - Move to the ECS cluster creater in the steps above and click on 'Task definitions' in the menu on the left where we will create a task responsible of deploying the EC2 instance. Click on 'Create new task definition'. Next, enter a name for the task definition and below in the 'Container - 1' enter a name for the container about to use and specify in the box on the right the URI path of your container image. The URI should look like this: <br/>

```
docker.io/<your docker username>/<your container name>
```
Below the settings of the network (Port mapping, responsible of mapping the Docker container to the Fargate IP address) should be visible, specially regarding the Container Port, Protocol (TCP), Port name (should be similar to the container name) and Aplication protocol (HTTP by default). Leave the rest on default so scroll down to the bottom and click on 'Next'. <br/>

5 - Now we will specify some features regarding the Environment, so on the 'Operating system/Architecture' make sure 'Linux/X86_64' is selected. Next, on the 'Task size' box on the 'Memory' box select 1GB and in the 'CPU' box select '.5 vCPU', that should be enough resources for the Docker application. Scroll down and under 'Monitoring and looging' leave these optional features as default since we will not use them in this project so click on 'Next' at the bottom of the page. <br/>

6 - Now we can overview the task definition we have just configured so scroll down to the bottom and click on 'Create'. On the bottom of the page a JSON encapsulating the task definition is provided, useful in order to deploy a task definition from one AWS account to another. <br/>

7 - Now it's time to run the container within the task definition we have just created so to do that click on 'Clusters' and click on the cluster we created eearlier in this project. Click on 'Tasks' and next click on 'Run new task'. First, we need to pick among the 'Compute options' the 'Launch type' so check that box. Once selected, make sure the 'FARGATE' in the 'Launch type' dropdown is picked as well as 'LATEST' under 'Platform version'. Scroll down and under 'Deployment configuration' make sure 'Task' in the 'Application Type' is selected. Next, scroll down a little bit and under 'Family' select the container we created previously and in 'Revision' pick 'LATEST' (this will ensure the latest version of the container is used). Also, select 1 in the 'Desired tasks' box and leave the 'Task group' box in blank. Scroll down and expand 'Networking', make sure the default VPC is selected as well as all subnets inside the VPC. The cluster is intendede to work as a provider of an Elastic Web Interface when the task is run within, with its own security group so make sure in the box below an appropiate security group is selected such that it allows us to access the containerized application, so check the 'Create a new secyurity group' under 'Security group' 

