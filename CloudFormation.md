We used CloudFormation template which is written in YAML that defines the resources needed for the LAMP stack. This would typically include an EC2 instance with a Linux-based operating system, Apache web server, MySQL database, and PHP installation. We would also define necessary security groups, IAM roles, and other configurations.

## We will create a LAMP stack with the help of template in CloudFormation Stack.


### Task 1. Creating a S3 bucket and upload LAMP_template.json
1.1 Create a S3 bucket
   
![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/3fae4035-dafd-4e15-a9f4-b0f8375c76ea)

1.2. Uncheck Block all public access
   
![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/286ab585-316a-4a8e-ab08-84ed08cbd471)

1.3. Click on Create Bucket

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/861d44f2-e0ee-44de-b97c-6f1bb3bc70b5)

1.4 Upload LAMP_template.json file in s3 bucket which we have created

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/bf3a886c-73ac-48df-9bf1-5a4d69c21699)

1.5 Copy the Object Url of the file 

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/258bf5db-b9d9-478a-a8c1-82c9ef019d74)

https://mylamps3bucket.s3.ap-south-1.amazonaws.com/LAMP_template.json

### LAMP_template.json contains the JSON code for launching the LAMP Server using Cloudformation.

### Task 2  Create Cloudformation Stack

2.1 Click services, click on CloudFormation in the Management and Governance section.

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/9f4ad8be-a8b3-46b6-b25a-529325d156b7)

2.2 In the CloudFormation dashboard, click on create stack.

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/4a709768-ac83-43e7-87b6-70b981aefa63)

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/2e963eae-61e6-4bf6-9d90-d5193e3ebb79)

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/46bfba36-6cd0-4516-ad06-88bee3080771)

2.3 Give the name as MyLampStack

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/535435f1-269c-438a-b645-d33967cfe2d9)

2.4 Parameters

* DB Name : Enter a database name - MyDatabase
* DB Password : Enter a database password - mylabsdb123
* DB Root Password : Enter database root password - mylabsdbroot123
* DB User : Enter the database username - MylabsDBUser
* Instance Type : Select t2.micro
* Key Name : Select the key from the list name EC2 keypair
* SSH Location : Enter 0.0.0.0/0

Click on Next.

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/05719efd-4ea3-4de9-8f89-b66ae6b10259)

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/dd8dcaf8-cfe6-4358-9242-d5170b631fff)

2.5 Leave all other configuration fields as default.

2.6 Review: Review your stack details and click on Submit.

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/194d8d5e-9345-46e4-8d3a-95eac8bceb71)

2.7 After sometime stack status changes to Create Complete. 

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/501477a3-2acc-438d-8360-3b2f26ca00fe)


### Task 3 Testing 

3.1 Navigate to the outputs tab and you will be able to see an URL similar to below. Click on the URL. This will take you to your server's home page.

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/80a73cdc-0e5e-4a7c-9d2e-c45a8871bdca)

http://ec2-54-221-107-28.compute-1.amazonaws.com

3.2 If you see the PHP info and your database connection, it means you have completed a LAMP server setup with AWS CloudFormation. Sample screenshot provided below:

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/5c310454-bfd6-4f68-9b4b-84acc6b4c484)



