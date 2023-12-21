---
title: Building Data Platforms I — The ETL bias
description: 
published: true
date: 2023-04-21T10:24:59.204Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:24:55.038Z
---

# Building Data Platforms I — The ETL bias

This is the first article of the Building Data Platforms series. Why doing this in the first place you might be asking? It is simple. If you step back and look at the way the software industry approaches Data you see it has not fundamentally changed if we compare to other shifts that happened in the last 10–15 years. We got new tools such as Kafka, Spark, Flink and Snowflake but we haven’t changed the mindset of how we build Data Platforms. As you will see, this has a lot of consequences that go from lack of productivity, broken systems, knowledge silos, unhappy people, etc.
Let’s start this series with what I consider to be the biggest problem in building Data Platforms — the ETL bias.

## What is ETL?
ETL stands for Extract, Transform and Load and became quite popular in 1970s. During that decade, companies started to have multiple data repositories and wanted to persist relevant information in one place for analysis. ETL became the de facto standard to perform these type of actions.

### Extract
This step is responsible for reading data from a set of Data sources. These usually are relational databases, NoSQL databases, JSON, CVS or XML files, etc. Most of the times, this step requires direct read only access to where the data is located.


### Transform
Translates the source data to match the format of the destination system. Operations in this stage include changing data types, combining or splitting fields, applying more complex formulas to derive new fields.

### Load
This steps takes the Data that was resulted from the Transform stage and persists it into a target data system. It is very common for the target system to be a Data Warehouse or in a file system.


ETLs can be chained together to create pipelines of computations and are usually scheduled to run with a given frequency (e.g. hourly, daily, monthly, etc).


Now that we have a definition of what ETL is, let’s start to understand some of the problems it has.


## The first Data Product
The first Data Product in a company is usually the same and answers the following need
> “We need metrics and KPIs about our main product”.
It is important to mention that we are not talking about Platforms yet but a simple Data Product. The goal of this Data Product is to allow everyone in the company to use Data for analytics.
Most of the times, the first Data Product is done using some sort of ETL like the one below.

![](https://miro.medium.com/max/2400/1*KYnMFi7S5JaeoKlJvd7p4A.jpeg)
*A classic ETL pipeline*

Let’s take a close look of is happening here:
- We have a read only replica of the main operational database
- We fully understand the domain model and schema
- There is a cron job (or a visual drag and drop tool) that runs a set of MongoDB and SQL queries, applies some transformations and loads it into another SQL database (e.g. another Postgres/MySQL instance because people don’t have time to learn a new database technology.)


The domain is small and its knowledge is shared across all engineering the team, the mapping logic from the operational system to the KPIs is quite simple which makes it quite fast to ship. After the ETL pipeline is done the company is able pull data out of its operational system and has a nice set of dashboards to power its business.
The first Data Product is a **massive success** and everyone is happy with it.

![](https://miro.medium.com/max/2400/1*ntrZ3s0n7-8VrqHLyDp3NQ.jpeg)


## Complexity kicks in
There is a point in time where every company starts to break their monolith into smaller components — the so called Microservices. In this journey to microservices, engineering teams usually apply [Domain Driven Design concepts](https://martinfowler.com/tags/domain%20driven%20design.html#:~:text=Domain%2DDriven%20Design%20is%20an,through%20a%20catalog%20of%20patterns.) and API best practices with the goals of encapsulating complexity (via higher order abstractions) while making the system extensible. All of this under the mantras of operational reliability, developer speed and experience with the ultimate goal of building a better product faster.

![](https://miro.medium.com/max/2400/1*aG7VGMEShINnguKhUmY55Q.jpeg)

During the expansion to microservices teams might decide to adapt different database technologies because they make sense under the domain and scope their system operates in. Since systems communicate via APIs there is no problem in having a different database technology.
A couple of months after the company decided to break the monolith engineers are shipping code faster, the product is stable and new features are being built. The quest to break the monolith was a success!
But, what about our Data Product?
Over the last months, the ETL has grown and became more complex. Instead of having to query one database to fetch data, the ETL has to connect to multiple data sources. Besides this obvious increase in complexity, what else has changed?
**Ownership** — maintaining and developing the ETL is now a shared responsibility of the first engineers that built it. These engineers now work on different systems responsible for specific domains. Who is the owner of the ETL?

**Database technology** — Different systems might have different database technologies (e.g. MongoDB, MySQL, Postgres, Cassandra, etc..). This means the ETL logic will have a mix of all of those technologies with different query languages. This increases the complexity of the business logic that the ETL needs to implement.

**Stability** — the main goal of having APIs is to allow teams to change their systems without breaking their clients logic. However, since ETLs (most of the times) connect to read only databases, changes in the schema might break the ETL and cause **data downtime** (I will go deeper on this topic later in this series). Also, building data extraction APIs in the operational systems is not considered a priority because the team needs to focus on building and improving the main product.
There is a point in time where companies realizes they need to have an owner for their Data Product, for their ETL. They might acknowledge it because:

- People are opening issues complaining about missing data
- ETL pipelines are breaking more often and engineers are spending more time fixing them
- Engineers claim the query logic of the data product is too complex and they lack deep knowledge on a specific database
- Building complex data objects require deeper database expertise

The solution is simple
> They need to have a Data Engineering Team

## Rise and fall
The company now has a Data Engineering team with extensive knowledge in some of the databases that are being used. In the beginning, the team might be able to fix some of problems related to performance and stability of the current ETL pipelines. After that happens, there is a positive trend in the organisation. That trend is based on the following facts:
1. There is (at last!) a team that has extensive knowledge of different database technologies
1. There is a single team that knows the nuances of several systems, because they need to know their ontologies
1. The data collection process is working correctly
1. Engineering teams can focus in building features because the Data component is being taken cared of

The Data Engineering team has done what many considered to be impossible

> They have made Data Products great again


Armed with this new positive and bullish vibe towards Data, Product and Engineering teams start planning new features. With these new features come important changes to the product to support new use cases and new metrics to be made available. The backlog of the Data Engineering team starts to become packed with new issues to support new features and other areas in the business such as Customer facing teams. This leads to a set of meetings with engineering teams to discuss changes in Data models and how they impact the current ETL pipelines. With multiple changes to be made, engineers suddenly realize that the complexity of changing data pipeline DAGs is non trivial. Keeping the current Data Products up to date is not as simple as it initially people thought.
Because no one wants to be in the place where they constantly fixing broken data pipelines, the consequences are simple: new features will not be shipped as fast as Product and other business units planned. This leads to frustration, friction and a general feeling that..

> “Data is slowing us down”

The fact is, they are all right.

## Why is Data Broken and slow?
The question about Data being broken and slow is not new. Everyone that works or has worked on Data systems knows this.
I believe the root of the problem is not related to technical experience but with a mindset of looking at Data. Since ETL became a standard, Data teams were responsible of maintaining them and ensuring they worked. Data teams were mostly groups of people with very technical knowledge of databases (the so called DBAs)
Product and Engineering teams did not consider Data as a first class citizen because Data teams existed exactly to “deal with Data”. It did not make sense for a team building features to worry about who needs the data they produce. That has been the standard in the industry for quite some time (I’d say since the 70s). However, let me ask you the following question

> “If Data is the most precious asset in a company, does it make sense to have only one team responsible for it?”

The answer is of course No. However, the way the majority of companies deal with Data is still the same and results from what I call the ETL bias. This means that there is one team (Data Engineering) that must:
- Pull Data from all operational systems
- Know all database schemas in the company
- Build all the data models for reporting

While other engineering teams must:
- Build features for the product
- Not worry about “data stuff” to avoid slowing the product development

In order for companies to be successful and use Data for their advantage they need to change their mindset. In recent years, Agile and DevOps changed the way software is built. If we look back to the reasons behind those two movements they solved the same problem: something was fundamentally slowing down companies. Back then it software and dealing with infrastructure. Today it is data.
In the second part of this series, we will cover how I think R&D teams should approach Data because, as we all know, Data is the most important asset in a XXI century modern Internet company. It is time for it to start being treated as such.

Source : https://medium.com/codex/building-data-platforms-the-etl-bias-d589733ce4cc

---

# Building Data Platforms II — The Age of the plumber is over

In the first part of this series we talked about the ETL bias and the problems it brings to the way Data is perceived and managed in companies. The first chapter ended with the following question that is going to be the motivation for this second part.

> “If Data is the most precious asset in a company, does it make sense to have only one team responsible for it?”

The answer is of course no and if you ask around in your company they probably have the same feeling. However, if you look around you will probably find a Data team swamped in requests for new ETLs, Data Warehouse schema upgrades and even building dashboards. There’s a complete misalignment of what is intuitive to most people and how they act. In order to align this I believe there are 3 main things that must happen:
1. Distributed Data ownership
1. Data Plumbers become Data Engineers
1. The evolution of the Software Engineer (will be covered in the next chapter)

## Distributed Data ownership
The foundational problem of today’s Data landscape is not tooling but a misplaced sense of ownership. The entire purpose of microservices and the application of domain driven design is to have teams working on a given problem, understand it and build functionalities to address it. In the modern world, Data is a vital component that companies use to stay ahead of their competitors. If that is so important, why do we centralize it? Why do we have Data Engineers maintaining ETL pipelines to bring information from domain X into a Data Lake and/or Warehouse to answer questions they don’t nothing about? This puts Data Teams in the middle of teams that known the domain and internal stakeholders that have questions about it. This problem is perfectly described on the great article by ThoughtWorks that introduces the concept of a Data Mesh.

![](https://miro.medium.com/max/2400/1*7L7VjiSgdaRD8PHWlVkFUA.png)
*Example of silos due to centralization — original image from Data Mesh blog post*

The high level solution to this problem is simple: decentralization. One must bring the teams that own the systems that generate data and understand a given domain with the consumers of that Data. This concept is so clear, makes so much sense and has always been there, in front of us, all this time. This is easier said then done because decentralization brings its own challenges such as governance or in more practical terms: how to ensure things have a certain level of consistency?
Although decentralization is crucial to enable companies to use Data more efficiently I believe that, in most companies, you don’t need to have fully embrace such an elegant but also complex change like a Data Mesh. One the challenges I see with the Data Mesh the following:
> How do you bring a Data Mesh to life?
It is very tempting to believe Data Mesh will solve all of our problems. I strongly believe that we must start with changing the mindset of people so they start seeing and thinking about Data in a more decentralized approach. This is the hardest step that has to be done and this requires a big shift in how we, as an industry, think about Data Engineers and Software Engineers. For the first, we need to totally redefine what they do. For the second, we need to understand what they need to start doing.

## Data Plumbers become Data Engineers
Let me ask you to do something simple: open your favorite search engine and type the following: “Data Engineering skills”. You will most likely find results that contain some of following concepts that are presented below

![](https://miro.medium.com/max/2400/1*qK3GqndgbTQDmvrKsSWT1w.png)
**An example of hits you might find when searching for Data Engineering Skills*

What all of these results have in common? They are all part of a mindset where Data Engineers are responsible for writing some sort of ETLs to fetch Data from operational systems into somewhere else. In this mindset, Data Engineers are basically plumbing data around, building pipelines to satisfy a variety set of teams from which they don’t know nothing about the domain. How many Data Teams do plumbing work to support Analytics and also build datasets for AI teams to train their Machine Learning models? Too many and this is one of the main reasons of why “Data is slowing us down”.
In order to start promoting decentralization of Data we need to clarify what Data Engineers should **not be doing**.
> Data Engineers should not build ETL pipelines
This is a strong statement and it is clashes with how a lot of people have seen the role that Data Engineers in the industry. However, if we want to promote decentralization one must destroy this main centralization point. When we assume that this must happen a natural follow up question arises: “*What should a Data Engineer do?*”
Well, as the name implies Data Engineers should build systems to manage Data. But what does exactly does it mean? If they don’t build ETLs anymore, and they are not plumbers, what else is there to do on the Data side to do? They need to build software that answers the following questions.

## Is Data Correct?
This is one of the most important topics due to the importance that Data has in today’s world. Let’s imagine your company has two Data Pipelines, one to feed internal Analytics and other to generate datasets that will be used to train Machine Learning models. Now let’s assume that one of the source systems that has information about any important attribute (e.g. a duration of an action) introduces a bug that causes both pipelines to generate data with errors. Most of the times, the observability metrics of a data pipeline focus on answering the following questions
1. Can I connect to the source system?
1. Are there any exceptions during transformation?
1. Am I able to write to the output destination?

What happens in this case is that, despite all pipelines metrics being green, the system is generating data that is wrong. These type of errors usually take some time to troubleshoot and can have really bad consequences to the company because most of decisions and systems (e.g. specially AI based ones) are based on the quality of the Data.
Therefore, it is crucial that Data Engineers focus on building systems that detect these problems and alert the teams that need to be notified that something is not correct. Tools such as [Cuelang](https://cuelang.org/), [Great Expectactions](https://greatexpectations.io/) or [Pandas Profiling](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/), can provide great support here.

## Is Data up to date?
Having high quality data is not enough for a company these days. Today’s world moves at a very fast pace and that means companies want to get the pulse of how their business is going. The real importance of this is not about latency of processing (batch vs streaming) but freshness of data. Depending on the use case, we might need to have different service levels for different data items. Let me give you an example to prove why is this important. The margin of a product does not need to be available every time a customer makes a purchase but if that data is evaluated on a bi-weekly basis the team must be aware of the latest entry is 4 or 5 days old. A more advanced way to tackle this problem is have a system that understands patterns of data flows and triggers an alarm every time something wrong happens. This system can be anything, anything from an outlier detection model to a set of hard coded rules.

## Is Data Accessible and Discoverable?
How many times have you asked yourself (or heard someone asking)
> Where can I find information about X and how can I access it?
The common answers to this question are some various of “Go talk with person Y”, “You have these API endpoints”, “It’s on a spreadsheet, I will send it you” (you open it and last edit was 2 quarters ago..). Turns out doing this properly and maintaining it up to date is harder than it looks. Concepts have connections with different levels of relevance and they might exist in more than one system. This brings the aspect of a very important thing that companies do not give the right attention: metadata. Metadata is data about data. Lets take an article here on Medium for instance. On its metadata one would see that it is written by user with ID I, takes around Y time to read, was published at day D, it is related to topic A.B and C and has links to other articles or tools. Without reading the article and only by looking at its metadata you have a pretty good understanding of it. The only way to work with data at scale is by having good metadata.
Data Engineers should build (or maintain) systems that manage metadata to make the lives of data consumers easier. Those consumers can be people in the Finance Team, a group of a Data Scientists, a Product Manager or a Software Engineer that just joined the company. All of them should be able to navigate through the company’s metadata to find definitions of concepts and how they relate to each other. [Amundsen](https://www.amundsen.io/amundsen/) is a great example of a tool that tries to solve this problem.

## Is Data Secure?
Nowadays, Data security has become crucial to every company. Customers and overall audience is more aware of how companies handle their data. If we want to promote decentralization of Data we must do so with some rules and processes that are adopted by everyone. Data Engineers should work in collaboration with Product Managers, Software Engineers and Security Engineers to:
1. Ensure everyone has access to the data they need to do their job (no more and no less)
1. Ensure the company has access logs for all data items
1. Ensure all systems have proper data retention policies
1. Control and centralize the management of sensitive data (e.g PII, customer contracts, etc)

![](https://miro.medium.com/max/2400/1*6qE66bKSMAXMG3uVrwsdTg.jpeg)
*The 4 main things Data Engineers should be focusing one*

## Next steps
The last part of the three aspects that need to happen to promote decentralization of Data is the evolution of the Software Engineer. Originally, I wanted to put this section on this article but I decided to leave this topic to the next part of this series because this part is already long. In the next chapter, I will cover how do Software Engineers need to evolve and give some examples of how one can work with Data in a decentralized and more efficient way.

Source : https://medium.com/geekculture/building-data-platforms-ii-the-age-of-the-plumber-is-over-11488420a39