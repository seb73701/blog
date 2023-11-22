---
title: Why we use Terraform and not Chef, Puppet, Ansible, Pulumi, or CloudFormation
description: 
published: true
date: 2023-07-19T16:35:46.322Z
tags: 
editor: markdown
dateCreated: 2023-07-19T16:35:46.322Z
---

# Why we use Terraform and not Chef, Puppet, Ansible, Pulumi, or CloudFormation

![1_legsc7cu0sh8dyzz0o5n9a.webp](/assets/img/apps/terraform/1_legsc7cu0sh8dyzz0o5n9a.webp)

This is Part 1 of [the Comprehensive Guide to Terraform series](https://blog.gruntwork.io/a-comprehensive-guide-to-terraform-b3d32832baca). In the intro to the series, we discussed [why every company should be using infrastructure-as-code (IAC)](https://blog.gruntwork.io/a-comprehensive-guide-to-terraform-b3d32832baca). In this post, we’re going to discuss why we picked Terraform as our IAC tool of choice.

If you search the Internet for “infrastructure-as-code”, it’s pretty easy to come up with a list of the most popular tools:

- [Chef](https://www.chef.io/)
- [Puppet](https://puppet.com/)
- [Ansible](https://www.ansible.com/)
- [Pulumi](https://www.pulumi.com/)
- [CloudFormation](https://aws.amazon.com/cloudformation/)
- [Terraform](https://www.terraform.io/)
- [Heat](https://wiki.openstack.org/wiki/Heat)

What’s not easy is figuring out which one of these you should use. All of these tools can be used to manage infrastructure as code. All of them are open source, backed by large communities of contributors, and work with many different cloud providers (with the notable exception of CloudFormation, which is closed source and AWS-only). All of them offer enterprise support. All of them are well documented, both in terms of official documentation and community resources such as blog posts and StackOverflow questions. So how do you decide?

What makes this even harder is that most of the comparisons you find online between these tools do little more than list the general properties of each tool and make it sound like you could be equally successful with any of them. And while that’s technically true, it’s not helpful. It’s a bit like telling a programming newbie that you could be equally successful building a website with PHP, C, or Assembly — a statement that’s technically true, but one that omits a huge amount of information that would be incredibly useful in making a good decision.

In this post, we’re going to dive into some very specific reasons for why we picked Terraform over the other IAC tools. As with all technology decisions, it’s a question of trade-offs and priorities, and while your particular priorities may be different than ours, we hope that sharing our thought process will help you make your own decision. Here are the main trade-offs we considered:

- [Configuration Management vs Provisioning](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#3f44)
- [Mutable Infrastructure vs Immutable Infrastructure](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#b264)
- [Procedural vs Declarative](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#f26f)
- [General-purpose language versus domain-specific language](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#9e94)
- [Master vs Masterless](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#c7ba)
- [Agent vs Agentless](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#64a6)
- [Paid versus free offering](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#2818)
- [Large Community vs Small Community](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#5176)
- [Mature vs Cutting Edge](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#d105)
- [Using Multiple Tools Together](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#1641)

## Configuration Management vs Provisioning
Chef, Puppet, and Ansible are all configuration management tools, which means they are designed to install and manage software on existing servers. CloudFormation, Heat, Pulumi, and Terraform are provisioning tools, which means they are designed to provision the servers themselves (as well as the rest of your infrastructure, like load balancers, databases, networking configuration, etc), leaving the job of configuring those servers to other tools. Although the distinction is not entirely clear cut, given that configuration management tools can typically do some degree of provisioning (e.g., you can deploy a server with Ansible) and that provisioning tools can typically do some degree of configuration (e.g., you can run configuration scripts on each server you provision with Terraform), you typically want to pick the tool that’s the best fit for your use case.

In particular, we’ve found that if you use [Docker](https://www.docker.com/) or [Packer](https://www.packer.io/), the vast majority of your configuration management needs are already taken care of. With Docker and Packer, you can create images (such as containers or virtual machine images) that have all the software your server needs already installed and configured. Once you have such an image, all you need is a server to run it. And if all you need to do is provision a bunch of servers, then a provisioning tool like Terraform is typically going to be a better fit than a configuration management tool (here’s an example of [how to use Terraform to deploy Docker on AWS)](https://github.com/brikis98/infrastructure-as-code-talk).

That said, if you’re not using server templating tools, a good alternative is to use a configuration management and provisioning tool together. For example, a popular combination is to use Terraform to provision your servers and Ansible to configure each one.

## Mutable Infrastructure vs Immutable Infrastructure
Configuration management tools such as Chef, Puppet, and Ansible typically default to a mutable infrastructure paradigm. For example, if you instruct Chef to install a new version of OpenSSL, it will run the software update on your existing servers, and the changes will happen in place. Over time, as you apply more and more updates, each server builds up a unique history of changes. As a result, each server becomes slightly different than all the others, leading to subtle configuration bugs that are difficult to diagnose and reproduce (configuration drift). Even with automated tests, these bugs are difficult to catch; a configuration management change might work just fine on a test server, but that same change might behave differently on a production server because the production server has accumulated months of changes that aren’t reflected in the test environment.

If you’re using a provisioning tool such as Terraform to deploy machine images created by Docker or Packer, most “changes” are actually deployments of a completely new server. For example, to deploy a new version of OpenSSL, you would use Packer to create a new image with the new version of OpenSSL, deploy that image across a set of new servers, and then terminate the old servers. Because every deployment uses immutable images on fresh servers, this approach reduces the likelihood of configuration drift bugs, makes it easier to know exactly what software is running on each server, and allows you to easily deploy any previous version of the software (any previous image) at any time. It also makes your automated testing more effective, because an immutable image that passes your tests in the test environment is likely to behave exactly the same way in the production environment.

Of course, it’s possible to force configuration management tools to do immutable deployments, too, but it’s not the idiomatic approach for those tools, whereas it’s a natural way to use provisioning tools. It’s also worth mentioning that the immutable approach has downsides of its own. For example, rebuilding an image from a server template and redeploying all your servers for a trivial change can take a long time. Moreover, immutability lasts only until you actually run the image. After a server is up and running, it will begin making changes on the hard drive and experiencing some degree of configuration drift (although this is mitigated if you deploy frequently).

## Procedural vs Declarative
Chef and Ansible encourage a procedural style where you write code that specifies, step-by-step, how to to achieve some desired end state. Terraform, CloudFormation, Pulumi, Heat, and Puppet all encourage a more declarative style where you write code that specifies your desired end state, and the IAC tool itself is responsible for figuring out how to achieve that state.

To demonstrate the difference, let’s go through an example. Imagine that you want to deploy 10 servers (EC2 Instances in AWS lingo) to run an AMI with ID `ami-0fb653ca2d3203ac1` (Ubuntu 20.04). Here is a simplified example of an Ansible template that does this using a procedural approach:

```
- ec2:
    count: 10
    image: ami-0fb653ca2d3203ac1
    instance_type: t2.micro
```

And here is a simplified example of a Terraform template that does the same thing using a declarative approach:

```
resource "aws_instance" "example" {
  count         = 10
  ami           = "ami-0fb653ca2d3203ac1"
  instance_type = "t2.micro"
}
```

On the surface, these two approaches might look similar, and when you initially execute them with Ansible or Terraform, they will produce similar results. The interesting thing is what happens when you want to make a change.

For example, imagine traffic has gone up, and you want to increase the number of servers to 15. With Ansible, the procedural code you wrote earlier is no longer useful; if you just updated the number of servers to 15 and reran that code, it would deploy 15 new servers, giving you 25 total! So instead, you need to be aware of what is already deployed and write a totally new procedural script to add the five new servers:

```
- ec2:
    count: 5
    image: ami-0fb653ca2d3203ac1
    instance_type: t2.micro
```

With declarative code, because all you do is declare the end state that you want and Terraform figures out how to get to that end state, Terraform will also be aware of any state it created in the past. Therefore, to deploy five more servers, all you need to do is go back to the same Terraform configuration and update the count from 10 to 15:

```
resource "aws_instance" "example" {
  count         = 15
  ami           = "ami-0fb653ca2d3203ac1"
  instance_type = "t2.micro"
}
```

If you applied this configuration, Terraform would realize it had already created 10 servers and therefore all it needs to do is create five new servers. In fact, before applying this configuration, you can use Terraform’s plan command to preview what changes it would make:

```
$ terraform plan
# aws_instance.example[10] will be created
    + resource "aws_instance" "example" {
        + ami            = "ami-0fb653ca2d3203ac1"
        + instance_type  = "t2.micro"
        + (...)
      }
# aws_instance.example[11] will be created
    + resource "aws_instance" "example" {
        + ami            = "ami-0fb653ca2d3203ac1"
        + instance_type  = "t2.micro"
        + (...)
      }
# aws_instance.example[12] will be created
    + resource "aws_instance" "example" {
        + ami            = "ami-0fb653ca2d3203ac1"
        + instance_type  = "t2.micro"
        + (...)
      }
# aws_instance.example[13] will be created
    + resource "aws_instance" "example" {
        + ami            = "ami-0fb653ca2d3203ac1"
        + instance_type  = "t2.micro"
        + (...) 
      }
# aws_instance.example[14] will be created
    + resource "aws_instance" "example" {
        + ami            = "ami-0fb653ca2d3203ac1"
        + instance_type  = "t2.micro"
        + (...)
      }
    
Plan: 5 to add, 0 to change, 0 to destroy.
```

Now what happens when you want to deploy a different version of the app, such as AMI ID `ami-02bcbb802e03574ba`? With the procedural approach, both of your previous Ansible templates are again not useful, so you need to write yet another template to track down the 10 servers you deployed previously (or was it 15 now?) and carefully update each one to the new version. With the declarative approach of Terraform, you go back to the exact same configuration file again and simply change the `ami` parameter to `ami-02bcbb802e03574ba`:

```
resource "aws_instance" "example" {
  count         = 15
  ami           = "ami-02bcbb802e03574ba"
  instance_type = "t2.micro"
}
```

Obviously, these examples are simplified. Ansible does allow you to use tags to search for existing EC2 Instances before deploying new ones (e.g., using the `instance_tags` and `count_tag` parameters), but having to manually figure out this sort of logic for every single resource you manage with Ansible, based on each resource’s past history, can be surprisingly complicated: for example, you may have to manually configure your code to look up existing Instances not only by tag but also by image version, Availability Zone, and other parameters. This highlights two major problems with procedural IaC tools:

- **Procedural code does not fully capture the state of the infrastructure**. Reading through the three preceding Ansible templates is not enough to know what’s deployed. You’d also need to know the order in which those templates were applied. Had you applied them in a different order, you might have ended up with different infrastructure, and that’s not something you can see in the codebase itself. In other words, to reason about an Ansible or Chef codebase, you need to know the full history of every change that has ever happened.
- **Procedural code limits reusability**. The reusability of procedural code is inherently limited because you must manually take into account the current state of the infrastructure. Because that state is constantly changing, code you used a week ago might no longer be usable because it was designed to modify a state of your infrastructure that no longer exists. As a result, procedural codebases tend to grow large and complicated over time.

With Terraform’s declarative approach, the code always represents the latest state of your infrastructure. At a glance, you can determine what’s currently deployed and how it’s configured, without having to worry about history or timing. This also makes it easy to create reusable code, since you don’t need to manually account for the current state of the world. Instead, you just focus on describing your desired state, and Terraform figures out how to get from one state to the other automatically. As a result, Terraform codebases tend to stay small and easy to understand.

## General-purpose language versus domain-specific language
Chef and Pulumi allow you to use a general-purpose programming language (GPL) to manage infrastructure as code: Chef supports Ruby; Pulumi supports a wide variety of GPLs, including JavaScript, TypeScript, Python, Go, C#, Java, and others. Terraform, Puppet, Ansible, CloudFormation, and OpenStack Heat each use a domain-specific language (DSL) to manage infrastructure as code: Terraform uses HCL; Puppet uses Puppet Language; Ansible, CloudFormation, and OpenStack Heat use YAML (CloudFormation also supports JSON).

The distinction between GPLs and DSLs is not entirely clear-cut — it’s more of a helpful mental model than a clean, separate categorization — but the basic idea is that DSLs are designed for use in one specific domain, whereas GPLs can be used across a broad range of domains. For example, the HCL code you write for Terraform works only with Terraform and is limited solely to the functionality supported by Terraform, such as deploying infrastructure. This is in contrast to using a GPL such as JavaScript with Pulumi, where the code you write can not only manage infrastructure using Pulumi libraries but also perform almost any other programming task you wish, such as run a web app (in fact, Pulumi offers an Automation API you can use to embed Pulumi within your application code), perform complicated control logic (loops, conditionals, and abstraction are all easier to do in a GPL than a DSL), run various validations and tests, integrate with other tools and APIs, and so on.

DSLs have several advantages over GPLs:

- **Easier to learn**. Since DSLs, by design, deal with just one domain, they tend to be smaller and simpler languages than GPLs and therefore are easier to learn than GPLs. Most developers will be able to learn Terraform faster than, say, Java.
- **Clearer and more concise**. Since DSLs are designed for one specific purpose, with all the keywords in the language built to do that one thing, code written in DSLs tends to be easier to understand and more concise than code written to do the exact same thing but written in a GPL. The code to deploy a single server in AWS is usually going to be shorter and easier to understand in Terraform than in Java.
- **More uniform**. Most DSLs are limited in what they allow you to do. This has some drawbacks, as I’ll mention shortly, but one of the advantages is that code written in DSLs typically uses a uniform, predictable structure, so it’s easier to navigate and understand than code written in GPLs, where every developer might solve the same problem in a completely different way. There’s really only one way to deploy a server in AWS using Terraform; there are hundreds of ways to do the same thing with Java.

GPLs also have several advantages over DSLs:

- **Possibly no need to learn anything new**. Since GPLs are used in many domains, there’s a chance you might not have to learn a new language at all. This is especially true of Pulumi, as it supports several of the most popular languages in the world, including JavaScript, Python, and Java. If you already know Java, you’ll be able to jump into Pulumi faster than if you had to learn HCL to use Terraform.
- **Bigger ecosystem and more mature tooling**. Since GPLs are used in many domains, they have far bigger communities and much more mature tooling than a typical DSL. The number and quality of Integrated Development Environments (IDEs), libraries, patterns, testing tools, and so on for Java vastly exceeds what’s available for Terraform.
- **More power**. GPLs, by design, can be used to do almost any programming task, so they offer much more power and functionality than DSLs. Certain tasks, such as control logic (loops and conditionals), automated testing, code reuse, abstraction, and integration with other tools, are far easier with Java than with Terraform.

## Master Versus Masterless
By default, Chef and Puppet require that you run a master server for storing the state of your infrastructure and distributing updates. Every time you want to update something in your infrastructure, you use a client (e.g., a command-line tool) to issue new commands to the master server, and the master server either pushes the updates out to all of the other servers or those servers pull the latest updates down from the master server on a regular basis.

A master server offers a few advantages. First, it’s a single, central place where you can see and manage the status of your infrastructure. Many configuration management tools even provide a web interface (e.g., the Chef Console, Puppet Enterprise Console) for the master server to make it easier to see what’s going on. Second, some master servers can run continuously in the background and enforce your configuration. That way, if someone makes a manual change on a server, the master server can revert that change to prevent configuration drift.

However, having to run a master server has some serious drawbacks:

- **Extra infrastructure**. You need to deploy an extra server, or even a cluster of extra servers (for high availability and scalability), just to run the master.
- **Maintenance**. You need to maintain, upgrade, back up, monitor, and scale the master server(s).
- **Security**. You need to provide a way for the client to communicate to the master server(s) and a way for the master server(s) to communicate with all the other servers, which typically means opening extra ports and configuring extra authentication systems, all of which increases your surface area to attackers.

Chef and Puppet do have varying levels of support for masterless modes where you run just their agent software on each of your servers, typically on a periodic schedule (e.g., a cron job that runs every five minutes), and use that to pull down the latest updates from version control (rather than from a master server). This significantly reduces the number of moving parts, but, as I discuss in the next section, this still leaves a number of unanswered questions, especially about how to provision the servers and install the agent software on them in the first place.

Ansible, CloudFormation, Heat, Terraform, and Pulumi are all masterless by default. Or, to be more accurate, some of them rely on a master server, but it’s already part of the infrastructure you’re using and not an extra piece that you need to manage. For example, Terraform communicates with cloud providers using the cloud provider’s APIs, so in some sense, the API servers are master servers, except that they don’t require any extra infrastructure or any extra authentication mechanisms (i.e., just use your API keys). Ansible works by connecting directly to each server over SSH, so again, you don’t need to run any extra infrastructure or manage extra authentication mechanisms (i.e., just use your SSH keys).

## Agent Versus Agentless
Chef and Puppet require you to install agent software (e.g., Chef Client, Puppet Agent) on each server that you want to configure. The agent typically runs in the background on each server and is responsible for installing the latest configuration management updates.

This has a few drawbacks:

- **Bootstrapping**. How do you provision your servers and install the agent software on them in the first place? Some configuration management tools kick the can down the road, assuming that some external process will take care of this for them (e.g., you first use Terraform to deploy a bunch of servers with an AMI that has the agent already installed); other configuration management tools have a special bootstrapping process in which you run one-off commands to provision the servers using the cloud provider APIs and install the agent software on those servers over SSH.
- **Maintenance**. You need to update the agent software on a periodic basis, being careful to keep it synchronized with the master server if there is one. You also need to monitor the agent software and restart it if it crashes.
- **Security**. If the agent software pulls down configuration from a master server (or some other server if you’re not using a master), you need to open outbound ports on every server. If the master server pushes configuration to the agent, you need to open inbound ports on every server. In either case, you must figure out how to authenticate the agent to the server to which it’s communicating. All of this increases your surface area to attackers.

Once again, Chef and Puppet do have varying levels of support for agentless modes, but these feel like they were tacked on as an afterthought and don’t support the full feature set of the configuration management tool. That’s why in the wild, the default or idiomatic configuration for Chef and Puppet almost always includes an agent and usually a master, too.

All of these extra moving parts introduce a large number of new failure modes into your infrastructure. Each time you get a bug report at 3 a.m., you’ll need to figure out whether it’s a bug in your application code, or your IaC code, or the configuration management client, or the master server(s), or the way the client communicates with the master server(s), or the way other servers communicate with the master server(s), or…

Ansible, CloudFormation, Heat, Terraform, and Pulumi do not require you to install any extra agents. Or, to be more accurate, some of them require agents, but these are typically already installed as part of the infrastructure you’re using. For example, AWS, Azure, Google Cloud, and all of the other cloud providers take care of installing, managing, and authenticating agent software on each of their physical servers. As a user of Terraform, you don’t need to worry about any of that: you just issue commands, and the cloud provider’s agents execute them for you on all of your servers. With Ansible, your servers need to run the SSH daemon, which is common to run on most servers anyway.

## Paid versus free offering
CloudFormation and OpenStack Heat are completely free: the resources you deploy with those tools may cost money, but you don’t pay anything to use the tools themselves. Terraform, Chef, Puppet, Ansible, and Pulumi are all available in free versions and paid versions: for example, you can use the free and open source version of Terraform by itself, or you could choose to use it with HashiCorp’s paid product, Terraform Cloud. The price points, packaging, and trade-offs with the paid versions are beyond the scope of this blog post. The one question I want to focus on here is whether the free version is so limited that you are effectively forced to use the paid offering for real-world, production use cases.

To be clear, there’s nothing wrong with a company offering a paid service for one of these tools; in fact, if you’re using these tools in production, I strongly recommend looking into the paid services, as many of them are well worth the money. However, you have to realize that those paid services aren’t under your control — they could go out of business, or get acquired (e.g., Chef, Puppet, and Ansible have all gone through acquisitions that had significant impacts on their paid product offerings), or change their pricing model (e.g., Pulumi changed its pricing in 2021, which benefited some users but increased prices by ~10x for others), or change the product, or discontinue the product entirely — so it’s important to know whether the IaC tool you picked would still be usable if, for some reason, you couldn’t use one of these paid services.

In my experience, the free versions of Terraform, Chef, Puppet, and Ansible can all be used successfully for production use cases; the paid services can make these tools even better, but if they weren’t available, you could still get by. Pulumi, on the other hand, is harder to use in production without the paid offering known as Pulumi Service.

A key part of managing infrastructure as code is managing state (you’ll learn about how Terraform manages state in [How to manage Terraform state](https://blog.gruntwork.io/how-to-manage-terraform-state-28f5697e68fa)), and Pulumi, by default, uses Pulumi Service as the backend for state storage. You can switch to other supported backends for state storage, such as Amazon S3, Azure Blob Storage, or Google Cloud Storage, but the Pulumi backend documentation explains that only Pulumi Service supports transactional checkpointing (for fault tolerance and recovery), concurrent state locking (to prevent corrupting your infrastructure state in a team environment), and encrypted state in transit and at rest. In my opinion, without these features, it’s not practical to use Pulumi in any sort of production environment (i.e., with more than one developer), so if you’re going to use Pulumi, you more or less have to pay for Pulumi Service.

## Large Community vs Small Community
Whenever you pick a technology, you are also picking a community. In many cases, the ecosystem around the project can have a bigger impact on your experience than the inherent quality of the technology itself. The community determines how many people contribute to the project; how many plugins, integrations, and extensions are available; how easy it is to find help online (e.g., blog posts, questions on Stack Overflow); and how easy it is to hire someone to help you (e.g., an employee, consultant, or support company).

It’s difficult to do an accurate comparison between communities, but you can spot some trends by searching online. The table below shows a comparison of popular IaC tools, with data I gathered in June 2022, including whether the IaC tool is open source or closed source, what cloud providers it supports, the total number of contributors and stars on GitHub, how many open source libraries are available for the tool, and the number of questions listed for that tool on Stack Overflow (note: the data on contributors and stars comes from the open source repositories for each tool, but as CloudFormation is closed source, this information is not available).

![1_diztfpumydqehtkj4nloua.webp](/assets/img/apps/terraform/1_diztfpumydqehtkj4nloua.webp)

Obviously, this is not a perfect apples-to-apples comparison. For example, some of the tools have more than one repository: e.g., Terraform split the provider code (i.e., the code specific to AWS, Google Cloud, Azure, etc.) out into separate repos in 2017, so the preceding table significantly understates activity; some tools offer alternatives to Stack Overflow for questions; and so on.

That said, a few trends are obvious. First, all of the IaC tools in this comparison are open source and work with many cloud providers, except for CloudFormation, which is closed source and works only with AWS. Second, Ansible and Terraform seem to be the clear leads in terms of popularity.

Another interesting trend to note is how these numbers have changed since the first edition of Terraform: Up & Running. The table below shows the percentage change in each of the numbers from the values I gathered in the first edition of Terraform: Up & Running, back in September 2016. (Note: Pulumi is not included in this table, as it wasn’t part of this comparison in the first edition of the book.)

![1_x12qfj3arvf1-kgr9ly4yg.webp](/assets/img/apps/terraform/1_x12qfj3arvf1-kgr9ly4yg.webp)

Again, the data here is not perfect, but it’s good enough to spot a clear trend: Terraform and Ansible are experiencing explosive growth. The increase in the number of contributors, stars, open source libraries, and Stack Overflow posts is through the roof. Both of these tools have large, active communities today, and judging by these trends, it’s likely that they will become even larger in the future.

## Mature Versus Cutting Edge
Another key factor to consider when picking any technology is maturity. Is this a technology that has been around for years, where all the usage patterns, best practices, problems, and failure modes are well understood? Or is this a new technology where you’ll have to learn all those hard lessons from scratch? The table below shows the initial release dates, current version numbers (as of June 2022), and my own subjective perception of the maturity of each of the IaC tools.

![1_3pcurdepmemgmh-exbdksg.webp](/assets/img/apps/terraform/1_3pcurdepmemgmh-exbdksg.webp)

Again, this is not an apples-to-apples comparison: age alone does not determine maturity — neither does a high version number (different tools have different versioning schemes). Still, some trends are clear. Pulumi is the youngest IaC tool in this comparison and, arguably, the least mature: this becomes apparent when you search for documentation, best practices, community modules, etc. Terraform is a bit more mature these days: the tooling has improved, the best practices are better understood, there are far more learning resources available (including this blog post series!), and now that it has reached the 1.0.0 milestone, it is a considerably more stable and reliable tool than when the first and second editions of Terraform: Up & Running came out. Chef and Puppet are the oldest and arguably most mature tools on this list.

## Using Multiple Tools Together
Although I’ve been comparing IaC tools throughout this blog post, the reality is that you will likely need to use multiple tools to build your infrastructure. Each of the tools you’ve seen has strengths and weaknesses, so it’s your job to pick the right tools for the job.

The following sections show three common combinations I’ve seen work well at a number of companies.

1. Provisioning plus configuration management
1. Provisioning plus server templating
1. Provisioning plus server templating plus orchestration

## Provisioning plus configuration management
Example: Terraform and Ansible. You use Terraform to deploy all the underlying infrastructure, including the network topology (i.e., virtual private clouds, subnets, route tables), data stores (e.g., MySQL, Redis), load balancers, and servers. You then use Ansible to deploy your apps on top of those servers.

![1_u7yomwlsziupgvojn8drwa.webp](/assets/img/apps/terraform/1_u7yomwlsziupgvojn8drwa.webp)

This is an easy approach to get started with, because there is no extra infrastructure to run (Terraform and Ansible are both client-only applications), and there are many ways to get Ansible and Terraform to work together (e.g., Terraform adds special tags to your servers, and Ansible uses those tags to find the servers and configure them). The major downside is that using Ansible typically means that you’re writing a lot of procedural code, with mutable servers, so as your codebase, infrastructure, and team grow, maintenance can become more difficult.

## Provisioning plus server templating
Example: Terraform and Packer. You use Packer to package your apps as VM images. You then use Terraform to deploy servers with these VM images and the rest of your infrastructure, including the network topology (i.e., VPCs, subnets, route tables), data stores (e.g., MySQL, Redis), and load balancers.

![1_q9osldi3ooynbpdoieed_g.webp](/assets/img/apps/terraform/1_q9osldi3ooynbpdoieed_g.webp)

This is also an easy approach to get started with, because there is no extra infrastructure to run (Terraform and Packer are both client-only applications), and you’ll get plenty of practice deploying VM images using Terraform later in this blog post series. Moreover, this is an immutable infrastructure approach, which will make maintenance easier. However, there are two major drawbacks. First, VMs can take a long time to build and deploy, which will slow down your iteration speed. Second, as you’ll see in later in this blog post series, the deployment strategies you can implement with Terraform are limited (e.g., you can’t implement blue-green deployment natively in Terraform), so you either end up writing lots of complicated deployment scripts or you turn to orchestration tools, as described next.

## Provisioning plus server templating plus orchestration
Example: Terraform, Packer, Docker, and Kubernetes. You use Packer to create a VM image that has Docker and Kubernetes agents installed. You then use Terraform to deploy a cluster of servers, each of which runs this VM image, and the rest of your infrastructure, including the network topology (i.e., VPCs, subnets, route tables), data stores (e.g., MySQL, Redis), and load balancers. Finally, when the cluster of servers boots up, it forms a Kubernetes cluster that you use to run and manage your Dockerized applications.

![1_wcesc4ylqicebpgazmou1a.webp](/assets/img/apps/terraform/1_wcesc4ylqicebpgazmou1a.webp)

The advantage of this approach is that Docker images build fairly quickly, you can run and test them on your local computer, and you can take advantage of all of the built-in functionality of Kubernetes, including various deployment strategies, auto healing, auto scaling, and so on. The drawback is the added complexity, both in terms of extra infrastructure to run (Kubernetes clusters are difficult and expensive to deploy and operate, though most major cloud providers now provide managed Kubernetes services, which can offload some of this work) and in terms of several extra layers of abstraction (Kubernetes, Docker, Packer) to learn, manage, and debug.

## Conclusion
Putting it all together, the table below shows how the most popular IaC tools stack up. Note that this table shows the default or most common way the various IaC tools are used, though as discussed earlier in this blog post, these IaC tools are flexible enough to be used in other configurations, too (e.g., you can use Chef without a master, you can use Puppet to do immutable infrastructure, etc.).

![1_vvxy8jqmormm3bsyg7ppow.webp](/assets/img/apps/terraform/1_vvxy8jqmormm3bsyg7ppow.webp)

At Gruntwork, what we wanted was an open source, cloud-agnostic provisioning tool with a large community, a mature codebase, and support for immutable infrastructure, a declarative language, a masterless and agentless architecture, and an optional paid service. The table above shows that Terraform, although not perfect, comes the closest to meeting all of our criteria.

If Terraform sounds like something that may fit your criteria too, head over to Part 2: [An Introduction to Terraform](https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180), to learn more.

source : https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c