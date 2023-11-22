---
title: DevOps - 01 - Example project with Terraform
description: 
published: true
date: 2023-07-16T09:29:17.739Z
tags: 
editor: markdown
dateCreated: 2023-07-16T09:12:41.115Z
---

# DevOps - 01 - Example project with Terraform

## Overview
- This lab is part of a series of DevOps labs featuring full sample projects.
- All you have to do is follow step by step to complete it.
- This lab can cost some money but not much if you keep it running for a short time.

## What you’ll learn in this lab
- Use Terraform to set up infrastructure on AWS.
- The infrastructure includes VPC, NAT gateway, security group, Auto Scaling group, EC2, ALB, Elastic IP, and Certificate Manager (SSL/TLS certificates).
- Note that this is a **3-Tier** VPC network architecture. We rely on the Terraform module “**terraform-aws-modules/vpc/aws**” to create most of the network components here.

## Architecture diagram
- Here’s what you’ll create in this lab!

![1_prtlidjiplrh5pjmrxddzg.webp](/assets/img/apps/terraform/1_prtlidjiplrh5pjmrxddzg.webp)

*DevOps01 - Lab architecture diagram*

## Set up tools
This is for macOS, but you may find the guide for Windows or Linux in these URLs.

- Install **Terraform**
https://learn.hashicorp.com/tutorials/terraform/install-cli

```
$ brew tap hashicorp/tap
$ brew install hashicorp/tap/terraform
$ terraform -version
```

- Install **awscli**
https://formulae.brew.sh/formula/awscli

```
$ brew install awscli
$ aws --version
```

- Install **aws-vault**
https://github.com/99designs/aws-vault

```
$ brew install --cask aws-vault
```

- Install Visual Studio Code:
https://code.visualstudio.com/download

## Prepare AWS account and user
- You need an AWS account and an IAM user (console and programmatic access). If you don’t have an AWS account, sign up for it, but make sure you have an international payment card.
- For lab environments, an IAM user with the managed policy “AdministratorAccess” is fine. For better security, you should also apply the “ForceMFA” policy. You can skip this extra setting for now.
- You can get the content of the ForceMFA policy here:
https://gist.github.com/cybersecmas/ba9d96e879a614ab2d753347b7c293cc

![1_7n9lqygdfgrzpnebleda6q.webp](/assets/img/apps/terraform/1_7n9lqygdfgrzpnebleda6q.webp)
*IAM user’s attached policies*

- Use **aws-vault** to add (store) your IAM user’s access key. You may name the profile “terraform.test”.

```
$ aws-vault add terraform.test
$ aws-vault list
$ aws-vault exec terraform.test -d 24h
```

- If you enable MFA, check this:
https://github.com/99designs/aws-vault#roles-and-mfa

```
# https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html
# .aws/config

[default]
region=us-west-2
# output=json

[profile terraform.test]
region=us-east-1
mfa_serial=arn:aws:iam::xxxxxxxxxxxx:mfa/terraform.test
```

*Sample awscli config file*

## Register a domain
- This lab requires a domain to set up HTTPS! Better to have some kind of test domain used for lab purposes.
- The domain can be registered from any domain registrar or right in the same AWS account depending on your needs. Owning a domain name will typically cost between $10 and $20 per year.
- You also need to create a Route53 hosted zone for the above domain in the same AWS account and get the NS record information from there.

![1_plw4bvxwg7khesu1qftzwa.webp](/assets/img/apps/terraform/1_plw4bvxwg7khesu1qftzwa.webp)
*Route53 hosted zones*

- Then go to your domain settings and update its nameservers to route traffic to your AWS account’s Route53.

![1_xxxchcevecdjw1m28uijow.webp](/assets/img/apps/terraform/1_xxxchcevecdjw1m28uijow.webp)
*Set the nameservers*

## Remote state storage with S3 and DynamoDB
- **NOTE**: If you choose not to use remote state for a simpler approach, you can skip this step, but be sure to comment out the contents of the “t1–02-backend.tf” file later.
- Terraform state (“terraform.tfstate" file) can also be stored remotely. In this lab, you need to create an AWS S3 bucket in advance to store the state file as follows:
  - Bucket name: devops-projects-infra-test
  - Region: US-East (N.Virginia) us-east-1
  - Bucket Versioning: Enable
  - Create folder: dev/devops-01
- Then, create a Dynamo DB Table that is used for state locking:
  - Table Name: dev-devops-01
  - Partition key (Primary Key): LockID (Type as String)

## Check out the source code
- Here is the source code to check out:

```
$ cd somewhere/on/your/pc
$ git clone git@github.com:cybersecmas/devops-labs.git
$ cd 01-terraform-aws-3tier/terraform/
```

- Because you will have a different test domain so you will need to modify some code to fit your environment:

```
# File t11-acm-certificatemanager.tf
    "*.your-domain.com" # change this!
# File t12-route53-dnsregistration.tf
  name    = "devops01.your-domain.com" # change this!
# File t13-05-autoscaling-notifications.tf
  endpoint  = "someone@domain.com" # change this! (to your email)
# File my-canary/nodejs/node_modules/my-canary.js
    const urls = ['https://devops01.your-domain.com']; // change this!
```

- After changing `my-canary.js`, you need to re-run the following command:

```
cd my-canary
zip -r my-canaryv1.zip nodejs
```

- Next, you need to create a key pair named “**terraform-key**” in your AWS account (used to log in to your EC2 instance later):

```
$ cd private-key/
$ ssh-keygen -f terraform-key.pem -y | pbcopy
# Go to EC2 -> Key pairs -> Import key pair
# and follow the guide.
# Remember the key pair's name is "terraform-key".
```

- Congratulations! Now everything should be ready.

## Run terraform commands
- Load the AWS access key by using aws-vault. Terraform needs this to connect AWS.

```
$ aws-vault exec terraform.test -d 24h
Enter MFA code for arn:aws:iam::xxxxxxxxxxxx:mfa/terraform.test:
```

- Confirm the AWS environment variables have been set.

```
$ env | grep -i ^aws
```

- Now it’s time to officially run the terraform commands to create your environment!
- **terraform init**
The first command to run in order to prepare the current working directory. It will install plugins and child modules, initialize the backend, copy a source module, etc.
It is safe to run this command multiple times.

```
$ terraform init
...
Terraform has been successfully initialized!
```

- **terraform validate**
It will verify whether a configuration is syntactically valid and internally consistent, regardless of any provided variables or existing state.
It is safe to run this command automatically, for example as a post-save check in a text editor or as a test step for a re-usable module in a CI system.

```
$ terraform validate
Success! The configuration is valid.
```

- **terraform plan**
It will create an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure.
The plan command alone will not actually carry out the proposed changes, so you can use this command to check whether the proposed changes match what you expected.
- **terraform apply**
It will execute the actions proposed in a Terraform plan.
In the default case, with no saved plan file, terraform apply creates its own plan of action, in the same way that “**terraform plan**” would.

```
$ terraform apply
...
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.
Enter a value: yes
...
Apply complete! Resources: 96 added, 0 changed, 0 destroyed.
Outputs:
...
```

## Explore and confirm AWS resources
- Check your email for the SNS Subscription
- EC2 Instances (1 bastion and 2 web servers)
- Auto Scaling Groups (and Launch template)
- Load Balancers
- Target Groups (and Health checks)
- CloudWatch
ALB Alarm, ASG Alarm, CIS Alarms, Synthetics
- Access the service domain:
https://devops01.your-domain.com/
https://devops01.your-domain.com/app1/
https://devops01.your-domain.com/app1/metadata.html
- Download the remote state file if needed:

```
$ terraform state pull
```

- Log in to the Bastion instance:

```
$ ssh -i private-key/terraform-key.pem ec2-user@BastionHost-Public IPv4
```

## Clean up

- **terraform destroy**
Destroy all remote objects managed by a particular Terraform configuration.

```
$ terraform destroy
Acquiring state lock. This may take a few moments...
...
Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.
Enter a value: yes
...
Releasing state lock. This may take a few moments...
Destroy complete! Resources: 98 destroyed.
```

- Some AWS resources you have to delete manually:
CloudWatch > Log groups
CloudWatch > Alarms
Lambda > Functions
S3 bucket, DynamoDB
- and also the following local files:

```
# Delete Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Conclusion
- Congratulations on completing the lab. It is not easy to understand what it consists of, but by going through all the steps you have learned a lot.
- Finally, if you want to do some more demos, please try the following useful course:
https://www.udemy.com/course/terraform-on-aws-with-sre-iac-devops-real-world-demos/

source : https://medium.com/@cyber-security/devops-01-example-project-with-terraform-ade540824db1