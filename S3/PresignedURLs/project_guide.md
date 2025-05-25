# Creating and using PresignedURLs

In this demo lesson you will create a bucket, upload an object and generate a presignedURL allowing access for any unauthenticated identities. <br/>

1 - First, make sure you are logged in the AWS general management account in the N. Virginia region (us-east-1). Then, go ahead and move to the S3 console. Click on 'Create bucket', give it a name and leave the 'Block public access' option checked (keep it private). Also for this demo we will not use any form of encryption so go ahead and click on 'Create bucket'. <br/>

2 - Once you've done that, go to the bucket and upload the image attached to the repo. Next, click on this object and click on 'Open' on the top right of the screen (this will open the object in a new tab). It is relevant to point out some important points on how this object has been opened: if you takea moment to review the url that has been used to open this object, you will notice that a series of pieces of information has been specified in your url, including a security token, so essentially a form of authentication has been provided on the url which allows you to access this object. Let's contrast it with the 'Object URL' which can be found on the bottom right of the previous object panel. See that it does not provide any piece of authentication  for this object. Copy that into your clipboard and paste it into your browser. You will come across an access denied message (403) looking like this <br>
[Error message acess denied](deny_error_message.PNG)

which makes sense since we are trying to access this object as an unauthenticated identity, just like any internet user. The bucket should be made public in order to provide free access to the bucket object to any unauthenticated user. These are the main differences between both methods: one is providing identity authentication and the other is not. <br/>

3 - Now, we will work in the scenario we want to provide access to this object to an unauthenticated identity for a limited amount of time so you do not want to provide the url which contains authentication information: a time-limited presigned URL will do that work. For that matter, open a the AWS CloudShell which can be found at the bottom left of the page (the cloudshell is opened with the credentials of the identity we are logged in currently, this is, cloudshell will interact with AWS sing our current credentials). To create a presigned url, type this command on the cloudshell: <br/>

```
aws s3 presign <add_here_the_S3_URI_of_the_object> --expires-in <number_of_seconds_until_expiration>
```
In our case, this S3 URI should look like `s3://<your_bucket_name>/<your_pic_name>` (can be found on the object panel). For instance, if we aim to create a presigned URL in S3 to grant time limited access to an object called 'img.png' in the 'mybucket' bucket for 180 seconds, the prompt should look like this: <br/>
```
aws s3 presign s3://mybucket/img.png --expires-in 180
```
Below an example is shown: <br/>

[Example](example.PNG)

Once you've done that, click on enter. Automatically an URL will be generated, copy that into your clipboard and paste it on your browser, granting time-limited access to the object. You can check yourself that after exceeding that amount of time an error message should pop up like this, stating that the url is no longer valid: <br/>
[Error message time exceeded out](time_exceeded_error_message.PNG)
<br/>

4 - We will work a little bit more with presigned URLs, so this time create a new one but fix the amount of time the url will be valid to a large number such as 3600 (1 hour). That will be enough time to test some procedures on this time-limited new URL. Once you've done that, move to IAM service and open it on a new tab. Select 'Users' in the menu on the left, select the 'iamadminuser' or the admin user of your AWS account, move to 'Permissions' and in the dropdown click on 'Create policy', next click on 'JSON' and paste there all the content of the json file attached to the repo. This policy contains an explicit deny which denies to any user any acces to S3. Is read as Deny (Effect:Deny) to Any User any Action within S3 (Action: s3:*) to Any Object within s3 (Resource:2):<br/>

```
{
  "Version" : "2021-10-17",
  "Statement" : [
    {
      "Sid" : "VisualEditor0",
      "Effect" : "Deny",
      "Action" : "s3:*",
      "Resource" : "*"
    }
  ]
}
```
5 - Go ahead and click on 'Review policy', give a name for the policy and click on 'Create policy'. This policy will work as a deny inline policy to the IAM admin user. <br/>

6 - Now we will test how does this policy affect the presigned URLs generation, so clear the CloudShell by typing 'clear' and run `aws s3 ls`. We will come across an Acces denied error message because of applying a Deny policy to the IAM admin user (remember the hierarchy of permissions in AWS policies: eventhough the IAM admin user has administrator permissions, an exlicit deny any implicit or explicit 'allow' is overridden by an explicit 'deny'). As a result, the 3600 seconds time-limited presigned URL will no longer be valid since the policy spreads to all resources created by the identity the policy is attached to, therefore the presigned URL no longer has access to S3. See that we can regenerate the presigned URL by typing again the same prompt (`aws s3 <...>`), but again the acces denied message will pop up. Acces will be denied until the Explicit Deny policy is removed. Keep in mind the presigned URL will have the same permissions as the identity which created it holds at that specific point in time.<br/>

7 - See that we can generate presigned URLs for objects within a bucket which do not exist, but they will not route you to somewhere and an error message will be shown again. Also, presigned URLs support roles in the sense the validity of the presigned URL will depend on the permissions provided by that role to that identity at that specific point in time.<br/>

8 - The creation os a presigned URL can be done as well from the AWS console, so move to the object panel, click on 'Object actions' and next on 'Share <object name> with a presigned URL'. There, pick the time interval until the presigned URL expires and execute it by clicking on 'Create presigned URL'. The URL will be automatically copied into your clipboard. This is an alternative way to create AWS presigned URLs away from using the AWS CloudShell. <br/>

9 -  Tidy up and empty the bucket, delete it and remove the deny policy attached to the identity we have been working within. <br/>


