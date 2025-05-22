# S3 Versioning

In this lesson S3 buckets versioning will be covered. 
S3 versioning is important because it provides data protection, recovery, and audit capabilities for objects stored in Amazon S3 buckets.<br/>

1 - Log in your general management AWS account and make sure we are logged in N. Virginia region (us-east-1). <br/>
2 - Move across AWS to the S3 console.<br/>
3 - Click on 'Create Bucket', provide a unique name for the bucket, scroll down and uncheck 'Block all public access' and check 'I acknowledge that the current ...' since this S3 bucket will be designed to hold an S3 Static Website. Scroll down a little bit more and under 'Bucket Versioning' click on 'Enable', keep scrolling down and on the bottom click on 'Create Bucket'.<br/>
4 - Next, once the bucket is on the create mode, click on 'Properties' and scroll down to the very bottom. Then, click on 'Edit' in the 'Static website hosting' box. Check the box to enable Static Website Hosting, for 'Hosting type' set the 'Host a static website' option. Next, type 'index.html' in the 'Index document'. Do the same in the 'Error docuemnt' box by tping 'error.html'. These are mandatory (almost) files which must be provided beforehand to run the static website (for more details go and check out [S3/StaticWebsite](https://github.com/bmestref/AWS-SAA-Projects/edit/main/S3/StaticWebsite). Scroll down to the bottom and click on 'Save changes'. <br/>
5 - As you may already know from the previous lesson, enabling Static Website Hosting is not enough to deliver a truly static website accessible from the public internet: we need to attach a Bucket policy. For that matter, move across the Properties tab and click on 'Edit' under 'Bucket Policy'. Copy on the clipboard the content of the bucket policy attached to the repo and paste it there (for more details on the structure of the bucket policy [check it out](https://github.com/bmestref/AWS-SAA-Projects/edit/main/S3/StaticWebsite/projectguide.md). Make sure the 'Resource' statement points directly to your bucket arn, which can be found easily just above the 'Policy' box. Once you've done that, scroll down and click on 'Save changes'. <br/>
6 - Next, we will populate our bucket with some files (attached in the repo). Upload them, from the index.html and error.html files to the images folder. <br/>
7 - 
