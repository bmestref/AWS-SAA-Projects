# How to Create a Static Website Hosted by S3
1 - First, make sure you are logged in as IAM User, in the Northern-Virginia region. <br/>
2 - Move across the AWS Console to S3, and click on create a new S3 bucket <br/>
3 - Fill the Bucket Name box with a unique name (the S3 bucket will not be created if there is another one which holds the same name. Make sure it is unique). One can create the bucket within a custom domain by adding at the end if the bucket name the domain identifier. <br/>
4 - Scroll down and uncheck 'Block all public access' (this will enable external people to access the static wesbite). <br/>
5 - To confirm we grant public access to the S3, check the 'I acknowledge that the current settings...' just below. <br/>
6 - For this demo lesson, you can leave the rest as default, so scroll down to the bottom and click on 'Create bucket'. <br/>
7 - So far, this S3 bucket can only be accessed via S3 API or Console UI, so we need to enable the Static Website Hosting feature in the S3 bucket, so click on the bucket and next click on 'Properties'. On the properties tab scroll down all the way 
to the bottom and right at the very bottom we have 'Satic website hosting'. Now, you need to click on the 'Edit' tab and click on the 'Enable' button. <br/>
8 - There is a number of different hosting types. For this lesson, we will select the 'Host a static website' (we can also select 'Redirect requests for an object' in case we want to redirect traffic to a 3rd party bucket). Next, in order to enable the static website hosting feature we need to provide two files which can be found attached in the repository: the index.html and the error.html. The first acts as the home or default page of the static website hosting (what the user will see everytime it opens the website). The latter is used whenever we have an error (used, for instance, everytime we want to retrieve an object which does not exist in the bucket). So type in their respective boxes index.html and error.hml. In this demo we will not add any redirection rules, so scroll down and click on 'Save changes'. <br/>

9 -  The Static Website Hosting has been finally enabled (and we can access to the static website by copying into the clipboard and pasting it to the search bar the url displayed at the bottom), so it is time to upload some objects to the bucket. For this matter, click on 'Objects'. As mentioned before, we need to provide the index.html and error.html files, so click on 'Add files' and drop into the box the files attached to the repository. <br/>
10 - Furthermore, we may add some content to the website, so click on 'Add folder' and upload the folder named 'pics' attached as well in the repository. <br/>
11 - 
