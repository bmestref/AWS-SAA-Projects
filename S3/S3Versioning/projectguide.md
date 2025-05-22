# S3 Versioning

In this lesson S3 buckets versioning will be covered. 
S3 versioning is important because it provides data protection, recovery, and audit capabilities for objects stored in Amazon S3 buckets.<br/>

1 - Log in your general management AWS account and make sure we are logged in N. Virginia region (us-east-1). <br/>
2 - Move across AWS to the S3 console.<br/>
3 - Click on 'Create Bucket', provide a unique name for the bucket, scroll down and uncheck 'Block all public access' and check 'I acknowledge that the current ...' since this S3 bucket will be designed to hold an S3 Static Website. Scroll down a little bit more and under 'Bucket Versioning' click on 'Enable', keep scrolling down and on the bottom click on 'Create Bucket'.<br/>
4 - Next, once the bucket is on the create mode, click on 'Properties' and scroll down to the very bottom. Then, click on 'Edit' in the 'Static website hosting' box. Check the box to enable Static Website Hosting, for 'Hosting type' set the 'Host a static website' option. Next, type 'index.html' in the 'Index document'. Do the same in the 'Error docuemnt' box by tping 'error.html'. These are mandatory (almost) files which must be provided beforehand to run the static website (for more details go and check out <br/>
