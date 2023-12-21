---
title: 3 Tier Architecture in AWS
description: 
published: true
date: 2023-08-02T19:35:40.761Z
tags: 
editor: markdown
dateCreated: 2023-08-02T19:26:46.713Z
---

# 3 Tier Architecture in AWS

![1_kj9n3pvwjqrnsck1q9o_da.webp](/assets/img/clouds/tempo/1_kj9n3pvwjqrnsck1q9o_da.webp)

Lets talk about this for a second! What is a 3 tier architecture? The three-tier architecture is the most popular implementation of a multi-tier architecture and consists of a single presentation tier, logic tier, and data tier. Some advantages are maintainability, scalability, flexibility, availability, reusability, and faster development.

- **Presentation Tier** - This is the topmost level of the application responsible for collecting the information. It provides user interface and handles the interaction with the user, it sends content to browsers in the form of HTML/JS/CSS, and it communicates with other tiers by which it provides the results to the browser/client side.
- **Logic Tier** - Also known as the application tier. This contains a set of rules for processing information, business logic and able to accommodate many users. Sometimes also called as middleware. It processes the inputs received from the clients and interacts with the database and runs on a web server.
- **Data Tier** - A database, comprising both data sets and the database management system or RDBMS software that manages and provides access to the data (back-end). It provides security, data integrity and support to the application.

**Scenario:**

You have been asked to design and create a highly available 3 Tier architecture for your company’s new web application. Lets break this down for each individual tier to make it easier:

## ☁️ Build your VPC:☁️
> 2 public subnets associated with a public route table.
> 
> Minimum of 2 EC2’s with a bootstrapped static webpage and auto scaling group.
>
> EC2 security group allowing inbound permissions.

### Step 1: Create VPC

- In your AWS console- go to your VPC dashboard ⇢create VPC ⇢VPC subnets & more⇢give your VPC a name ⇢IPV4 CIDR Block `10.0.0.0/16` ⇢2 availability zones⇢ 2 public subnets ⇢0 private subnets ⇢NAT Gateways none and VPC endpoint none⇢create VPC.

![1_l7v_n1tgemiyv1enokdc_a.webp](/assets/img/clouds/tempo/1_l7v_n1tgemiyv1enokdc_a.webp)

![1_bxbvymlexczrbwdnfaoj8g.webp](/assets/img/clouds/tempo/1_bxbvymlexczrbwdnfaoj8g.webp)

### Step 2: Create Launch Template

- In your EC2 dashboard under the Instances tab select Launch Templates ⇢ create launch template. Name your template and add a description ⇢ check the box that says “Provide guidance to help me…” ⇢ choose your AMI, I am using AWS Linux ⇢ for instance type select t2.micro ⇢ select or create existing key pair.
- Now we need to configure our security group rules. You will create one rule for SSH and one rule for HTTP, both with 0.0.0.0/0 as the source. This is for demonstration purposes only! 🚨Never allow access to all IP addresses in a real world scenario.🚨

![1_vjhswsyeor2c2ywmsq228g.webp](/assets/img/clouds/tempo/1_vjhswsyeor2c2ywmsq228g.webp)

- Under advanced network configuration, enable auto-assign public IP. Then scroll down to the user data box and you will enter your bootstrap script here.

```shell
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

From the success page, you are able to click the “create auto scaling group” link.

![1_zxhxwb8ubulr6wuopnhbgg.webp](/assets/img/clouds/tempo/1_zxhxwb8ubulr6wuopnhbgg.webp)

### Step 3: Create Auto Scaling Group

- Select your VPC that we created earlier ⇢ select your subnets we also created earlier. Now it is time to attach a new load balancer ⇢ name your load balancer ⇢ select internet facing ⇢ ensure you have your VPC and subnets attached ⇢ listeners and routing will be HTTP 80 ⇢ select your target group name that we just created ⇢ configure group size 2,2,5 ⇢ under scaling policies select target tracking scaling and set to 60 seconds- this will allow another instance to be created if the utilization rate goes above 50% ⇢ **Create Auto Scaling Group**.


![1_pdczn_yvkofkcfoe75228w.webp](/assets/img/clouds/tempo/1_pdczn_yvkofkcfoe75228w.webp)

![1_nzetmmeiowome6a-yegxxg.webp](/assets/img/clouds/tempo/1_nzetmmeiowome6a-yegxxg.webp)

![1_9kif38piiahjftzimddngq.webp](/assets/img/clouds/tempo/1_9kif38piiahjftzimddngq.webp)

![1_5lmwv1slmbqr0doq5xj-kg.webp](/assets/img/clouds/tempo/1_5lmwv1slmbqr0doq5xj-kg.webp)

Double check yourself by grabbing your auto assigned public IP address from your instance to ensure your Apache webpage is there-

![1_drfe15_6-7g86qgdd8x-sw.webp](/assets/img/clouds/tempo/1_drfe15_6-7g86qgdd8x-sw.webp)

Success!


## ☁️Application Tier needs☁️

> 2 private subnets associated with a private route table.
> 
> Minimum of 2 EC2’s in an autoscaling group.
> 
> EC2 security group allowing inbound permissions.
> 
> **NOTE: This is not a true application tier as we don’t have any provided code to run on the EC2 instances. We will be following the same steps as above, just in private.**

### Step 1: Create your private subnets

- In your VPC dashboard, select subnets. You will select create subnet ⇢ select your VPC ID you created eariler in the lesson ⇢ your CIDR block will vary with each subnet and you can utilize a subnet calculator if you need assistance ⇢**create subnet**.

![1_620tzegn1kpauqtowar6nw.webp](/assets/img/clouds/tempo/1_620tzegn1kpauqtowar6nw.webp)

### Step 2: Create Route Table

- Now you want to navigate to the route table tab ⇢ click create route table⇢ enter your table name ⇢ attach your VPC ⇢ click **create route table**.

### Step 3: Create Auto-scaling Group:

- In your EC2 dashboard select autoscaling groups ⇢ create autoscaling groups ⇢ name your group ⇢ **switch to launch configuration**⇢Name your template ⇢ find your EC2 AMI and enter it into the AMI search box⇢ we are using AMI Amazon Linux 2 Kernal 5.10 ⇢ Instance type will be t2.micro ⇢ Create a new security group and give it a name ⇢ Create or use an existing key pair ⇢ hit **create launch configuration**.
- Select your new Ec2 and in the actions tab, select **create auto scaling group**. Give it a name ⇢ attach your VPC and subnets for your App tier ⇢ next ⇢ no load balancer ⇢ then add the same group size and scaling policies that you used in the Web tier.

## ☁️Database Tier needs☁️

> 2 private subnets with a private route table.
>
> Application server security group allowing inbound permissions for SQL.

### Step 1: Create Subnets

- Starting in your AWS console, you want to create 2 private subnets using the steps you used in your previous application tier. Then follow the steps for creating a private routing table and associate the 2 subnets you created for the database tier.

### Step 2: Create Route table

- Then you want to create your route table. Navigate to route tables ⇢ create route table ⇢ name your table ⇢ select your VPC ⇢ **create route table**. Now on the actions drop down, select edit subnet associations ⇢ check the 2 subnets you just created.

![1_r-tnjwqjm3vd1v0gv3ko3a.webp](/assets/img/clouds/tempo/1_r-tnjwqjm3vd1v0gv3ko3a.webp)

![1_izbwrr0gqdsljfjkc4jxcq.webp](/assets/img/clouds/tempo/1_izbwrr0gqdsljfjkc4jxcq.webp)

### Step 3: Create DB Subnet Group

- In your console search for RDS. It will bring you to a list of items and you will click on subnet groups⇢then create DB subnet group. Name your group⇢ select your vpc⇢ choose your availability zones ⇢ then select the 2 subnets you just created ⇢ **hit create**.

![1_hkhjvrdu46lylx7vsiql7w.webp](/assets/img/clouds/tempo/1_hkhjvrdu46lylx7vsiql7w.webp)

### Step 4: Create Database

- Now we will create our database for the database tier. You will go back into the RDS dashboard and select create database. Select **standard create** ⇢ engine type will be mySQL⇢ under templates you will select **FREE TIER** ⇢ in settings, create a password for your admin ⇢ under storage, disable storage autoscaling.

![1_v--dqqzk4ozbbdgl4dlbuw.webp](/assets/img/clouds/tempo/1_v--dqqzk4ozbbdgl4dlbuw.webp)

![1_pq-pacqux3wxpxorrytruw.webp](/assets/img/clouds/tempo/1_pq-pacqux3wxpxorrytruw.webp)

![1_b7zarf8pvjxtawvjm-8ktw.webp](/assets/img/clouds/tempo/1_b7zarf8pvjxtawvjm-8ktw.webp)

### Step 5: Create VPC security group

- Under connectivity, you will want to select your new VPC you created for this project and your subnets for this tier ⇢ create new VPC security group ⇢ name your new VPC security group ⇢ select your availability zone ⇢ hit **create**.

![1_kew43gwkesezksqp-3deaw.webp](/assets/img/clouds/tempo/1_kew43gwkesezksqp-3deaw.webp)

- Now we want to update the connectivity, so select your database you just created. Under the connectivity and security tab you will click the link under VPC security groups. Under inbound rules ⇢ select edit inbound rules⇢ add rule ⇢ delete the default rule ⇢ your port range is 3306 which is the mySQL protocol ⇢ and the source will be your application tier security group. I had a few listed here- so honestly I had to go back and look to see which was the app tier group!

![1_32qgkyl_153b0incgkibba.webp](/assets/img/clouds/tempo/1_32qgkyl_153b0incgkibba.webp)

![1_a5vfv6vldgo-vujvarjh-g.webp](/assets/img/clouds/tempo/1_a5vfv6vldgo-vujvarjh-g.webp)

![1_erdrfk1x_o5_dfibswtyiq.webp](/assets/img/clouds/tempo/1_erdrfk1x_o5_dfibswtyiq.webp)

## ☁️NAT Gateway☁️
You will need a Nat Gateway in one of your public subnets to allow your instances in private subnets to update packages and patches. So lets get into it.

### Step 1: Create

- In your VPC dashboard select NAT gateways⇢ **create NAT gateway**. A name for your NAT gateways is optional ⇢ select the correct subnet ⇢ keep the connectivity type public⇢ click the allocate elastic IP button and this will auto generate an IP ⇢ **hit create**.

![1_5eblpbcekldfpluhx8hrvw.webp](/assets/img/clouds/tempo/1_5eblpbcekldfpluhx8hrvw.webp)

### Step 2: Direct the public route table to the internet

- In your VPC dashboard go to route tables⇢ select your public route table ⇢ click on routes tab and edit routes⇢ add route ⇢ for target select Internet Gateway and select the one you created ⇢ destination will be 0.0.0.0/0⇢ **hit save**.

### Step 3: Direct the private subnets to the NAT gateway

- Select your private route table ⇢ click routes ⇢ edit route ⇢ add route ⇢ the target is NAT Gateway and the NAT gateway you created ⇢ destination is 0.0.0.0/0⇢ **save**.

**☁️You have now successfully created your 3-tier architecture. Make sure you tear down anything that you built in your AWS dashboard for the purpose of the project so that you don’t accrue costs! ☁️**

![1_shhtcocd-u3f2jdths81wa.webp](/assets/img/clouds/tempo/1_shhtcocd-u3f2jdths81wa.webp)



source : https://aws.plainenglish.io/3-tier-architecture-in-aws-e410d610ed74