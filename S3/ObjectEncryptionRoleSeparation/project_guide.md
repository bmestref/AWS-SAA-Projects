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
10 - 
