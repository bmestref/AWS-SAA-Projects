# Migrating EC2 DB into RDS

In this mini-project we will create a MySQL RDS instance and migrate the Wordpress Database from the self-managed MariaDB server running on EC2 into this RDS instance.

1 - As always, make sure you're logged in the General AWS account in the us-east-1 region.
2 - Next, to start from the point we left on the previous lesson, we will deploy some infrastructures using a CloudFormation template attached to the project. Wait until the stack has been deployed successfully.
3 - We can check what resources were created by moving to the EC2 console. Two instances have beenc created with the CloudFormation template, the same two as we used in previous mini-projects, one hsoting the Apache Web Server and the other running the Maria database
