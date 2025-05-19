# How to Create a Static Website Hosted by S3

---

1 - First, make sure you are logged in as IAM User, in the US East (N. Virginia) region. <br/>
2 - Move across the AWS Console to S3, and click on create a new S3 bucket <br/>
3 - Fill the Bucket Name box with a unique name (the S3 bucket will not be created if there is another one which holds the same name. Make sure it is unique). One can create the bucket within a custom domain by adding at the end if the bucket name the domain identifier. <br/>
4 - Scroll down and uncheck 'Block all public access' (this will enable external people to access the static wesbite). <br/>
5 - To confirm we grant public access to the S3, check the 'I acknowledge that the current settings...' just below. <br/>
6 - For this demo lesson, you can leave the rest as default, so scroll down to the bottom and click on 'Create bucket'. <br/>
7 - So far, this S3 bucket can only be accessed via S3 API or Console UI, so we need to enable the Static Website Hosting feature in the S3 bucket, so click on the bucket and next click on 'Properties'. On the properties tab scroll down all the way  to the bottom and right at the very bottom we have 'Satic website hosting'. Now, you need to click on the 'Edit' tab and click on the 'Enable' button. <br/>
8 - There is a number of different hosting types. For this lesson, we will select the 'Host a static website' (we can also select 'Redirect requests for an object' in case we want to redirect traffic to a 3rd party bucket). Next, in order to enable the static website hosting feature we need to provide two files which can be found attached in the repository: the index.html and the error.html. The first acts as the home or default page of the static website hosting (what the user will see everytime it opens the website). The latter is used whenever we have an error (used, for instance, everytime we want to retrieve an object which does not exist in the bucket). So type in their respective boxes index.html and error.hml. In this demo we will not add any redirection rules, so scroll down and click on 'Save changes'. <br/>
9 -  The Static Website Hosting has been finally enabled (and we can access to the static website by copying into the clipboard and pasting it to the search bar the url displayed at the bottom), so it is time to upload some objects to the bucket. For this matter, click on 'Objects'. As mentioned before, we need to provide the index.html and error.html files, so click on 'Add files' and drop into the box the files attached to the repository. Finally, click on 'Upload'. <br/>
10 - Furthermore, we may add some content to the website, so click on 'Add folder' and upload the folder named 'img' following the same steps as above. These pictures will be displayed in our static website. <br/>
11 - If we try to browse our website by copying into the clipboard the url already mentioned (located at the very bottom of the 'Properties' page) and pasting it into the search bar we will come across an Acces Denied Error (403). This comes up as a result of not having the right permissions to access the objects contained within the bucket (remember that S3 Buckets are private by default thus eventhough we enabled the static website hosting feature objects within the bucket are still not accessible from the web browser. We are actually trying to access as an unauthenticated user so we need to hold the right creentials. That can be fixed by attaching a bucket policy which grants public access to any object to any anauthenticated user. So, in the bucket console select the 'Permissions' tab and scroll down to the Policy box, click on 'Edit' and paste there the bucket policy attached in the repository. The bucket policy should be visible as displayed in the picture below.<br/>
![Alt text](bucket_policy_pic.png)
Here, we allow (Effect : Allow) to read any object (Action: [s3:GetObject]) to any user (Principal : *) (authenticated or not) inside the bucket (Resource : arn:aws:s3:::<your_bucket_arn>/ *) (/* grants access to all objects within the bucket, so keep this part when replacing the arn). It is crucial to use the arn of your bucket, which can be found on top of the Bucket Policy box. Go ahead and click on 'Save changes' <br/>
12 - Finally, browse the static website into the search bar. Automatically the index.html will be loaded except we specify any object to retrieve. Below displayed is the expected output.<br/>
![Alt text](pic1.png)
13 - To test the error.html file, let's try to browse an object which does not exist by adding at the end of the url '/something.png'. Automatically, the error.html file will be loaded and displayed. Files from the bucket can be loaded by typing at the end of the url its filename. Below displayed is the expected output.<br/>
![Alt text](pic2.png)

---
## Optional
---
14 - One may need to assign a custom DNS name (custom domain name) to the bucket provided by Route53. For that matter, move to the Route53 console, click on 'Hosted Zones' and we should have a hosted zone that matches with the custom domain name we registered previously. <br/>
15 - Go inside that and click on 'Create record'. <br/>
16 - We will be creating a simple routing record so click on 'Simple routing' and then click on 'Next'. <br/>
17 - Next, we will define a simple record, so in the 'Record name' box paste the bucket name we create at the beggining. <br/>
18 - Next, since we need an endpoint to point to this S3 Bucket, we need to choose a record type, so click on the Record Type dropdown, scroll down and pick the 'Alias to S3 website endpoint'. <br/>
19 - Next, we need to choose the region so pick the region the bucket was created within (US East (N. Virginia)). <br/>
20 - Now, in the 'Enter S3 endpoint' pick our bucket name (if it does not appear then either the region used in this part and the bucket region do not match or you have not used the same name in this part of the record name as the bucket name). Finally, click on 'Define simple record'. <br/>
21 - Click on 'Create records'. <br/>
22 - Wait until the Satic Website endpoint is available (it may take some short time). <br/>
---
23 - Finally, let's tidy up all the resources created during the demo, so delete the record you created if you hold a custom domain name. <br/>
24 - Also, move to the S3 console, empty the bucket and finally delete the bucket. <br/>
