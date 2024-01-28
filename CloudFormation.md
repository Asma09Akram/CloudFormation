[LAMP_template.json](https://github.com/Asma09Akram/CloudFormation-Introduction/files/14076979/LAMP_template.json)### Task 1. Creating a S3 bucket and upload LAMP_template.json
1.1 Create a S3 bucket
   
![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/3fae4035-dafd-4e15-a9f4-b0f8375c76ea)

1.2. Uncheck Block all public access
   
![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/286ab585-316a-4a8e-ab08-84ed08cbd471)

1.3. Click on Create Bucket

![image](https://github.com/Asma09Akram/CloudFormation-Introduction/assets/124654068/861d44f2-e0ee-44de-b97c-6f1bb3bc70b5)

1.4 Upload LAMP_template.json file in s3 bucket which we have created


[Uploading LAMP{
  "AWSTemplateFormatVersion" : "2010-09-09",
  
  "Description" : "AWS CloudFormation Sample Template LAMP_Single_Instance: Create a LAMP stack using a single EC2 instance and a local MySQL database for storage. This template demonstrates using the AWS CloudFormation bootstrap scripts to install the packages and files necessary to deploy the Apache web server, PHP and MySQL at instance launch time. **WARNING** This template creates an Amazon EC2 instance. You will be billed for the AWS resources used if you create a stack from this template.",
  
  "Parameters" : {
      
    "KeyName": {
      "Description" : "Name of an existing EC2 KeyPair to enable SSH access to the instance",
      "Type": "AWS::EC2::KeyPair::KeyName",
      "Default": "EC2 keypair",
      "ConstraintDescription" : "must be the name of an existing EC2 KeyPair."
    },    

    "DBName": {
      "Default": "MyDatabase",
      "Description" : "MySQL database name",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "64",
      "AllowedPattern" : "[a-zA-Z][a-zA-Z0-9]*",
      "ConstraintDescription" : "must begin with a letter and contain only alphanumeric characters."
    },

    "DBUser": {
      "NoEcho": "true",
      "Description" : "Username for MySQL database access",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "16",
      "AllowedPattern" : "[a-zA-Z][a-zA-Z0-9]*",
      "ConstraintDescription" : "must begin with a letter and contain only alphanumeric characters."
    },

    "DBPassword": {
      "NoEcho": "true",
      "Description" : "Password for MySQL database access",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "41",
      "AllowedPattern" : "[a-zA-Z0-9]*",
      "ConstraintDescription" : "must contain only alphanumeric characters."
    },

    "DBRootPassword": {
      "NoEcho": "true",
      "Description" : "Root password for MySQL",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "41",
      "AllowedPattern" : "[a-zA-Z0-9]*",
      "ConstraintDescription" : "must contain only alphanumeric characters."
    },

    "InstanceType" : {
      "Description" : "WebServer EC2 instance type",
      "Type" : "String",
      "Default" : "t2.micro",
      "AllowedValues" : ["t2.micro"]
,
      "ConstraintDescription" : "must be a valid EC2 instance type."
    },

    "SSHLocation" : {
      "Description" : " The IP address range that can be used to SSH to the EC2 instances",
      "Type": "String",
      "MinLength": "9",
      "MaxLength": "18",
      "Default": "0.0.0.0/0",
      "AllowedPattern": "(\\d{1,3})\\.(\\d{1,3})\\.(\\d{1,3})\\.(\\d{1,3})/(\\d{1,2})",
      "ConstraintDescription": "must be a valid IP CIDR range of the form x.x.x.x/x."
    } 
  },
  
  "Mappings" : {
    "AWSInstanceType2Arch" : {
      "t2.micro"    : { "Arch" : "HVM64"  }
    },

    "AWSInstanceType2NATArch" : {
      "t2.micro"    : { "Arch" : "NATHVM64"  }
    }
,
     "AWSRegionArch2AMI" : { 
      "us-east-1"      : { "HVM64" : "ami-0080e4c5bc078760e" } 
    }

  },
    
  "Resources" : {     
      
    "WebServerInstance": {  
      "Type": "AWS::EC2::Instance",
      "Metadata" : {
        "AWS::CloudFormation::Init" : {
          "configSets" : {
            "InstallAndRun" : [ "Install", "Configure" ]
          },

          "Install" : {
            "packages" : {
              "yum" : {
                "mysql"        : [],
                "mysql-server" : [],
                "mysql-libs"   : [],
                "httpd"        : [],
                "php"          : [],
                "php-mysql"    : []
              }
            },

            "files" : {
              "/var/www/html/index.php" : {
                "content" : { "Fn::Join" : [ "", [
                  "<html>\n",
                  "  <head>\n",
                  "    <title>AWS CloudFormation PHP Sample</title>\n",
                  "    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=ISO-8859-1\">\n",
                  "  </head>\n",
                  "  <body>\n",
                  "    <h1>Welcome to the AWS CloudFormation PHP Sample</h1>\n",
                  "    <p/>\n",
                  "    <?php\n",
                  "      // Print out the current data and time\n",
                  "      print \"The Current Date and Time is: <br/>\";\n",
                  "      print date(\"g:i A l, F j Y.\");\n",
                  "    ?>\n",
                  "    <p/>\n",
                  "    <?php\n",
                  "      // Setup a handle for CURL\n",
                  "      $curl_handle=curl_init();\n",
                  "      curl_setopt($curl_handle,CURLOPT_CONNECTTIMEOUT,2);\n",
                  "      curl_setopt($curl_handle,CURLOPT_RETURNTRANSFER,1);\n",
                  "      // Get the hostname of the intance from the instance metadata\n",
                  "      curl_setopt($curl_handle,CURLOPT_URL,'http://169.254.169.254/latest/meta-data/public-hostname');\n",
                  "      $hostname = curl_exec($curl_handle);\n",
                  "      if (empty($hostname))\n",
                  "      {\n",
                  "        print \"Sorry, for some reason, we got no hostname back <br />\";\n",
                  "      }\n",
                  "      else\n",
                  "      {\n",
                  "        print \"Server = \" . $hostname . \"<br />\";\n",
                  "      }\n",
                  "      // Get the instance-id of the intance from the instance metadata\n",
                  "      curl_setopt($curl_handle,CURLOPT_URL,'http://169.254.169.254/latest/meta-data/instance-id');\n",
                  "      $instanceid = curl_exec($curl_handle);\n",
                  "      if (empty($instanceid))\n",
                  "      {\n",
                  "        print \"Sorry, for some reason, we got no instance id back <br />\";\n",
                  "      }\n",
                  "      else\n",
                  "      {\n",
                  "        print \"EC2 instance-id = \" . $instanceid . \"<br />\";\n",
                  "      }\n",
                  "      $Database   = \"localhost\";\n",
                  "      $DBUser     = \"", {"Ref" : "DBUser"}, "\";\n",
                  "      $DBPassword = \"", {"Ref" : "DBPassword"}, "\";\n",
                  "      print \"Database = \" . $Database . \"<br />\";\n",
                  "      $dbconnection = mysql_connect($Database, $DBUser, $DBPassword)\n",
                  "                      or die(\"Could not connect: \" . mysql_error());\n",
                  "      print (\"Connected to $Database successfully\");\n",
                  "      mysql_close($dbconnection);\n",
                  "    ?>\n",
                  "    <h2>PHP Information</h2>\n",
                  "    <p/>\n",
                  "    <?php\n",
                  "      phpinfo();\n",
                  "    ?>\n",
                  "  </body>\n",
                  "</html>\n"
                ]]},
                "mode"  : "000600",
                "owner" : "apache",
                "group" : "apache"
              },

              "/tmp/setup.mysql" : {
                "content" : { "Fn::Join" : ["", [
                  "CREATE DATABASE ", { "Ref" : "DBName" }, ";\n",
                  "GRANT ALL ON ", { "Ref" : "DBName" }, ".* TO '", { "Ref" : "DBUser" }, "'@localhost IDENTIFIED BY '", { "Ref" : "DBPassword" }, "';\n"
                  ]]},
                "mode"  : "000400",
                "owner" : "root",
                "group" : "root"
              },
              "/etc/cfn/cfn-hup.conf" : {
                "content" : { "Fn::Join" : ["", [
                  "[main]\n",
                  "stack=", { "Ref" : "AWS::StackId" }, "\n",
                  "region=", { "Ref" : "AWS::Region" }, "\n"
                ]]},
                "mode"    : "000400",
                "owner"   : "root",
                "group"   : "root"
              },

              "/etc/cfn/hooks.d/cfn-auto-reloader.conf" : {
                "content": { "Fn::Join" : ["", [
                  "[cfn-auto-reloader-hook]\n",
                  "triggers=post.update\n",
                  "path=Resources.WebServerInstance.Metadata.AWS::CloudFormation::Init\n",
                  "action=/opt/aws/bin/cfn-init -v ",
                  "         --stack ", { "Ref" : "AWS::StackName" },
                  "         --resource WebServerInstance ",
                  "         --configsets InstallAndRun ",
                  "         --region ", { "Ref" : "AWS::Region" }, "\n",
                  "runas=root\n"
                ]]},
                "mode"    : "000400",
                "owner"   : "root",
                "group"   : "root"
              }
            },

            "services" : {
              "sysvinit" : {  
                "mysqld"  : { "enabled" : "true", "ensureRunning" : "true" },
                "httpd"   : { "enabled" : "true", "ensureRunning" : "true" },
                "cfn-hup" : { "enabled" : "true", "ensureRunning" : "true",
                              "files" : ["/etc/cfn/cfn-hup.conf", "/etc/cfn/hooks.d/cfn-auto-reloader.conf"]}
              }
            }
          },

          "Configure" : {
            "commands" : {
              "01_set_mysql_root_password" : {
                "command" : { "Fn::Join" : ["", ["mysqladmin -u root password '", { "Ref" : "DBRootPassword" }, "'"]]},
                "test" : { "Fn::Join" : ["", ["$(mysql ", { "Ref" : "DBName" }, " -u root --password='", { "Ref" : "DBRootPassword" }, "' >/dev/null 2>&1 </dev/null); (( $? != 0 ))"]]}
              },
              "02_create_database" : {
                "command" : { "Fn::Join" : ["", ["mysql -u root --password='", { "Ref" : "DBRootPassword" }, "' < /tmp/setup.mysql"]]},
                "test" : { "Fn::Join" : ["", ["$(mysql ", { "Ref" : "DBName" }, " -u root --password='", { "Ref" : "DBRootPassword" }, "' >/dev/null 2>&1 </dev/null); (( $? != 0 ))"]]}
              }
            }
          }
        }
      },
      "Properties": {
        "ImageId" : { "Fn::FindInMap" : [ "AWSRegionArch2AMI", { "Ref" : "AWS::Region" },
                          { "Fn::FindInMap" : [ "AWSInstanceType2Arch", { "Ref" : "InstanceType" }, "Arch" ] } ] },
        "InstanceType"   : { "Ref" : "InstanceType" },
        "SecurityGroups" : [ {"Ref" : "WebServerSecurityGroup"} ],
        "KeyName"        : { "Ref" : "KeyName" },
        "UserData"       : { "Fn::Base64" : { "Fn::Join" : ["", [
             "#!/bin/bash -xe\n",
             "yum update -y aws-cfn-bootstrap\n",

             "# Install the files and packages from the metadata\n",
             "/opt/aws/bin/cfn-init -v ",
             "         --stack ", { "Ref" : "AWS::StackName" },
             "         --resource WebServerInstance ",
             "         --configsets InstallAndRun ",
             "         --region ", { "Ref" : "AWS::Region" }, "\n",

             "# Signal the status from cfn-init\n",
             "/opt/aws/bin/cfn-signal -e $? ",
             "         --stack ", { "Ref" : "AWS::StackName" },
             "         --resource WebServerInstance ",
             "         --region ", { "Ref" : "AWS::Region" }, "\n"
        ]]}}        
      },
      "CreationPolicy" : {
        "ResourceSignal" : {
          "Timeout" : "PT10M"
        }
      }
    },
    
    "WebServerSecurityGroup" : {
      "Type" : "AWS::EC2::SecurityGroup",
      "Properties" : {
        "GroupDescription" : "Enable HTTP access via port 80",
        "SecurityGroupIngress" : [
          {"IpProtocol" : "tcp", "FromPort" : "80", "ToPort" : "80", "CidrIp" : "0.0.0.0/0"},
          {"IpProtocol" : "tcp", "FromPort" : "22", "ToPort" : "22", "CidrIp" : { "Ref" : "SSHLocation"}}
        ]
      }      
    }          
  },
  
  "Outputs" : {
    "WebsiteURL" : {
      "Description" : "URL for newly created LAMP stack",
      "Value" : { "Fn::Join" : ["", ["http://", { "Fn::GetAtt" : [ "WebServerInstance", "PublicDnsName" ]}]] }
    }
  }
}_template.json…]()


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

2.3 
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



