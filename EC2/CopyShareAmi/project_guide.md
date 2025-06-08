# Copying & Sharing an AMI 

This project steps through the main points of copying an AMI between regions, details some encryption considerations and steps through the permissions model when looking at public, private and shared AMIs. <br/>

1 -First, make sure you're logged to the main AWS account of your organization within the us-east-1 region (N.Virginia). <br/>

2 - Remember that AMIs are region constrained (they are not a global service). That can be checked changing the current AWS region and going to EC2 main Console > Images > AMIs: the AMI we created in the previous projects no longer is visible. To forward the AMI to external regions, right click on the AMI (once we are back on the original AWS region the AMI was created) and select 'Copy AMI'. Next, select a 'Destination Region' from the dropdown menu (also, you can change the name of the AMI and add a description below). Go ahead and click on 'Copy AMI'. <br/>

3 - Now, move to the destination region and check the AMI is on the 'pending' state going to EC2 main Console > Images > AMIs. This is not an inmediate process: the amount of time may vary depending on the resources and size of the instance. Note that AMIs are regional, therefore this copied AMI is a brand new one: if it is modified, the original AMI located within the us-east-1 region will not be affected (also, both have completely different IDs). <br/>

4 - One last thing to cover in this mini-project, we can manage permissions on AMIs by right clicking on the AMI and selecting 'Edit AMI permissions'. By default, an AMI is private which means is only accessible from the AWS account it was originally created. If the 'Public' access option is selected, be aware that any AWS account will have access to it. A smarter approach would be to keep it private but granting permissions to a number of AWS accounts of our interest. That can be done by selecting the 'Private' access option and in the 'Shared accounts' box below adding the on 'Add account ID' of those AWS accounts of our interest. <br/>
