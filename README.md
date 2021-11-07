# SSM-PARAMETER-STORE-CLI
Store data in SSM Parameter store and view from AWS CLI
This PROJECT invlove the steps to store the sample data in AWS Systems Manager's Parameter Store and View the data from CloudShell CLI.

we will also practice storing encrypted data (Secure string) in SSM Parameter Store and data will be encrypted by AWS KMS.



AWS Region: US East (N. Virginia) us-east-1

About SSM Parameter Store
 Use case of Parameter store

Parameter Store can be used for securely storing configuration and secrets.

we can also encrypt the data AWS KMS

It is claimed to be a serverless service, easily scalable, highly durable, easy SDK, and free to use.

 can use the version tracking feature for your configurations/secrets.

It provides configuration management using path & IAM.

Notifications can be enabled with CloudWatch Events.

It can also be Integrated with CloudFormation.

In this project will be storing the database endpoint and database password in the SSM Parameter Store, and viewing via SSH through CloudShell, which is AWS CLI integrated into the Console.




Task Details
Launching  Environment

login to aws 

Task 2: Create Parameter Store and store the data
Make sure you are in US East (N. Virginia) us-east-1 Region. 

Navigate to Systems Manager by clicking on the  menu in the top, then click on  under  section.

On the left panel, scroll down to the Application Management and click on the Parameter Store.


On the home page of the AWS Systems Manager Parameter store, Click on the  button.

 will be storing two parameters, db-endpoint as String and db-password as SecureString.

Enter the following details about Parameter db-endpoint:

Name: Enter /whiz-app/dev-env/db-endpoint

Description: Enter Endpoint of the database for the development environment

Tier: Select Standard



Type: Select String

Data type: Select text

Value: Enter mysqlinstance1-k82h.123456789012.us-east-1.rds.amazonaws.com:3306



To create, click on the  button.
 

Click on Create parameter button

Enter the following details about Parameter db-password:

Name: Enter /whiz-app/dev-env/db-password

Description: Enter Password of the database for the development environment

Tier: Select Standard

Type: Select SecureString



KMS key source: Select My current account

KMS Key ID: Select alias/aws/ssm

Value: Enter Eh9x6E!RGHa8zYM



To create, click on the  button.
 

Both db-endpoint and db-password, parameters are now created.


Task 3: Create an Environment in CloudShell
Navigate to CloudShell by clicking on the  menu in the top, then click on  under  section.

Optionally, you can start CloudShell by Clicking on the  icon (CloudShell) on the top right AWS menu bar.

A new tab in your browser opens and if you see a welcome message to cloud shell then click on the  button in that message.

now will see a creating environment message on the screen.

         

Task 4: View the data from CloudShell using get-parameters
To view the db-endpoint, which is stored in String format, run the following command:

aws ssm get-parameters --names /whiz-app/dev-env/db-endpoint


To view the db-password, which is stored in SecureString format, run the following command:

aws ssm get-parameters --names /whiz-app/dev-env/db-password



Oh, looks like this is encrypted, let's decrypt it. To view the db-password, in decrypted format, run the following command:

aws ssm get-parameters --names /whiz-app/dev-env/db-password --with-decryption



To view both db-endpoint and db-password with a single command, run the below CLI command:

aws ssm get-parameters --names /whiz-app/dev-env/db-endpoint /whiz-app/dev-env/db-password



Oh, Looks like db-password is still in encrypted format, let's decrypt it with --with-decryption parameter

aws ssm get-parameters --names /whiz-app/dev-env/db-endpoint /whiz-app/dev-env/db-password --with-decryption



Task 5: View the data from CloudShell CLI using get-parameters-by-path
To view both the parameters of the development environment, run the following command:

aws ssm get-parameters-by-path --path /whiz-app/dev-env


we  can decrypt the above SecureString by adding --with-decryption parameter

aws ssm get-parameters-by-path --path /whiz-app/dev-env --with-decryption

Alternatively, we can view the parameters of /whiz-app path by adding --recursive parameter.

aws ssm get-parameters-by-path --path /whiz-app --recursive --with-decryption




