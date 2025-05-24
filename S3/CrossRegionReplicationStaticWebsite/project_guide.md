# Cross-Region Replication of an S3 Static Website

In this Demo lesson we will create 2 S3 buckets, one in N. Virginia and the other in N. California and configure Cross-Region Replication (CRR between the two. This will enable obejcts to be replicated on both buckets. <br/>

1 - Make sure we are logged in you general AWS Account with the N. Virginia region selected. <br/>

2 - Go ahead and and move to the S3 console. We will create our source and destination buckets. For the source bucket, cllick on 'Create bucket', give it a name, pick the 'us-east-1' aws region (N. Virginia). Leave the rest on default, scroll down and click on 'Create bucket'. <br/>

3 - The scenario is that we are using this bucket to host a S3 Static Website so we need to enable it. Click on the bucket properties tab, scroll down to the bottom and click on 'Edit' on the 'Static Website Hosting' panel. Pick the 'Enable' option, next in the dropdown pick 'Host a static website' and enter 'index.html' on both index.html and error.html boxes (the index.html file will be displayed eventhough an error occurs) and next save changes. <br/>

4 - Finally, to enable this S3 bucket to host a public static website we need to go to the permissions tab and click on it, click on 'Edit' in the 'Block public access (bucket settings)' panel and uncheck 'Block all public access', next save changes. Notice that this step could be done at the bucket creation stage. This step has been performed now to show that we can change the settings of the bucket afterwards. <br/>

4 - 
