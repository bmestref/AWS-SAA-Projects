# S3 Versioning

In this lesson S3 buckets versioning will be covered. 
S3 versioning is important because it provides data protection, recovery, and audit capabilities for objects stored in Amazon S3 buckets.<br/>

1 - Log in your general management AWS account and make sure we are logged in N. Virginia region (us-east-1). <br/>

2 - Move across AWS to the S3 console.<br/>

3 - Click on 'Create Bucket', provide a unique name for the bucket, scroll down and uncheck 'Block all public access' and check 'I acknowledge that the current ...' since this S3 bucket will be designed to hold an S3 Static Website. Scroll down a little bit more and under 'Bucket Versioning' click on 'Enable', keep scrolling down and on the bottom click on 'Create Bucket'.<br/>

4 - Next, once the bucket is on the create mode, click on 'Properties' and scroll down to the very bottom. Then, click on 'Edit' in the 'Static website hosting' box. Check the box to enable Static Website Hosting, for 'Hosting type' set the 'Host a static website' option. Next, type 'index.html' in the 'Index document'. Do the same in the 'Error docuemnt' box by tping 'error.html'. These are mandatory (almost) files which must be provided beforehand to run the static website (for more details go and check out [S3/StaticWebsite](https://github.com/bmestref/AWS-SAA-Projects/edit/main/S3/StaticWebsite). Scroll down to the bottom and click on 'Save changes'. <br/>

5 - As you may already know from the previous lesson, enabling Static Website Hosting is not enough to deliver a truly static website accessible from the public internet: we need to attach a Bucket policy. For that matter, move across the Properties tab and click on 'Edit' under 'Bucket Policy'. Copy on the clipboard the content of the bucket policy attached to the repo and paste it there (for more details on the structure of the bucket policy [check it out](https://github.com/bmestref/AWS-SAA-Projects/edit/main/S3/StaticWebsite/projectguide.md). Make sure the 'Resource' statement points directly to your bucket arn, which can be found easily just above the 'Policy' box. Once you've done that, scroll down and click on 'Save changes'. <br/>

6 - Next, we will populate our bucket with some files (attached to the repo). Upload them, from the index.html and error.html files to the images folder. Click finally on 'Upload'. <br/>

7 - Once you've done that, move across the 'Properties' tab, scroll down to the bottom and open the static website on a new tab by clicking on the url located at the end of the page. <br/>

8 - Now it is time to experiment working with versioning. In this way, move back to the S3 console and click on 'Objects'. Since versioning is enabled, everytime we upload an already existing file within the bucket a new version will be created without overwritting the original (a new one is created with a unique id). Toggle the 'Show versions' slider so all available versions are now visible. This is what expected to see when having both versions. <br/>

![The cutest puppy in the world](versioning/dog.jpg) <br/>

9 - Now a unique ID is visible per version, that is, a unique code for that particular version of the object. Next, to test the versioning feature, upload a file with the same name as an already existing file within the bucket. If we refresh the static website, we will come across the recently uploaded picture (due to the fact S3 shows the most recent version). However, since versioning is enabled nothing is truly deleted (if the second image is deleted, then the first version of the object will be shown).<br/>

10 - See that if we step through the deletion of the most recently added version, a delete marker will be attached to the version (this is how S3 handles deletions when versioning is enabled). If we select that marker and click on 'Delete' the full deletion is completed, a permanent deletion, no more recoverable. Now this is what you will see. <br/>

![Another cute puppy](dog.jpg) <br/>

11 - Remember that versioning cannot be turned off on a bucket but instead suspended (but it does nothing to the current versions), so be careful to enable this feature since can incurr significant costs. <br/>

12 - Finally, tidy up by emptying the bucket and emptying it. <br/>
