# CloudFormation-Introduction

### What is a LAMP stack?
A LAMP stack is a bundle of four different software technologies that developers use to build websites and web applications. LAMP is an acronym for the operating system, Linux; the web server, Apache; the database server, MySQL; and the programming language, PHP. All four of these technologies are open source, which means they are community maintained and freely available for anyone to use. Developers use LAMP stacks to create, host, and maintain web content. It is a popular solution that powers many of the websites you commonly use today.

### AWS CloudFormation

CloudFormation is a service provided by AWS for designing our own infrastructure using code, i.e. infrastructure as code.
AWS CloudFormation gives you an easy way to model a collection of related AWS and third-party resources, provision them quickly and consistently, and manage them throughout their lifecycles, by treating infrastructure as code. A CloudFormation template describes your desired resources and their dependencies so you can launch and configure them together as a stack. You can use a template to create, update, and delete an entire stack as a single unit, as often as you need to, instead of managing resources individually. You can manage and provision stacks across multiple AWS accounts and AWS Regions.

CloudFormation allows you to model your entire cloud environment in text files. You can use open-source declarative languages, such as JSON or YAML, to describe what AWS resources you want to create and configure. If you prefer to design visually, you can use AWS CloudFormation Designer to help you get started with AWS CloudFormation templates.

![image](https://github.com/Asma09Akram/CloudFormation/assets/124654068/a5005449-8527-4fc2-a174-a9dbd5623dae)

CloudFormation Template consists of following sections like 
* AWS Template Format Version
* Description
* Metadata
* Parameters
* Mappings
* Conditions
* Resources (Required Field)
* Outputs

### CloudFormation Stack
A stack consists of a collection of resources. 
* In other words, the stack consists of one or more templates.
* The advantage of the stack is that it is easy to create, delete or update the collection of resources.
* The advanced stacks have a nested stack which holds a collection of stacks.


Here we will be doing the following task
1. Sign in to AWS Management Console.
2. Creating S3 bucket and adding LAMP_template.json file
3. Create CloudFormation Stack.
4. Testing
