# AWS-Project
I am going to introduce an AWS architecture of an application. This is a complete application that you will be building in a step-by-step manner. With every module you complete, you will be adding a new service or resource to your architecture,But before we start, let’s look at the prerequisites. 
Prerequisites:
 
1.	An AWS account with privileges to create IAM roles, AWS VPCs, EC2 instances, and RDS databases. 

Problem:

In this lab, you will create the VPC inside your AWS account. As you know, before you create VPC, you need to select a region by considering factors like cost, compliance, latency, etcetera. You will also need two subnets inside your VPC. A public subnet to host your web application. For that, you will launch an EC2 Instance. Also, a private subnet to deploy your RDS MySQL database. Then, you will set up the connection between RDS and EC2, then install the WordPress website on the instance. Finally, you will start hosting a simple WordPress website on an EC2 instance and export the static assets into the S3 bucket.

 Architecture:
 
![image](https://user-images.githubusercontent.com/63364609/225718740-959f0476-b0b0-4f21-bfb9-8bdc95fbb4fc.png)


•	Create a VPC inside your AWS account. 
What is VPC??
VPC stands for Virtual Private Cloud, which is a virtual network infrastructure provided by Amazon Web Services (AWS). It enables you to create a logically isolated section of the AWS cloud where you can launch resources such as EC2 instances, RDS databases, and more.
A VPC allows you to have complete control over your network configuration, including IP address ranges, subnets, and routing tables. You can also create security groups and network access control lists (ACLs) to control traffic to and from your instances.
With a VPC, you can extend your data center into the cloud and connect to it securely using an IPsec VPN or AWS Direct Connect. Additionally, you can integrate your VPC with other AWS services like Amazon S3, AWS Lambda, and more.

Step 1 – Open AWS account, search VPC in search bar and click on VPC 

![image](https://user-images.githubusercontent.com/63364609/225718899-c3b114af-a84f-411d-8403-bd3000c6b32c.png)

Before creating an own VPC, lets understand about default VPC
By default, when you create a new AWS account, a default VPC (Virtual Private Cloud) is created for you in each AWS region. The default VPC is a logically isolated virtual network within the AWS Cloud that you can use to launch your AWS resources, such as EC2 instances, RDS databases, and more.
The default VPC comes preconfigured with several default settings, including an Internet Gateway and a default subnet in each Availability Zone within the region. This means that you can launch your resources in the default VPC without having to worry about configuring networking settings.

![image](https://user-images.githubusercontent.com/63364609/225718961-68751fda-fd23-4314-b407-045ee7ec44a6.png)

 
Step 2 - Click on create VPC, select VPC only and entre IPv4.

![image](https://user-images.githubusercontent.com/63364609/225718998-ccac4f73-ccc9-4445-8a04-87a63b72f447.png)
 
•	A Main Route table will be created automatically but it has only local in target group.

![image](https://user-images.githubusercontent.com/63364609/225719021-56d6b620-0cbb-4467-a61c-397ba5b3bb22.png)
 
•	Create a Public Subnet in your Custom VPC 

What is Public Subnet?
A public subnet is a portion of a computer network that is accessible from the Internet, meaning it has a public IP address that can be reached by anyone on the Internet. In a public subnet, resources like web servers, email servers, and other services that need to be publicly accessible are typically deployed.
Public subnets are often used in cloud computing environments like Amazon Web Services (AWS), where they are paired with a private subnet to create a virtual private cloud (VPC). The private subnet is used to host resources that should not be accessible from the Internet, such as databases, backend servers, and internal applications.

![image](https://user-images.githubusercontent.com/63364609/225719058-d8f8ee10-5274-4b4b-a65d-e92a35358f7a.png)
 
In above pic you can see subnets which are create by AWS when default VPC is created.
Step 1 – Click on Create subnet, select your custom VPC, Give name to subnet, select Availability zone according your convince and assign IPV4 CDIR block to this subnet.

![image](https://user-images.githubusercontent.com/63364609/225719080-72c8dec4-7b70-4f3a-9eff-d55041eb46da.png)
 
Step 2 – Make Auto-assign IP enable because as name suggest it is Public subnet.

![image](https://user-images.githubusercontent.com/63364609/225719107-f47cea2b-da8f-4812-926e-da764fe899e2.png)
 
Step 3 – Create an Internet gateway to your Custom VPC

What is an Internet Gateway?
An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available component in Amazon Web Services (AWS) that allows communication between resources in a VPC and the internet.
An Internet Gateway is a virtual router that connects a VPC to the internet. It provides a target for traffic destined for the public internet from instances in the VPC and a source for traffic originating from the internet and intended for instances in the VPC.
It is an AWS-managed component that is attached to your VPC and It acts as a gateway between your VPC & the internet, basically the outside world.

![image](https://user-images.githubusercontent.com/63364609/225719140-860f48f9-fbee-439b-a925-3e26af1b798e.png)
 
In above pic you can see an internet gateway already create which is create by AWS when default are VPC is created.

•	Click on Create internet gateway, give name to it and create internet gateway

![image](https://user-images.githubusercontent.com/63364609/225719197-9f66688f-9d0d-470f-b862-c0c1aae0e497.png)
 
•	Now, Attach this internet gate to your custom VPC 

![image](https://user-images.githubusercontent.com/63364609/225719226-a773ca2f-0b19-4bb4-ac7a-ef6fda7a8511.png)
 
![image](https://user-images.githubusercontent.com/63364609/225719254-3d80b98e-add3-4e87-929f-bbffb2c4758f.png)

![image](https://user-images.githubusercontent.com/63364609/225719275-d2a16a9f-11d2-4406-8d95-6bbfac462a61.png)
  
Step 4 – Add Internet way id to main route table 

What is Route table ?
In Amazon Web Services (AWS), the main route table is the default route table that is created when you create a VPC. Every subnet that you create in the VPC is associated with this default route table unless you explicitly associate it with a custom route table.
The main route table contains rules that define how traffic is directed within the VPC. It is used to route traffic between subnets within the VPC and to control access to and from the internet. By default, the main route table has a route that sends all traffic to a local route for communication within the VPC.
You can add, modify or delete the routes in the main route table to control the flow of traffic within your VPC. For example, you can add a route that sends traffic to an Internet Gateway to allow resources in a public subnet to access the internet. You can also add a route that sends traffic to a NAT Gateway to allow resources in a private subnet to access the internet.

![image](https://user-images.githubusercontent.com/63364609/225719377-9222e9e4-7a74-41b5-bec4-7430a8b1e8dd.png)

Select route table and go to edit routes in that add destination as 0.0.0.0/0 and past your internet gateway id in target and save the changes.

![image](https://user-images.githubusercontent.com/63364609/225719405-16e41b4b-9e99-4b58-aa93-7721105016c0.png)
 
•	Create a Ec2 instance in public subnet of custom VPC

Give name to that instance, select instance type, create a key pair and in network setting select your custom VPC and public subnet of it. 

![image](https://user-images.githubusercontent.com/63364609/225719440-3b1b794d-13f1-4ea1-bf61-988d444ef69c.png)


•	Create a Private Subnet in your Custom VPC 
In Amazon Web Services (AWS), a private subnet is a subnet within a VPC that does not have a direct route to the Internet, meaning it is not associated with an Internet Gateway. Instances within a private subnet can communicate with other instances within the same VPC or with other VPCs through a VPN connection or a VPC peering connection.
Typically, resources that require a high degree of security, such as databases, backend servers, or internal applications, are deployed in private subnets. Since they are not directly accessible from the Internet, they are less vulnerable to attacks from the public Internet. Instead, they can only be accessed by authorized users or resources within the same VPC.

Step 1 – click on create a subnet then Select your custom VPC, give name to subnet, select availability zone and assign a IPV4 CIDR block to this subnet.

![image](https://user-images.githubusercontent.com/63364609/225719477-243b4274-4218-4eef-90f8-2bf1a94cf858.png)
 
Note – 
•	Here, Auto-assign IP will be disable because as name suggest it is Private subnet.
•	Internet gateway is not required as it is private subnet.

Step 2 – Create a Route table and keep only local target in it.

![image](https://user-images.githubusercontent.com/63364609/225719540-52762f94-e6fc-4502-91e6-e50d82837dd0.png)
 
![image](https://user-images.githubusercontent.com/63364609/225719565-7e179e1e-b53e-4a8e-a23e-47c4faeac898.png)

![image](https://user-images.githubusercontent.com/63364609/225719588-6aa71451-b6cb-4396-8fec-db4332315dfc.png) 

In below image you can see resource map, Public subnets are attached to Route table and Internet gateway, where Private subnets are attached to only Route table without Internet gateway access.

![image](https://user-images.githubusercontent.com/63364609/225719631-9500fcf4-058a-444b-aeac-74b860a5edbd.png)
 
•	Create an RDS MySQL database in Private subnet of custom VPC.
RDS - RDS (Relational Database Service) is a web service provided by AWS (Amazon Web Services) that makes it easy to set up, operate, and scale a relational database in the cloud. RDS supports multiple database engines, including MySQL, PostgreSQL, Oracle, and SQL Server.
RDS allows users to manage their databases in a cloud-based environment without the need for extensive hardware or software resources. It provides automated backups, software patching, and scalable storage, making it an efficient and cost-effective solution for database management.
When using RDS in AWS, users can select from several instance types, which determine the computing and memory capacity of the database instance. RDS also supports read replicas, which allow users to create multiple copies of their database for read-intensive workloads.
Step 1 - Before Creating RDS, First create a subnet group consisting of only private subnets in RDS.

![image](https://user-images.githubusercontent.com/63364609/225719670-47c3f3ab-0b2d-4cfd-bf5f-038de60916d0.png)
 
Step 2 – Create database by click on create database.

![image](https://user-images.githubusercontent.com/63364609/225719694-067c7af5-38cc-4868-bb0b-ea9f47ae28b9.png)
 
Step 3 – Select Standard create, choose mysql and version, and I select free tier template 

![image](https://user-images.githubusercontent.com/63364609/225719732-9b19a0bd-204f-4f34-9dc9-5e06e6c4192f.png)

![image](https://user-images.githubusercontent.com/63364609/225719762-000d59c8-35a3-427c-965c-308284378248.png)
 
Step 4 – In setting type a name for your database, type master username and type of password.

![image](https://user-images.githubusercontent.com/63364609/225719784-a954e7a3-5ddc-408c-8be7-fbff5eb8d67a.png)
 
Step 5 – In Instance configuration select Db instance class and select storage type and size. 

![image](https://user-images.githubusercontent.com/63364609/225719804-0a8289a7-f5a8-45d9-8127-28fb2c790227.png)
 
Step 6 – In connectivity choose your VPC and Public access NO.

![image](https://user-images.githubusercontent.com/63364609/225719836-d1ab763d-650a-40f1-97ea-40d694792c1a.png)
 
Step 7 – Select Database authentication as password authentication, and click on click on database  

![image](https://user-images.githubusercontent.com/63364609/225719861-1016d2a1-10ce-436a-bf56-35db82d06103.png)

![image](https://user-images.githubusercontent.com/63364609/225719881-1f155a4f-d9ae-40a4-860c-8e33ae75fd80.png)
   
Step 8 - Now, we will continue further and modify security group of RDS and EC2 instance. So, we need EC2 instance. So please go ahead and create EC2 instance and all missing AWS resources which we created so far.
•	select public-instance-sg,
•	Click Edit inbound rules button
•	Click Add rule, For Type, select MYSQL/Aurora,
•	For Source, select custom and find the assg-rds-mysql-sg , and click Save rules
•	Click Add rule, For Type, select HTTP,
•	For Source, select My IP, and click Save rules

![image](https://user-images.githubusercontent.com/63364609/225719905-5913be17-1dd1-4848-ac3c-0f9aa046c7a6.png)

•	select db-sg ,
•	Click Edit inbound rules button
•	Delete existing rule
•	Click Add rule, For Type, select MYSQL/Aurora,
•	For Source, select custom and find the public-instance-sg , and click Save rules

![image](https://user-images.githubusercontent.com/63364609/225719925-32dd10a7-d2ba-4189-be81-5f8e602ead6b.png)

•	Create an S3 bucket with unique name
Step 1 – Search S3 in search bar and create an S3 bucket with the “assg-bucket-<random-number>” name.

![image](https://user-images.githubusercontent.com/63364609/225719960-4ae04212-b06b-4fdf-a5cb-bea411716d71.png)
 
Step 2 – Block Public Access to this bucket and click on create bucket. 

![image](https://user-images.githubusercontent.com/63364609/225719983-ae3faa8c-549c-449b-be68-229480789b63.png)

![image](https://user-images.githubusercontent.com/63364609/225719998-5b0bda78-3442-4130-bddf-54b9b9cd2c09.png)

•	Set Up the Wordpess Environment

Step 1 - Select the Public instance and click Connect,
•	In Connect to instance page, click SSH client on menu and you can find the connection command in Example.
•	Windows User: follow the step in this deck to connect EC2 instance by Putty
•	MacOS User: open a terminal on your computer, navigate to the folder where your.pem key file is stored, and paste the connection command.

![image](https://user-images.githubusercontent.com/63364609/225720079-3a629cd2-992d-4651-9eaa-377f599cb60f.png)
 
Step 2 - After connection successes, run the following command to install LAMP stack (Linux, Apache, Mariadb and php) on your instance
o	sudo yum install httpd -y
o	sudo service httpd start
o	sudo service httpd status

o	sudo yum install mariadb-server -y
o	sudo service mariadb start
o	sudo service mariadb status

o	sudo amazon-linux-extras install php8.0 -y
o	sudo service php-fpm start
o	suod service php-fpm status

o	sudo service httpd restart
o	sudo service mariadb restart
o	sudo service php-fpm restart

Here, I’m installing LAMP using shell script and we have to give execute permission to that LAMP file.

![image](https://user-images.githubusercontent.com/63364609/225720119-00c1884e-8bed-43d6-907a-dd0f7eb3e9e3.png)

![image](https://user-images.githubusercontent.com/63364609/225720138-eeaf4df1-925e-41c0-afcf-ea641936fd78.png)
 
Step 3 - Set the environment variable of MySQL in your computer, replace <your-endpoint> to the endpoint which can be found in RDS console→ your database.
o	sudo mysql -h <your-endpoint>  -u <username> -p <password>
o	create a database called database1

![image](https://user-images.githubusercontent.com/63364609/225720178-5f7ddf63-2b82-45d2-a5ca-3c7ce6e411cd.png)
 
Step 4 – Go to html using path cd /var/www/html then, download the WordPress module and unzip it
o	wget https://wordpress.org/latest.tar.gz
o	tar -xzf latest.tar.gz
 
![image](https://user-images.githubusercontent.com/63364609/225720203-5c27fc02-3b3d-47c1-8697-b7999a5f223d.png)

![image](https://user-images.githubusercontent.com/63364609/225720224-c78841a1-b97e-49a6-a78f-f05b73631a3f.png)

Step 5 - Move into WordPress folder and backup the default config file
o	cd wordpress
o	cp wp-config-sample.php wp-config.php

![image](https://user-images.githubusercontent.com/63364609/225720258-ee48bdd1-69d4-4fd4-8f44-c73466c72621.png)
 
Step 6 -After that use nano to edit the wp-config.php file
o	nano wp-config.php
Step 7 - Modify the following script into the correct value:
DB_NAME:  'wordpress'● DB_USER:  'wordpress' ● DB_PASSWORD: 'wordpress-pass'● DB_HOST: your RDS endpoint and save it.

![image](https://user-images.githubusercontent.com/63364609/225720299-69d5cb68-587d-4b57-b52e-cd6696b67c97.png)
 
Step 8 - Select the EC2 instance, and find the Public IPv4 DNS in Details below and paste it on your browser, then you will see the setup page of WordPress.
In setup page, enter your own value in the Site Title, Username, Password and Your Email, then click Install WordPress

![image](https://user-images.githubusercontent.com/63364609/225720334-4120f4d9-5c07-4b2a-9269-2e8e14049511.png)

•	After few seconds, it will be redirected to Login page,
•	Enter your username and password, and click Login button, you will see the admin page,
•	You can use Admin dashboard enhance your blog.
•	Now, you can view your blog in <your ec2 domain>
 
![image](https://user-images.githubusercontent.com/63364609/225720368-646334a3-2ff1-4c95-93f5-cd24b97c7f04.png)


Congratulations, you have successfully hosted a simple WordPress website on EC2 instance and configured all required AWS services. 

In case you have any confusion, you can drop message here. 

That’s all for this assignment. 


