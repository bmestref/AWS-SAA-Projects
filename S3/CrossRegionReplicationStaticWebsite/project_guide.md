# Cross-Region Replication of an S3 Static Website

In this Demo lesson we will create 2 S3 buckets, one in N. Virginia and the other in N. California and configure Cross-Region Replication (CRR between the two. This will enable obejcts to be replicated on both buckets. <br/>

1 - Make sure we are logged in you general AWS Account with the N. Virginia region selected. <br/>

2 - Go ahead and and move to the S3 console. We will create our source and destination buckets. For the source bucket, cllick on 'Create bucket', give it a name, pick the 'us-east-1' aws region (N. Virginia). Leave the rest on default, scroll down and click on 'Create bucket'. <br/>

3 - The scenario is that we are using this bucket to host a S3 Static Website so we need to enable it. Click on the bucket properties tab, scroll down to the bottom and click on 'Edit' on the 'Static Website Hosting' panel. Pick the 'Enable' option, next in the dropdown pick 'Host a static website' and enter 'index.html' on both 'Index document' and 'Error document' boxes (the index.html file will be displayed eventhough an error occurs) and next save changes. <br/>

4 - Next, to enable this S3 bucket to host a public static website we need to go to the permissions tab and click on it, click on 'Edit' in the 'Block public access (bucket settings)' panel and uncheck 'Block all public access', next save changes. Notice that this step could be done at the bucket creation stage. This step has been performed now to show that we can change the settings of the bucket afterwards. Confirm these changes and click on 'Confirm'.<br/>

4 - Finally, to make the bucket public an S3 bucket policy must be attached. In this way, go to the 'Permission' tab within your buclet, scroll down and in the 'Bucket policy' panel click on 'Edit'. Attached to this repo you will the bucket policy we need to paste there. We have stepped through this part several times in the previous lessons so you should be already familiar with this (remeber to change the default arn path to your bucket arn). With this policy any anonymouts or unauthenticated user will be able to get any object within this bucket. Click on 'Save changes' at the end. <br/>

5 - The source bucket is fully public accessible. Next we need to create the destination bucket, so proceed to move to the S3 console and click on 'Create bucket'. Give it a unique name and uncheck 'Block all public access' so we do not need to change the bucket settings later as done in the source bucket. Acknowledge your providing public access to your bucket by checking the box below and click on 'Create' at the bottom of the page. <br/>

6 - Next, let's enable the static website hosting feature, so move to 'Properties', scroll down to the bottom and click on 'Edit' on the 'Static website hosting' panel. Click on 'Enable', choose the option to 'Host a static website', and just like we did before enter 'index.html' on both 'Index document' and 'Error document' boxes, and click on 'Save changes'. <br/>

5 - We need to provide public access to the destination bucket, so follow step 4 but this time on the destination bucket. <br/>

6 - So far we have created two buckets hosting static websites  with public access. Now we will get to the core of the lesson and enable the cross region replication feature between the source and destination buckets. To commit this task, click on the source bucket, click on the 'Management' tab, scroll down and we need to configure a 'Replication rule' so click on 'Create replication rule'. <br/>

7 -  See that in order to enable replication across regions versioning must be turned on as well on the source bucket. AWS gives us a button next to the warning on top of the page so no need to get back to the bucket configuration. Click on it to enable this feature. Now this bucket can be the source of this replication rule. Next, give it a name for the replication rule, click on 'Enable' in the 'Status' panel. Scroll down and you will come across the 'Source bucket' panel, where we will be able to filter based on some scopes (this feature may be useful on cases where the user needs to replicate only a part of the bucket (some objects within it)). In our case we want to replicate the entire bucket so in the 'Choose a rule scope' select 'Apply to all objects in the bucket'. Now let's configure the destination bucket: scroll a little bit down and in the 'Destination' panel we can pick either use as destination a bucket within the same AWS account of different. In case picking the latter, be aware of the ownership of objects. For this simple demo we will pick the first option, click on 'Browse S3' and select the destination path of the second bucket created earlier. Again, in the warning below enable versioning in the destination bucket by clickin in the button next to. Once you've done that, we need to provide the right permissions to the replication rule to interact with AWS resources to commit its task (read from the source and write into the destination), so click on the 'Choose IAM role' dropdown and select 'Create new role' (you could pick an existing one if you already configured one, but in this lesson we don't). <br/>

8 - In the 'Encryption' panel below you can enable replication on encrypted objects. Leave this option unchecked for this demo. <br/>

9 - Also, in the 'Destination storage class' you can change the storage class where objects are replicated and forwarded to. This may be useful on disaster recovery scenarios where a cheaper storage class is used in the destination bucket. By default both S3 buckets will have the same storage class, so leave this option unchecked in this lesson. <br/>

10 - Below you will find some extra new features recently added (on 'Additional replication options'). For instance, by enabling 'Replication Time Control (RTC)' feature you ensure 99.99% of new objects are replicated to the destination within 15 minutes (this come to an additional cost). An additional feature is 'Delete marker replication', enabling replication as well on delete markers. A description of each feature is displayed below each option box so we will not provide any further insights on these additional features for now. Click on 'Save' to end the replication step. <br/>

11 - We will come across a pop up offering the possibility to spread replication on objects which existed before the replication feature was enabled. Since we have not populated our bucket with objects yet we will check the 'No, do not replicate existing objects' option and click on 'Submit'. <br/>

12 - Now replication is finally enabled between these two buckets, so let's give it a try and upload some objects on the source destination (some can be found attached to this repo). Once you've done that, go back to your destination an wait until the replicated objects appear in your destination (do not alarm if they are not shown inmediately, it may take some minutes based on your internet connection and object size). If done correctly, the static website should look like this (copy into your clipboard the url path displayed at the bottom of the page after click on 'Properties' on the bucket panel).<br/>

![The best plane in the world](plane.PNG)
<br/>

13 - At this point that is it all we wanted to cover in this lesson, so tidy up all resources created along the lesson so empty and delete both the source and destination buckets. Remember that to enable replication across buckets we had to create an IAM role so move to the IAM console, click on 'Roles', locate the role we created, select it (make sure you select the one that has the name of source bucket) and delete that role. <br/>

$$\color{red}WARNING!$$
It cannot be stressed enough that client-managed KMS keys need to be removed since they do not fall into the AWS free tier. Since removing a KMS key is a destructive and potentially dangerous procedure (after a KMS key is deleted, you can no longer decrypt the data that was encrypted under that KMS key, which means that data becomes unrecoverable), AWS requires to set waiting period of 7-30 days (by default it is 30 days). During this waiting period we will not be charged any cost of usage of the key, but if the deletion order is withdrawn then we will be charged the cost of usage of all this period until the deletion was canceled. <br/>

<br/>
