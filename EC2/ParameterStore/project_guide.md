# Parameter Store

In this mini project we will create some Parameters in the Parameter Store and interact with them via the command line - using individual parameter operations and accessing via paths. <br/>

1 - As always, make sure we are logged in the Management AWS account of your organization while working within the us-east-1 region. <br/>

2 - Parameter Store is actually a sub-service the System Manager, therefore move to the AWS Service Manager console and in the menu on the left click on 'Parameter Store'. Next, click on 'Create parameter'. First, enter a name for the parameter (names follow hierarchy based on the use of ```/```, such that parameters with name ```/myrepo/<name>``` do share the same hierarchical level) and optionally a description. We can now pick between the Standard tier and the Avanced. For most cases, the first will be enough (use the latter in case you plan to create more than 10000 parameters, this option falls apart from the free tier), so pick up the first, next below 'Type' select the type options which suits the best to your case (String for a single value parameter, StringList for a comma separated multi-value parameter and SecureString for encrypted parameters via KMS keys, the latter specially designed to store passwords and sensitive data). In case the SecureString type is selected, we are now able to pick between using 'My current account'KMS key or use one from 'Another account'. On both cases, we will need to provide a KMS Key ID. Below, enter the 'Data type' the parameter will store and enter as well a value for it. Scroll down and finish the parameter creation by clicking on 'Create parameter'. <br/>

3 - Now, in order to interact with these parameters move to the AWS CloudShell console (click on the CloudShell icon on the top of the page). It will need some minutes to create the environment to prepare the terminal. Next, using the credentials we are logged in, we can retrieve the parameter content in JSON form by typing: <br/>

```
aws ssm get-parameters --names <parameter name>
```

4 - Parameters do follow a hierarchical schema, so we can also get parameters by entering the path of the level above: <br/>

```
aws ssm get-parameters --path /<level_above>/
```

As a result, all parameters stored witin the <level_above> level will be retrieved at once. See that encyrpted parameters using the SecureString parameter type will yeild its content encrypted running the command above. This adds another layer of security as we can interact with parameters without compromising its content. In case the user had the correct KMS permissions to interact with the Parameter Store service, SecureString parameters can be decrypted and retrieve the real content running the command below: <br/>

```
aws ssm get-parameters-by-path --path <path to the level above of the encrypted parameter> --with-decryption
```

Now, all parameters within ```<path to the level above of the encrypted parameter>``` are decrypted wether they were SecureString type or not. <br/>

5 - Finish up this mini-project clearing and removing all parameters created throughout. <br/>
