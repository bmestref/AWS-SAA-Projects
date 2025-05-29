# EBS Volumnes 

In this DEMO lesson, we will interact with EBS, Instance Store Volumes, and EC2. We will begin by creating an EBS volume and mounting it to an EC2 instance. Once attached, we will create and mount a file system on this volume and generate a test file to verify data persistence. Next, the volume will be migrated to another EC2 instance within the same Availability Zone, ensuring that both the file system and the test file remain intact after the move. Following this, a EBS snapshot of the volume will be created, and from that snapshot, provision a new EBS volume in a different Availability Zone (AZ-B). Again, we will verify that the file system and test file are present and unaltered.
We will then copy the snapshot to another AWS region, enabling cross-region data portability. <br/>
After working with EBS, a EC2 instance will be launched such that it uses instance store volumes. We will create a file system and a test file on the instance store volume. Upon restarting the instance, we will confirm that the file system remains intact. However, after stopping and starting the instance — which causes a change in the underlying EC2 host — one will be able to observe that the file system and its contents are no longer present, demonstrating the ephemeral nature of instance store volumes. <br/>

1 - To get started with this project we will need to deploy beforehand some infrastructure. To do that make sure we are logged in the N.Virginia region. Attached to the repo you can find a CloudFormation template moving the starting point to where we left at the VPC repo (VPC created and populated with a number of subnets with public Internet access). Wait until the stack has been successfully created. This template will create a number of resources. However, our attention will go to the 3 instances called 'Instance1', 'Instance2' and 'Instance3'. <br/>

2 - Let's take a look on these instances moving to the EC2 console and clicking on 'Instances' on the menu on the left. 3 instances will be displayed: 'A4L-EBS-INSTANCE1-AZA', 'A4L-EBS-INSTANCE2-AZA' and 'A4L-EBS-INSTANCE1-AZB'. The first 2 are alocated within the same AZ (A) while the latter runs within AZ B. All 3 share the same instance type (falling into the free tier). <br/>

3 - Next, on the menu on the left scroll down and select 'Volumes'. The CloudFormation template should have created a single EBS volume for each instance, providing boot volumes (all 3 hold a size of 8 GiB and a number of IOPS of 3000). Let's get some hands on experience creating EBS volumes so click on 'Create volume'. One thing that needs to be configured is the 'Volume type'. For this lesson pick the 'General Purpose SSD (gp3)' on the dropdown as it is a volume type mostly used for general purposes (see that IOPS and GiB are linked together in this volume type). In case IOPS and GiB rates need to be managed independelty, select an 'io' volume type listed below the 'gp' options (addressed for scenarios where high performance is demanded on small volumes) (recently AWS launched the 'io2' volume type which outperforms on most cases 'io1'). The rest of available volume types are either destined to specific scenarios or legacy (Magnetic (standard) which is unrecommendable for production stages). In this lesson we wille 'gp3' volume type. Next, in the 'Size (GiB)' box select a size of 10 (it will be enough for our project) and an IOPS rate of 3000. Also, under 'Availability Zone' select the 'us-east-1a' region we are currentlly working witin. In this stage of the project we will neither create the EBS volume from a snapshot nor configure any kind of encryption on the volume so leave the rest on default. Finally, enter a name tag for the volume and click on 'Create volume'. <br/>

4 - It may take a certain amount of time to make the volume available. Once it's over, right click on it and select 'Attach volume'. See that EBS volumes are constrained to only be attached to those instances running within the same AZ, so in the 'Instance' dropdown only the instances sharing the same AZ will be displayed. At this stage, select the Instance1 running in AZ A. Below you will see the 'Device name' box is already populated (actually, Device name stands for the device id the instance will see for this volume so this is how the volume will be exposed to the instance). As the warning below suggests, depending on the kernel running the device name may vary. For now, leave it on default and click on 'Attach volume'. <br/>

5 - Let's test out the instance once the volume is attached to it, so move to the EC2 console, select the Instance1 instance in AZ A, right click on it and then click on 'Connect'. We will connect via the web browser so pick the 'Instance Connect' connect method. Next,
below we will run a series of prompts to interact with the volume. Sart by checking what block devices are connected to the instance:<br/>
```
lsblk
```
The output should look like this in the terminal: <br/>

![lsblk](lsblk.PNG)

You will find two blocks: the first (xvda) stands for the instance running system (which is 8 GiB in size) while the latter (xvdf) stands for the EBS volume we attached earlier (with 10 GiB). Check if any file system is alocated within the volume by typing on the terminal the prompt below: <br/>

```
sudo file -s /dev/xvdf
```
If 'data' is yielded, then the volume does not contain any file system so we need to create it within this raw block device. So to do that we will run the command below: <br/>
```
sudo mkfs -t xfs /dev/xvdf
```
The prompt above is read as 'using sudo permissions create a xfs type file sistem within the /dev/xvdf block device'. Press enter and confirm the creation of the file system as successful by running again the prompt 2 steps above (now the response should not be 'data'). <br/>

6 - Now, the way Linux works is by mounting a file system to a mounted endpoint which is a directory, so we are going to create a directory called 'ebstest'321º by using the command below: <br/>

```
sudo mkdir /ebstest
```

7 - Next, let's mount the file system to the directory recently created: <br/>

```
sudo mount /dev/xvdf /ebstest
```
Press enter and verify the file system is successfully mounted to the folder entering the prompt below: <br/>

```
df -k
```

8 - Next, we will move to the file system: <br/>

```
cd /ebstest
```
Once on that folder, we will create a test file called 'testfile.txt' and enter some text on it: <br/>

```
sudo nano textfile.txt
```
Type whatever you want and save it by pressing Ctrl + O + Enter and Ctrl + X to exit. You should be able to see the content of the file by typing on the terminal: <br/>

```
ls -la
```
or see its full content typing ``` cat textfile.txt ```. So far we have set the folder as the mounted point of the file system within the volume attached to the instance, now reboot the instance by typing ```sudo reboot```.<br/>
