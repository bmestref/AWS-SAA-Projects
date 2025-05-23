# Object Encryption and Roles Separation

In this lesson we will be creating an S3 bucket, and uploading 3 images to the bucket using different encryption methods.
After adjusting the permissions of the IAMADMIN user we will review what access changes occur, and why.
This DEMO focusses on the role separation aspect of S3 encryption using KMS. <br/>

1 - Ensure we are logged in the AWS account as IAMUSER in the us-east-1 region. <br/>
2 - Create a bucket as done in the previous lessons (we will skip this part). <br/>
3 - Once the bucket is created, move to the KMS console, click on 'Customer managed keys' and then on 'Create key'. This will enable the feature of creating a unique encryption key which will keep our files encrypted and secure. <br/>
4 - For this demo lesson, pick the 'Simmetric key' option (the key is used for both encryption and decryption processes). <br/>
5 - Next, pick the 'Encrypt and decrypt' option under 'Key usage', and then make sure 'KMS' is selected in the 'Advanced options' dropdown as well as 'Single-Region key' under 'Regionality' (for now we will limit the usage of the key to us-east-1 region).<br/>
6 - Finally, click on 'Next' at the bottom of the page. <br/>
7 - Now, we need to provide an alias to the key, so type whatever you want. Then click on 'Next'. <br/>
8 - For this time, we will not select any key policy so go and click on 'Next' without selecting any 'Key administrators'. <br/>
9 - On the next page click again 'Next' (we will not select any usage permission for this key). <br/>
10 - For this demo the root user will be the only user with granted permissions to use the key (as shown in the 'Key policy' box). That is the default configuration, so click on 'Finish'. <br/>
11 - Once you've done that, move back to the S3 console and populate the S3 bucket we created before with some objects (they can be found attached to the current repo). Since we will enable the encryption feature on each object, they must be uploaded one by one. So, in the 'Server-side encryption' after clicking on 'Expand Properties' select 'Specify an encryption key'. In this demo we will be using our custom encryption key created before, so check 'Overwrite bucket settings for default encryption. <br/>
12 - Next, we are able to pick between two Encryption key types: S3 managed and KMS managed. The main point of using the first one relies on the fact it is fully managed, but that may be one of its main drawbacks as well: S3 has full control on them, so critical or sensitive keys should not be managed by a third party but instead by the user himself. In the other hand, AWS Key Management Service by KMS provides full control to the user to manage all encryption keys. For now, pick the S3 managed key option (SSE-SE) (low admin overhead at the expense of potential privacy leakage). Scroll down and click on 'Upload'.<br/>
13 - Follow the same steps for a new object, but this time pick the SSH-KMS managed encryption option. This will allow us to use the KMS key we created before, so on the features panel below select 'Choose from your AWS KMS keys'. In the search bar below, look for the name of the KMS key and use it to encrypt our objecy. Scroll down to the bottom and click on 'Upload'.<br/>
14 - Once you have done that, you can check you can access and see these objects since we are logged in as the IAM User, the root user which holds the encryption key. <br/>
13 - Let's play a little bit more with key permissions. This time, we will apply a Deny policy on the IAM user so it can no longer can read these objects.
12 - 
