# How to Design Structured Database Systems Using SQL [Full Book]

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755095979245/dfd39c26-3456-4e79-a01c-0b2a82f7a034.png)

Source : https://www.freecodecamp.org/news/how-to-design-structured-database-systems-using-sql-full-book

This book will guide you, step-by-step, through designing a relational database using SQL. SQL is one of the most recognized relational languages for managing and querying data in databases.

You’ll learn the fundamental concepts related to both data and the databases where they are stored and managed – from how data is transformed into information and subsequently into knowledge, to the architecture of a database management system (DBMS). We’ll also cover the different stages of the database design process, as well as its key principles, focusing specifically on the design of relational databases.

By the end of the book, you’ll have a solid understanding of how to design and maintain efficient, secure databases that can support complex data-driven applications, all aimed at meeting a series of requirements imposed by end users or clients. You’ll also learn the SQL fundamentals you’ll need to implement this design on a DBMS, and then maintain and query data on it.

So, whether you're a beginner or looking to enhance your skills, this book will provide the knowledge and tools you need to succeed in the world of data management.

## Table of Contents
1. Prerequisites
2. The Role of Data in Today's Digital World
3. Chapter 1: What is Data?
    - DIKW Pyramid
4. Chapter 2: What is a Database?
    - DMBS (Database Management Systems)
    - ACID Properties and Transactional DBMS
    - Database Management System Architecture
5. Chapter 3: Data Management Models and Technologies
    - Types of Data According to Structure
    - Big Data
    - NoSQL Databases
    - Data Warehousing
    - Data Lakes
    - Semantic Web
6. Chapter 4: Database Design
    - Database Design Levels
7. Chapter 5: Relational Model (Structured Data)
    - Table (Relation)
    - Schema
    - Tuple
    - Attribute Domain
    - Derived attribute
    - Conceptual Representation
    - Repeating Group
    - Data Inconsistency
    - Entity Associations
    - Generalization and Specialization
    - Entity Association Pitfalls
    - Keys
    - Weak Entities
    - Navigability
    - Constraints
    - Data Integrity
    - Integrity Constraints
8. Chapter 6: Relational Schema Diagram
    - 1-1 association
    - 1-M association
    - Minimum cardinality issues
    - N-M association
    - IS-A Hierarchy
9. Chapter 7: Normalization
    - Decomposition
    - Functional dependency
    - Normal forms
    - BCNF
    - Other normal forms
10. Chapter 8: Query Languages
    - Formal vs practical query languages
11. Chapter 9: SQL (Structured Query Language)
    - DDL
    - DCL
    - DML
    - Views
    - Database Administration
12. Chapter 10: Database Design Process Example
    - Entity-relation to logical model
    - How to create the database
13. Chapter 11: Example Queries
    - Tuple filtering
    - Subqueries
    - Common table expressions
    - Set operations
    - Aggregation queries
    - Division queries
    - Ranking queries
14. Conclusion

## Prerequisites:
Before going through this book, there are a few useful prerequisites you should have:

### Fundamentals:
1. Basic programming knowledge like variables, data types (string/number/boolean), and conditionals/loops
2. Familiarity with spreadsheet terms/basic functions (rows, columns, sorting/filtering) as this will help map to tables/tuples/attributes
3. Command-line basics like how to open a terminal, run a command, set PATH (you’ll use CLI tools occasionally here), and so on

### Environment to set up
A relational DBMS like PostgreSQL (recommended, as it’s what we’ll use here)

A SQL client like psql, pgAdmin, TablePlus, or DBeaver (pick one)

An Entity Relationship Diagram tool like draw.io/diagrams.net, Lucidchart, or dbdiagram.io

A code editor like VS Code (with SQL and ERD extensions is fine)

Helpful background
Familiarity with some math/logic basics like sets/subsets, relations, functions as well as basic propositional logic (AND/OR/NOT, implication).

Basic knowledge of data modeling terms (entity, attribute, relationship, cardinality, and so on)

Version control basics

With that sorted, let’s dive in.

The Role of Data in Today's Digital World
These days, every action we take on the internet leaves behind a trail of information or data – whether it's conducting a bank transaction or shopping online.

But you may sometimes wonder whether it actually makes sense for these digital actions to be recorded. Do we need records of keystrokes when using a keyboard app, images saved in a gallery app, files in a file management program, notes saved in a note-taking app, or even vehicle routes with integrated Android Auto technology?

Some of these actions may not seem particularly useful at first, but they help developers and designers provide better, more advanced, and efficient services to users.

For instance, understanding how a user types on a keyboard app can improve the real-time typing experience by adapting the internal dictionary to the user's typing style and correcting errors more effectively. It also improves gesture typing, a feature based on artificial intelligence techniques that requires a large number of examples to be deployed successfully on a product.

Similarly, simple images saved in a gallery may not seem significant enough to be recorded on external sites, or even registered at all.

Image files, for example, can contain EXIF metadata with information about the image, such as the location where it was captured, the date of creation, its resolution, orientation, and the camera model used – among other data. While a user may not be interested in this data, it serves as the foundation for various application services, including classifying images into albums based on location, creating visual timelines, and generating "memories." These features significantly enhance the user experience.

Besides metadata, the content of images also creates a "digital trail" on third-party servers, which might initially seem intrusive and not beneficial to the user. However, it can lead to enhanced services. Since these third parties have the resources to train large machine learning models, they can recognize objects and faces in images. This improves album classification and allows users to search through images using text. Third parties can also identify which people or items are in a photo and link their data with other services.

Regarding the data generated by smart vehicles or IoT devices in general, the purpose is fundamentally the same: to provide users with better services, such as route optimization, maintenance prediction, prevention of possible failures, driving assistance, and integration with other smart devices in the environment.

These features are implemented using artificial intelligence techniques that learn from examples. Typically, the more data available, the better the underlying models "learn," leading to better results.

Ultimately, regardless of the legal and privacy issues related to these practices, recording what we do is not an end in itself. Rather, it’s a means of turning scattered information into useful knowledge, which can then be used to create services that enhance our productivity or user experience.

One clear example of this is in this very article on the Hashnode platform. It provides writers with translation, rewriting, and keyword optimization services for SEO searches, all of which are based on artificial intelligence models that have been trained using large amounts of text – that is, data.

So to make sure this is all technically feasible, we had to develop specific techniques for collecting, storing, and managing information securely, efficiently, and consistently.

Collection involves capturing information from various sources, such as IoT sensors, mobile devices, and social interactions, either manually or automatically.

This information can then be stored and accessed again when we need to transform it or apply processes that improve query efficiency or reduce storage space. This is precisely why data compression is a critical aspect of data storage.

Lastly, data management involves organizational, protective, governance, and analytical tasks.

In this book, we will focus on storage, which is the key aspect handled by databases. But databases are used for more than just storing and accessing data, as we will see later. They also provide a set of functionalities that allow us to organize, protect, and ensure the integrity of data, as well as to query it efficiently and concurrently.

This makes databases a fundamental component of the infrastructure for these services, which are often offered to a large number of users.

More precisely, we will focus on explaining all the necessary theoretical concepts you need to know to design and maintain a database. There are many ways to store data, depending on its nature or the client's needs, but we will focus on one specific structure.

To grasp the fundamentals of storing and managing data, we should begin with the most straightforward cases, which involve the simplest possible data structures. You’ll also learn the SQL language and its relevance to database maintenance through examples.

Chapter 1: What is Data?
Before we start working with databases, it's helpful to have a clear understanding of what data is. More specifically, we need to understand what data means in the context of working with databases and SQL.

The official definition of data covers the most basic level, which states that data is a symbolic representation of a quantitative or qualitative attribute or variable that describes an empirical fact, event, or entity.

It's important to note that data has no inherent meaning. In other words, data is merely a value representing something observable or measurable – it doesn't provide any interpretable meaning.

For instance, the number 27 is data to which we initially can't provide meaning, though we can store, transform, compress, and encrypt it, and so on, if possible. Later, if we discover that this value stems from a variable representing temperatures, then we have more than just data – we have semantics, or meaning.

In this example, the number 27 is considered raw data. Raw data is data that has been collected from a source, yet it lacks meaning or semantics and has not been processed or organized.

In the context of databases, the term variable is occasionally used to denote the origin of the data. But the term attribute is more common, as we will see later. So to sum up, an attribute can be viewed as a variable in programming. It represents a feature of an entity, such as a person's age. It’s characterized by a data type and a domain that define what the values can be and what its possible values are, respectively.

Data types are the internal formats and operations supported by an attribute's data. They can include:

integers (ints), which are typically encoded in computer science as 32-bit sequences

text strings encoded in UTF-8 format

decimal numbers (floats or doubles, among others), represented using the IEEE-754 floating-point standard, and

boolean values that can be true or false and are encoded with bits as 0 or 1.

As you can see, the data type defines how an attribute's values can be, while the domain is a set containing all the acceptable attribute values. A domain consists of a data type that limits the form of the data and a series of constraints that restrict the possible values that can be instantiated within that base data type.

For instance, if an attribute is labeled as an integer and represents a person's age, it's evident that the domain can’t contain negative numbers, despite the int data type allowing them. Consequently, the domain can be defined as all possible integer values with additional constraints ensuring that values less than zero aren’t considered, leaving only the positive integers needed.

Through these concepts, we can understand data in its most basic form. If we take a decimal number like 3.24, it may indicate a measurement for scientific purposes. A text string like "Juan", on the other hand, may represent a person's name. In other words, the semantics of a sequence of characters define its meaning. Alone, the sequence of characters doesn't represent anything – but together, they can represent a Spanish text with a meaning, such as someone's name.

Beyond atomic data, which are the most basic elements that can contain information, there’s also much more complex data out there. This includes document data, spatial and geographic data, network or graph data, and multidimensional data. The only difference between the "atomic" data we saw earlier and these complex forms of data is that the latter are composed of relationships or associations between simpler data.

For instance, a document consists of sequences of characters (strings) related to each other, where one string might represent the title and another a paragraph. In a computer network modeled as a graph, there could be IP addresses at the nodes, which we can think of as encoded strings, and references to other nodes, which are also IP addresses.

We won't delve into the complex nature of such data here because it’s managed by specialized databases that are more difficult to understand, and where SQL is not always present.

DIKW Pyramid
So far, we’ve seen that data itself is just 'symbols' that can be stored, with no inherent meaning unless their origin or interpretation is known.

But it’s also possible to train machine learning models to provide services that appear much more complex compared to the data they were built with. In other words, we can build complex information systems from raw data that contain higher-level knowledge than the data we have discussed.

The DIKW (Data, Information, Knowledge, Wisdom) pyramid models this transformation from data to knowledge, establishing a hierarchy through which we can acquire knowledge about some aspect of reality based on data. To understand this, let’s look at the four levels of knowledge organization.

Data: At this level, our knowledge of the world – or rather, what we know about it – is represented as raw data. As previously mentioned, raw data is devoid of semantics. The only options here are to store and analyze the data. Although they don't explicitly provide high-level knowledge, we can clean the data to avoid missing or corrupt values and calculate statistical measures.
Example: As before, a raw value, such as the integer 27, is data from which we can only calculate certain statistical metrics. We can’t interpret it because we don't know its meaning until we get more context.

Information: After advancing from the previous level, the raw data is provided with semantics, which offers meaning to the stored and analyzed values. Now, the data is better organized because it’s contextualized with respect to its semantics.

This is the primary feature of this level, though certain relationships between the data also allow for more complex statistics to be calculated and more valuable questions to be answered about the data. The knowledge at this level is more abstract and valuable than the previous one.

Example: Continuing with the previous example, the number 27 could represent a person's age. So, here we can interpret and organize it with deeper comprehension and analyze it more precisely.

Knowledge: At this point, knowledge resides in models that capture the patterns of the analyzed and organized data according to their semantics. That is, data follow hidden patterns that aren’t easily discernible, but can be revealed through advanced statistical techniques or machine learning.

So, at this level, information is compressed and summarized, or rather, an understanding of it’s generated through a model, allowing it to be synthesized.

This level is higher than the previous one because it extracts even more abstract knowledge from the information. Such knowledge describes the data itself, serves to make predictions, and achieves certain outcomes by leveraging the higher-level relationships between the data.

Example: Once we have meaningful data, we can build models to describe or summarize it in order to make predictions about unseen data or draw conclusions.

For example, we can use a statistical metric, such as the mean, as a model to determine the average age of a given dataset. Later, by comparing this mean with the ages of other people, we can determine whether they are above or below the average. But the models used to describe data at this stage are usually more complex and practical.

Wisdom: Building on the knowledge from the previous level, we reach a point where it’s no longer possible to extract higher-level relationships from the data. This means that no further abstraction is possible. The only remaining task is to combine our description of the data from the previous level with a social and ethical context, along with the professional experience of people who intend to use this knowledge to guide strategic decision-making and evaluate its consequences over time.
Example: At this final level, we can use a person's age, for which we have models describing them, and combine it with information about the context in which that information was collected to inform strategic decisions.

Note that the data may emerge from an organization, which is the context in which strategic decisions are made. The key point here is that the purpose of having such high-level knowledge is to inform strategic decisions.

By studying this hierarchy, we can see that the interpretation of raw data leads to the acquisition of knowledge, which lets us make informed decisions. Databases help in this process primarily by storing data, which is one of their main objectives. But they also assist us with the analysis process by adapting the data's storage and organization methods.

At this point, you might ask the question: How do we want the database to store and analyze this data? First, we need to store the data persistently in secondary memory so that it can be retrieved at any time, rather than in volatile memory such as RAM.

On the other hand, analyzing data involves a wide variety of operations, ranging from simple searches and filtering to complex aggregations, pattern detection, statistical calculations, executing elaborate SQL queries, and processing text or images. Each type of data and operational need requires different algorithms and data structures for efficiency.

This means that since a database must provide functionalities at the storage and analysis layers, you might wonder whether a "general" database system exists that is capable of storing and analyzing data of any kind, regardless of its complexity or user needs. As we will see below, such a general system can’t exist. But there are systems built to handle any type of data-related problem, as long as the data is in a specific “shape”.

Chapter 2: What is a Database?
Once you learn about the main functions a database needs to provide, you can understand its advantages and why it exists – especially when compared to trying to implement these functions without a database. To help illustrate this, we’ll start by analyzing a case where we try to solve a problem involving data without a database. This will show the problems that can arise and how they are resolved.

Storing Data Without a Database
In terms of data storage, the raw data could be stored directly in binary files in secondary memory. For analysis, we can implement a software "layer" which we can label "processing layer,". It contains programs that manipulate the stored data by accessing it and performing transformations based on implemented logic. And to facilitate data manipulation by users, there can be a graphical interface component that simplifies the use of these programs.

A practical example will illustrate this better. Suppose we are working with a domain that contains data about people and their financial information. Our objective is to analyze this data and make economic predictions. This data may originate from government sources, surveys, or other information systems. So we’ll need to store it in our system as binary files.

But we’re faced with a couple problems: first, we need to choose the optimal file type. Then we need to choose the best way to represent the data in the file to minimize problems in future stages when designing programs for access and analysis.

For example, storing the data in a sequential file, where the data is stored contiguously, is different from storing it in an indexed file, where the information is organized by an index. In other words, there’s an index that organizes the data by name, so all people whose names (or a similar characteristic) begin with the same letter are stored contiguously in the same block, separated from the remaining letters. This recursive principle continues for the subsequent letters of the name. It's as if the data were sorted alphabetically, though generally, a single level of recursion is enough.

Let’s look at an example: In a sequential file, people's names are stored in a "disorganized" way, which requires us to search through the entire file to retrieve a specific person's record. In contrast, an indexed file sorts people alphabetically by name. By consulting the index, we can determine where names beginning with a certain letter start, thus avoiding the need to look through the entire file. In other words, the index is similar to the table of contents in a book, which tells us on which page each chapter begins.

This type of decision affects how efficient searches and queries on data are, as well as its processing. Each file type has its own advantages and disadvantages, as you might expect.

Similarly, there is a wide variety of decisions we can consider when designing programs to access and operate on the data. These are directly influenced by the previous decisions. For example, if we change the file type, the software of these programs will most likely need to be reprogrammed. You can think of these programs as Python scripts that automate certain analysis processes.

Also, when we’re implementing these programs, we need to account for details such as concurrent access to data, which is difficult to implement from scratch, as well as other security features, such as data encryption, compression, and detecting erroneous or incomplete data. These features are essential to providing a good analysis service, but they are difficult to program and maintain.

In short, without a database, it’s possible to solve the problem of storing and analyzing data – but implementing all the software is potentially quite complicated, especially if we aim to do so from scratch. If we have the right resources, it may be possible to complete this process and end up with a sufficiently efficient system. But in most cases, using a database is more convenient.

Storing Data Using a Database
One way to simplify these processes is to use a database, which is an organized collection of data that models a domain and provides storage and analytical support for the processes we need to apply to the data. Without a database, data had to be stored in "single files" – but using a database, it’s stored according to a model that defines the type of information and its internal relationships. This is why the definition uses the term "organized."

As for the term "collection," it refers to the idea that a database is a set of data from the same domain. Here, by "domain" we mean the problem we are dealing with, for which we need to store and analyze data. In our example above, the domain would be the "universe" of people and all the tax concepts associated with them – that is, the set of concepts and information from the real environment that may be relevant to solve a problem using those data.

The advantages of a database extend beyond just storage. They also include the normalization of storage and organization, allowing for efficient queries on the stored data. These queries form the basic operations of any analysis process (querying). They’re also the fundamental support for other tasks such as the technical maintenance of the information system, data management, or even features like the system's scalability.

DMBS (Database Management Systems)
Data management involves a series of additional functionalities that are provided by a component on which the vast majority of databases are currently based: the DBMS (Database Management System). As its name suggests, this component is a software element responsible for centrally and efficiently managing the entire life cycle of stored data.

In this context, management refers primarily to the storage, extraction, modification, deletion, and search of data. These are the fundamental operations necessary for a database to be considered operational.

But management also involves additional functionalities that are useful in a database:

Centralization: Storing all the data in one system avoids having information scattered across many files, which may lead to unnecessary duplication of information, such as data references or the data itself. If the information system is not designed and implemented correctly, this can lead to inconsistencies and errors. But this is not a concern if we use a database.

Data integrity and security: The management system controls who can access the data through access controls and permissions for different database users. It also ensures data integrity, a topic we will discuss later.

Concurrent access and sharing: Information systems typically support applications used by many users simultaneously, which causes synchronization issues handled automatically by the DBMS. Fortunately, this means we don't have to implement specific logic in our database to ensure concurrent access to data by many users.

Finally, another feature of DBMSs is that they streamline the development and maintenance of information systems built using databases, especially those that rely on a DBMS. There are many different DBMS software programs, such as MySQL, MariaDB, PostgreSQL, MongoDB, and Neo4j, among others. Here, we will focus on PostgreSQL.

ACID Properties and Transactional DBMS
Beyond the basic operations we’ve discussed, it's important to highlight the significance of transactional support in modern DBMSs for applications such as banking, online invoicing, and healthcare.

In these areas, it’s usually essential that any modification or query of the data follow a transaction mechanism. In other words, the operations performed on the database must be composed of a block of low-level instructions (reads and writes), and the manager must ensure that these operations are executed as a whole or not at all. This is often called an atomic operation.

This helps prevent technical failures from causing inconsistencies in databases (or similar problems). For example, if a user sends money via internet and an error occurs, the entire transaction is canceled, as if it had never occurred. This protects the database from remaining in an inconsistent state, such as when one party has sent money, but the other has not received it. So the DBMS is responsible for ensuring this atomicity in database operations, which requires it to fulfill the ACID properties. They are:

1. Atomicity: A transaction operates under the "all or nothing" principle, meaning that either all of its low-level instructions are completed, or none of them are executed.

Example: A bank transaction must be completed fully, not left in an intermediate state where one party sends the money and the other does not receive it.

2. Consistency: Every transaction updates the database, ensuring it remains in a valid state and preserves data integrity.

Example: If a transaction changes a person's age, the final age can’t be negative.

3. Isolation: Concurrent transactions should not interfere with each other in a way that produces inconsistent results.

Example: Two people try to book the last seat on a flight at the same time. Isolation ensures that only one booking succeeds and the seat isn't double-booked.

4. Durability: Once a transaction has been completed, its effects are permanent. Even if the system fails, it must be ensured that the changes remain by writing them to persistent storage.

Example: If you transfer money between bank accounts and the system crashes right after, the transfer should still be reflected when the system comes back online.

Finally, it’s important to understand that not all database management systems (DBMSs) need to be transactional, although many of them support such functionalities.

Database Management System Architecture
After seeing what a DBMS is at a high level, we can examine how its functionalities are implemented in greater detail. We won’t look at the lowest possible level, but rather at the architectural level.

To better understand how a DBMS operates, we can focus on each of its component's roles when receiving a user request, whether it's a data modification, management operation, or data retrieval query.



Overall, each DBMS is unique, with components specific to its design and needs. Broadly speaking, though, they all share the following components:

Precompiler: This component extracts and separates individual language statements embedded in applications based on the user query, which is usually in a language like SQL, before handing them off to the parser.

Parser: Processes and validates the syntax of the user query, generating an intermediate parse tree.

Authorization Control: Verifies the user's permissions to ensure that only authorized actions are performed.

Query Processor: Converts the user query into a logical execution plan before optimizing it.

Integrity Checker: Validates that the data meets all the constraints defined on the database while the query executes its statements.

Optimizer: Analyzes and rewrites the execution plan to choose the most efficient execution strategy.

Executable Code Generation: Transforms the optimized execution plan into specific calls to the storage engine API.

Transaction Manager: Coordinates the start, commit, or rollback of transaction operations to ensure atomicity and isolation.

Log (Transaction Record): Sequentially records all modifications to ensure durability and recovery support.

Recovery Manager: Uses the log to restore the database to a consistent state after failures.

Dictionary Manager (Catalog): Maintains and queries the metadata (schemas, statistics, permissions) of the database.

Data Manager: Implements the physical storage data structures and the operations for accessing data.

I/O Processor: Manages reading and writing of data to disk, that is, persistent memory.

Result Generator: Formats and sends the result sets (queried data) to the user or the application layer.

Finally, although most databases rely on a DBMS, this is not always the case. For technical or performance reasons, implementing a custom database from scratch may work better for some teams than using a common DBMS-based solution. So a DBMS is not necessary for a database to exist, though it’s present in the vast majority of databases because of its inherent benefits.

Chapter 3: Data Management Models and Technologies
As you’ve been learning, applications that rely on databases typically involve large amounts of diverse, complex data. Because of this, there’s no single database model that effectively addresses all scenarios. Rather, there are different families, each specializing in specific tasks or sets of tasks.

So here, we’ll explore a range of options that can help you select a database to use in a project, depending on the data and the system's requirements. More specifically, we’ll examine some models or approaches on which a database may be based. But keep in mind that there are many others apart from the ones we’ll discuss here.

Types of Data According to Structure
First, the most relevant factor in determining a database's paradigm in a project is the data itself – particularly its complexity. Data complexity is defined by its structure, variability, and internal relationships. This mainly determines how the data is stored and processed.

So, before analyzing the different paradigms or approaches available, you should understand the meaning of data complexity.

Complexity is a concept that we can informally understand as the degree to which data is "complicated." For instance, a list of integers is different from a graph with integers at each node or a list of numbers encoded in binary, encrypted, or compressed.

Thus, complexity has several dimensions.

Volume: Clearly, the more data we have, the harder it will be to manage. It's likely that not all of it will fit on a single machine, resulting in longer processing or query latency times.

Heterogeneity: This alludes to the vast variety of formats, structures, and origins that data can exhibit within a given information ecosystem. Each of these characteristics constitutes a specific type of heterogeneity. This concept is more related to the world of data integration than to databases themselves, because it’s the main problem we face when integrating data into a system, regardless of whether it includes a database.

Example: If we are going to build a database of cities and populate it with data from different sources, it’s likely that the city names will be written slightly differently in each source. This is referred to syntactic heterogeneity.
Structure: In our case, this is the key dimension, as it allows us to classify the data into different categories, each of which is associated with a specific database paradigm. Essentially, the structure of the data refers to the extent to which it adheres to a predefined schema.

For now, we can understand the schema as a formal definition that determines how data is organized, as well as the features of this organization depending on the nature of the data and the database. Later, we will focus on the concept of schema in a structured (relational) database.

So the complexity of the data depends mainly on two dimensions: the flexibility of the schema and the volume or heterogeneity of the data. The more flexible the schema and the greater the volume or heterogeneity of the data, the more complicated it will be to process it, requiring an appropriate database model.

This means that, regarding the structural dimension of complexity, we can categorize data according to how "rigid" it is.

First, we have unstructured data. These are data that do not follow a fixed schema or set of rules for automatic interpretation or labeling without prior processing. They are usually the most complex since they are unstructured and lack metadata, or additional information that describes or organizes them. This category includes images, videos, audio, and all kinds of multimedia, such as spatial data.

Next, we have semi-structured data. Unlike the unstructured data, this one uses tags as metadata to organize it. This allows the data to be clustered around these tags, which makes it easier to interpret, query, and process. But it can also be self-organized using key-value pairs or internal hierarchies.

Essentially, this data contains meta-information that enables its self-organization, though it does not adhere to the strict schema of structured data. For example, we can have data in XML or JSON format where data is presented as key-value pairs, with a key associated with one or more pieces of data. As such, the key-value pair scheme is not rigid enough to perfectly characterize the structure of the data since it does not explicitly limit the amount of data that can be associated with a tag.

Finally, we have structured data. Such data are organized by a strict schema that restricts them to tabular form. In other words, the organization is adapted to a schema and follows a series of rules. Each data point is composed of a sequence of values that it takes on a finite number of attributes, where each of these attributes is univalued.

We can think of the schema as the table header that determines the attributes for which each data point takes on values. In this way, a data point is a tuple or row of the table in which it’s stored.

There is one additional restriction: each attribute can only have one value, meaning that an attribute or cell of the table can’t contain more than one value.

Each of these categories leads to one or more database paradigms adapted to their nature. The easiest to deal with are the structured ones, as their rigidity does not allow for sufficient variation for the analytical techniques used on them to be considered "complex." In contrast, the most difficult to deal with are the unstructured ones, due to their variety and high flexibility.

Limitations of Structured Data
To keep things simple, we’ll focus on structured data and the databases supporting it. These databases are built using the relational model, which we’ll discuss later.

Since structured data is organized in tables, operating on them is simpler since tables have properties that make them easier to traverse and process. For example, knowing that each cell holds only one value allows us to programmatically traverse all the data in the table by traversing all its rows, regardless of the contents of each cell. This way, we avoid exploring an indeterminate number of values per cell, which would make it much less efficient.

This simplicity also allows tables to be implemented using record- or field-oriented data structures. These provide the necessary efficiency for structuring data within the relational model designed for this type of data. This model is a database "paradigm" that, when used with a query language such as SQL, lets us store and process most structured data, which is why it’s so important.

Keep in mind, though, that its status as a "general" paradigm that addresses almost any problem involving structured data introduces certain limitations:

1. Scalability
Most relational or structured database implementations use a monolithic architecture. This means the database runs on a single machine and can only be scaled vertically by allocating more resources to the machine.

Fortunately, distributed implementations use networks of multiple machines to run the database. This approach allows for horizontal scaling by adding more machines to the distributed system, providing greater scalability. Such scalability is critical for products like social networks, ensuring system availability.

2. Schema Flexibility
With such a rigid schema, if we need to store unstructured data (like JSON or image data), this requires transformation or an alternative to structured databases, such as NoSQL databases. We’ll discuss this more later. These databases allow greater flexibility in data schemas and support heterogeneous data.

3. Complex Data Types
In addition to having a flexible schema, the type of data we are dealing with may be complex, making querying insufficient. Operations on structured data are usually designed for simple data that will often be queried. But when storing images, graphs, or other complex entities, we may need to perform complex operations on them.

For example, we could need to perform object detection in images or calculate neighborhood and centrality metrics in graphs. This leads to the development of specific database models (which we’ll cover later) that support these operations and the storage of such data, which is usually kept in BLOBs.

4. Data Volume (Big Data)
As previously mentioned, the data volume has an impact on almost every database model, since storing a large amount of data slows down processes. But Warehousing and Data Lake models can mitigate this effect by leveraging their ability to scale horizontally and accelerate computation to process massive amounts of data faster. This is achieved through techniques like data pipelines or cluster computing (similar to distributed computing).

5. Real-Time Requirements
Finally, databases are expected to have low latency when performing operations, since the speed at which users are served is often determined by the latency of these operations. Also, as the number of users is usually large, the database must support concurrency.

But the persistent storage operations conducted during these processes – plus the mutual exclusion locks that ensure concurrency (and compliance with ACID principles) – slow down data processing. As a result, in-memory database implementations are frequently preferred to mitigate this issue. In addition to saving data in persistent storage, these implementations use RAM memory as a cache to store some of the data and respond to queries more quickly, achieving a close-to-real-time latency.

So despite being the simplest and most effective at modeling everyday problems, structured databases have certain disadvantages. These have led to the development of alternative database models and approaches. Each of these models attempts to address a specific issue with structured databases, providing support for more complex data and more technically challenging requirements.

Big Data
Before examining specific database models, we should consider a problem that affects all of them: the volume of data. When we have a problem with a sufficient amount of data, the term "Big Data" is typically applied. It’s not a model or set of models, but rather a concept referring to massive, complex data sets.

And given how much data is currently produced every day, it’s more and more common to encounter problems where massive volume becomes a limitation.

In a Big Data project, we can divide its lifecycle into several stages.

First, data is captured from multiple sources and integrated into common formats.

Then, it’s cleaned to ensure correct integration and, when necessary, manually annotated or tagged to feed machine learning models.

The data is then stored in scalable infrastructures or directly in databases, ensuring availability and fast access.

These "preprocessing" tasks can account for a significant portion of the work needed before the data is ready for use.

Once processed, we primarily use data to create knowledge models so we can understand the nature of the data. This also lets us generate predictions and informed decisions in professional environments. This process is usually referred to as business intelligence or data-driven decision-making.

These business intelligence processes can also assist with other tasks, such as statistical analysis and visualization. Some of these tasks, including visualization and statistical analysis, are considered part of the big data ecosystem and are fundamental to data processing. They go along with previous tasks like the management of databases and information systems. So it’s essential to correctly define from the start what data is needed for a project, how it will be processed, and what results are expected.

What Constitutes “Big Data”?
It’s worth noting that, for a project to be considered Big Data, there are no strict conditions for determining whether it belongs to this category. Still, there are a number of factors that contribute to this designation:

The first is volume. As we’ve already discussed, the volume of data refers to the amount of data generated and stored within a given project. The more data that’s generated and stored, the more likely the project is to be categorized as Big Data. Still, there is no specific amount that defines this distinction, as it also depends on other factors, including the availability and complexity of the data.

The next is velocity. This is the rate at which data is generated and must be processed. For example, in a project consisting of a social network or an IoT device network, data may be generated at a very high velocity – that is, a large amount of data per unit of time. This data must be processed as quickly as possible. This means that the faster the data is generated, the more likely it is to be considered part of the Big Data ecosystem.

The last main factor is variety, also called data heterogeneity. This means the more heterogeneous the data, the more difficult it is to process. This requires greater computing power, which makes the project more likely to be considered Big Data.

For instance, integrating data from sources that use the same formats is easier than integrating data from those that use different ones.

Heterogeneity is affected not only by the formats, but also by how they are encoded, transmitted, and so on. We also need to consider the level of data structuring because unstructured or unlabeled data likely requires machine learning techniques (such as clustering) to extract information from it.

These are the main factors, although more have been added over time thanks to technological advances in these processes. Among them are:

Veracity: Degree of reliability of the information received in terms of data quality and accuracy, in order to avoid decisions based on incorrect or biased information.

Viability: The degree to which the data can be effectively used in the project, as sometimes their volume or other factors make their processing technically unfeasible.

Visualization: It's the ease with which data can be transformed into understandable dashboards for users, allowing them to explore it intuitively.

Value: The expected value to be obtained from processing the data. Generally, it's economic value, although it doesn't need to be economic – it mainly depends on the application domain.

Viscosity: This is the significance that data have in decision-making. Not the value added by their processing, but the relevance they have when making a decision.

In summary, although volume is one of the key factors determining whether a problem or project is considered Big Data, it’s not the only one. The speed at which data is generated and the heterogeneity of the data require a large amount of computation to process it, which is the primary issue that led to the concept of Big Data.

NoSQL Databases
The first model or database approach we’ll examine is NoSQL. Its name indicates that these databases aren’t only structured, but also that the data can vary in structure.

The main characteristic of this database approach is its flexibility in storing data – it doesn’t force data to adhere to a fixed schema, such as a tabular one. They also focus on offering easy horizontal scalability, which allows the computational capacity of the database to be expanded by increasing the number of machines. This makes them efficient at processing complex, large-volume data and thus supporting Big Data problems.

To understand what they entail in practice, we could consider a use case involving a database for a bicycle rental system, that lets users rent bicycles through a subscription.

To implement this system, we can choose from a wide variety of databases or information systems. For example, in a relational database, the information is organized in tables, whereas NoSQL databases use different types of structures to organize the data. Each structure yields a specific type of NoSQL database.

Without delving into the specifics of the use case, we can see that using a relational database for such a project may pose challenges in the following areas:

Volume: If the system is deployed nationally or on a continental scale, a large number of users will perform transactions in our system, either by using or returning bicycles or by contracting or canceling their subscription to the service. Above all, scaling a relational database has the greatest impact on the system. To manage such a large number of users, the system requires powerful computing capacity to match to needs. This means that the database must be able to scale horizontally to reach optimal capacity. In relational databases, vertical scaling is usually applied, but it becomes costly to add more computing capabilities beyond a certain threshold.

Velocity: The system must respond quickly to user requests, such as displaying available bikes within a certain area or managing subscriptions. If the system uses a relational database, ensuring concurrency is computationally expensive, which causes high latency when many users query or modify the same information simultaneously.

Rigid schema: In a relational database, the schema does not frequently change. So if our system requires regular updates (like updates to bike models, the addition of new bike sensors, or significant modifications to the subscription service, especially the addition of functionalities or new features), these changes will require updating the database schema by adding or removing columns. This process is costly and complicated once the system is in production and its tables contain a large amount of data.

Temporal Analysis: Since structured databases are composed of tables, as we will learn later, if we need to perform a time series analysis or analyze data spanning a long period of time with a large number of records throughout that period, the database's response latency will be high. For example, consider calculating metrics on bike usage over the last 10 years, during which time there may have been a massive number of transactions between users and bicycles. These types of queries are often called analytical queries.

NoSQL databases offer different solutions to these problems, depending on how the data needs to be structured. So for each of these ways of organizing and storing data, there is a certain type of NoSQL database with a series of advantages and disadvantages depending on the nature of the project and the data involved. Let’s look at them now.

Key-Value model
The simplest option is to store all the data in a dictionary of key-value pairs, where each key is a unique identifier that acts as a tag linked to a single value. The type of content of each value depends on how we need to organize the data.

Here, we use the term "dictionary" to refer to the data structure used in languages such as Python and Java, as well as in languages where the dictionary structure is the only method of representing information, such as in JSON. In our use case, if we want to store user information, each user could be represented as a dictionary with the following key-value pairs:

{
  "id": 27,
  "name": "Juan",
  "email": "juan@juan.com",
  "birth": "1984-01-05"
}
As you can see, keys serve as names that identify the value we are storing in a given pair. In this case, the key is the user's name, although we can also save binary content or a Boolean value as the key.

Among this model's characteristics are:

its simplicity, which enables humans to easily understand it

its low latency, which benefits from data structures such as hash tables with very low access times, and

its ease of distribution on several machines, since a dictionary can be seamlessly partitioned by its keys. In practice, Redis is the most common DBMS used for this kind of database.

Document model
In this model, the information management unit is not a key-value pair, but rather a set of them, known as a "document.

The main difference from the previous key-value model is that the values are no longer "opaque." Here, a document holds its information in a nested, hierarchical structure. This means that a value might be a dictionary containing key-value pairs, some of which can also be dictionaries. Thus, a hierarchy is established within the stored information, rather than allowing the values to be of any kind as in the key-value model.

Some characteristics of the document model are its flexible schema and the hierarchical storage of heterogeneous data. For example, in our use case, we can store bike information as follows:

bike1 = {
  "id": 1,
  "model": "model1",
  "status": "available"
}

bike2 = {
  "id": 2,
  "model": "model2",
  "status": "in_use",
  "sensors": {
    "cadence": 85,
    "speed": 24.5
  }
}

bike3 = {
  "id": 3,
  "model": "model3",
  "status": "maintenance",
  "sensors": {
    "gps": {
      "latitude": 40.4168,
      "longitude": -3.7038
    },
    "camera": "front_hd"
  },
  "acquisitionDate": "2024-11-15"
}
Here, you can see that all the dictionaries represent bikes. But some contain more fields than others depending on the information that the specific bike model yields. This prevents the need for several tables to be created for each model or type of bike. You can also see that some fields have a dictionary as a value, which hierarchizes the data. Also, not all fields need to be structured equally since the model allows for some heterogeneity in this regard.

Finally, it’s important to emphasize that, in this model, the documents are self-descriptive, as the names of the keys or tags identify the stored information. MongoDB is one of the main DBMSs for implementing this model.

Column-oriented model
This model is similar to the structured model (the one used in relational databases) where information is stored in tables – but instead of each data point being kept in a row, it’s stored in a column. For example, in our use case, we could have:

Attribute	bike1	bike2	bike3
model	model1	model2	model3
status	available	in_use	maintenance
sensor_cadence	–	85	–
sensor_speed	–	24.5	–
In this type of database, the points are still rows in a table. But the items that the management system considers to compose the table aren’t the rows, but the columns.

In a relational database, a set of rows composes a table, where each row is a data point holding values taken for certain attributes, which are the columns. Similarly, in the column-oriented model, the management system treats a column as a "data point" on which operations are performed.

As illustrated above, a table from the relational model is transposed so that each column becomes a bicycle instead of an attribute. In the column-oriented model, each data point is a column, allowing analytical queries to be executed quickly since all the values of a column are considered a single "data point," which significantly speeds up aggregation operations.

Furthermore, better data compression is generally achieved since all the data in a column is of the same type. Simple horizontal scalability is also possible through techniques such as column sharding. One of the most popular DBMS for this model is Hadoop.

Graph model
Alternatively, there is the graph model, which relies on graphs as fundamental data structures for storing information and relationships between data.

In our use case, for instance, each node can represent entities ranging from people to bicycles, connected by edges representing relationships between them, such as subscriptions or rentals. Both nodes and edges can contain attributes, allowing us to further organize the information.

This model is characterized by its support for analysis and big data projects since problems that tend to be modeled with graphs often involve large volumes of information, such as social networks. Also, graphs as data structures allow for the modeling of complex information and relationships. Neo4j is a popular option here, but there’s a variety of other DBMSs oriented toward specific uses within this model.

Data Warehousing
Apart from the different options offered by the NoSQL model, you may have other needs that require different types of models. NoSQL is currently focused primarily on efficient data storage and querying. It’s especially useful in projects where data generation is the bottleneck – that is, a system that specializes in storing data is needed.

Conversely, other projects, especially those related to organizations, require a system that not only stores data efficiently but also manages the difficulty of extracting strategic information, as data lacks value on its own. The Warehousing model offers support for the centralization, organization, and subsequent transformation of data into knowledge that guides decision-making.

What is a Data Warehouse?
A Data Warehouse is essentially a specialized database for centrally storing large volumes of data from multiple sources. Besides storing all the data in "a single system" in a centralized way, its main purpose involves optimizing analytical queries on the data and generating dashboards or reports from the analysis itself. This is all aimed at supporting the efficient analysis and storage of the data.

By "analytical queries," I mean queries that require information over a certain period of time (or a different dimension) to calculate a metric on the data, such as the average magnitude over a 10-year period.

Returning to the previous example of the bicycle rental system, the Warehousing model provides advantages in terms of efficiency in storing users' bicycles and transactions, such as rentals or subscriptions. It also supports complex analytical queries on the data that contribute to strategic decision-making regarding the system. Such queries aim to predict demand and revenue, detect which parking areas are used more or less frequently, and so on.

Main Features of Data Warehouses
Now let’s look at some of the main features of a Data Warehouse so you undertstand how they work.

They’re Integrated
A data warehouse is typically a database that stores information from various sources. It integrates this information using transformations and processes that address the heterogeneity of the data, adapting it to the warehouse's common schema.

In our example, data can stem from various systems, including GPS positioning of bicycles, parking occupancy sensors, payment and subscription systems, and mobile applications. The warehouse then integrates all of this data, standardizing it into a common format to make its collective analysis easier. Note that these sources can vary greatly in nature, with some being structured and others not.

They Have a Historical Dimension
Over time, the Warehouse accumulates information from different sources to enable analytical queries. In our example, this would correspond to analyzing the data itself, such as examining user and bicycle behavior and usage, analyzing demand or revenue, among other possibilities.

They’re Optimization for Reading
Given the objectives we want to achieve with a warehouse, it’s optimized primarily for queries that only access data without modifying it, which is precisely what analytical queries require.

In our example, it would not be very efficient to implement the entire information system in a warehouse because of the need to optimize write operations. One possible solution would be to use the warehouse only to store data reserved for analysis, while providing the actual service to users with a more suitable system.

In other words, even if we use a different database to implement the bike rental service, we can also have a warehouse into which we periodically insert information that needs to be analyzed.

Different Data Warehousing Schemas
In addition to these characteristics, a data warehouse is primarily a database consisting of tables. So, if the data is highly complex or has too many dimensions, we can organize it into different data models.

1. Star Schema
Here, the data or measurements are mainly stored in a central table called the fact table, which is related to other tables representing possible dimensions for analyzing the data in the fact table. The main feature of this model is that the dimensional tables aren’t usually subdivided into more specific dimensions, as the goal here is to find a simple way to store data to speed up analytical queries as much as possible.

In our example, if you only need to build dashboards for usage, billing, or similar purposes, prioritizing query speed, you could opt for a star schema with a large rentals table containing fields like user, bike, origin/destination station, date, and cost, and surrounding tables for each of those entities that can be considered "dimensions" when analyzing that data.

Show an example of a Star schema, with a central fact table connected to several dimension tables around it. From https://en.wikipedia.org/wiki/Star_schema

2. Snowflake schema
Unlike the star data model, with a snowflake schema each surrounding table can be further subdivided into specific sub-dimensions, meaning smaller tables related to each other. This often saves space and improves data quality by reducing redundancy, as there are specific tables storing specific information and relating it to the rest of the tables, avoiding the duplication of information in too many tables. This streamlines the management of larger, more complex data sets.

An example of a Snowflake schema, with a central fact table connected to several dimension tables subdivided into more dimensions around it. https://en.wikipedia.org/wiki/Snowflake_schema

ETL (Extraction, Transformation, and Load)
As you’ve now learned, a Data Warehouse is populated with data from multiple sources, all potentially different in nature. So Data Warehouses need to have a component responsible for extracting data from the sources, processing it, and inserting it into the data warehouse. This component is the ETL, which is a specific software piece for each data source that handles:

Extraction: Obtains data from the source in the provided format.

Transformation: It applies a series of transformations to clean them, eliminate heterogeneity, and adapt them to the schema defined in our Warehouse. The complexity and detail of these transformations mainly depend on the problem being addressed, even leading to the derivation or prediction of new data from existing records.

Load: It inserts them into the Warehouse.

ETL processes are typically run periodically to populate the Data Warehouse or update the data within it.

Diagram of an ETL process. It extracts data from sources, transforms it, and loads it into an information system. From https://en.wikipedia.org/wiki/Extract,_transform,_load

OLAP
As you’ve already seen, Data Warehousing is designed to support analytical queries, commonly known as OLAP (Online Analytical Processing). Unlike OLTP (Online Transactional Processing), which focuses on reading or modifying records individually, OLAP allows for analyzing data across various dimensions to discover trends or patterns that support strategic decision-making.

To understand this, it's very common to think about queries on the time dimension, which is the easiest to see, such as calculating an average over data from a time period or any similar metric.

More specifically, in an OLAP environment, data is organized into multidimensional cubes, where each dimension represents a perspective of analysis like time, product, region, and so on, and the data or measures are the quantitative values that are aggregated according to the dimensions we are interested in.

Some basic navigation and aggregation operations are defined on these cubes:

Drill-Down: It involves moving from a high level of aggregation to a more detailed one. For example, after reviewing the total quarterly bike rentals, we apply drill-down to see those that occurred by month, and from there by day or even by parking spot, allowing us to quickly detect usage variations in specific periods.

Roll-Up: This is the opposite operation to drill-down: it groups data into higher levels of detail. Starting from daily rentals, with a roll-up, we can obtain monthly rentals, by region, or the annual total, helping summarize large volumes of data and provide an overall view of the modeled domain.

Slice: Here, a subset of data is selected by setting a value in one dimension. For example, a "slice" in the bike rental cube by setting the dimension "region = Spain" will show all bike rentals that have occurred in Spain, while keeping other dimensions like time or other services (service subscription) fixed.

Dice: Similar to slicing, a "filter" is applied to the cube across multiple dimensions simultaneously. For example, querying bike rentals in a specific geographic region and during a certain time period. The main difference is that a range is defined in several dimensions at once, creating a sub-cube with more specific results.

Pivot: This involves rearranging the dimensions of the cube to change the analysis perspective without altering the data. For example, swapping rows and columns in a report to view regions in columns and periods in rows, making it easier to compare different dimensions and discover correlations between them.

Data Lakes
In addition to the Warehousing model, we have Data Lakes, which are like Warehouses where data is not stored following a common schema but is kept as it stems from its respective sources. That is, to populate a Warehouse with data, ETL components are needed to transform and adapt it to a schema. But with a data lake, such components do not exist because there is no schema that the data must follow – instead, it’s simply stored in its original format and structure.

The main reason for this is that a Data Lake aims to analyze the data, while a Warehouse aims to integrate the data through transformations to turn it into knowledge that supports high-level business decision analysis.

Normally, data is stored in its raw form in a data lake without any processing, although it can be organized according to the project's needs. This implies that the associated costs are generally lower than those of a Warehouse, as it saves all the computation resources related to its transformation, which can sometimes be complex and computationally expensive.

Since Data Lakes focus on storing data rather than integrating it, they are suitable for machine learning tasks and exploratory analysis. It's easy to apply algorithms to find patterns in raw data. But don't confuse non-integrated data with unlabeled data. Labeled data can be stored in a Data Lake and used to train supervised machine learning models. It all depends on the project's needs and the level of abstraction you want to work with.

Semantic Web
In addition to the previous database models, there are other types of technologies and tools that can organize data and its semantics. One of these technologies is the Semantic Web, which arises from the need to provide meaning to the terms used on the traditional web.

For example, in an HTML document, the word "user1" might appear, which by itself is just data without any meaning. So to integrate semantics, the Semantic Web is used as a "layer" of software that associates meaning to the terms that appear on the web.

While a simple HTML document serves to structure a series of data at the layout level, the Semantic Web provides meaning, usually through tags or annotations, so they can be interpreted by both humans and machines. In this way, the data "user1" can be associated with a tag like "name”, indicating that the data is a username.

This technology is based on a series of components:

RDF (Resource Description Framework): A standard where information is represented through Subject – Predicate – Object triples, where the subject is usually a resource or entity within the domain, the predicate is an attribute or relationship that the entity has with a value, which is the object of the triple. This way of representing information is easily understandable by people and easily processed by machines, being independent of the language used to manage the triples (such as XML or Turtle).

  <http://example.org/users/user1> domain:name "Juan"
Vocabularies: A set of terms used to describe data in a specific domain. We can see this as a language or dictionary of concepts with their associated meanings, all belonging to a common domain. More specifically, it can have meanings associated with classes (sets of entities), properties of those entities, or relationships between them.

Example: https://en.wikipedia.org/wiki/Dublin_Core
Ontologies: A formal conceptualization of a domain, where the meanings of the entities within it are defined, along with their properties, relationships with other entities, hierarchies they form among themselves, and their constraints. In summary, they provide richer semantics than vocabularies due to the complexity with which they can model semantics.

Example: http://musicontology.com/docs/getting-started.html
In relation to the web, there are multiple ways we can store our data, whether on our own infrastructure or someone else's. On one hand, we can choose to have a complete infrastructure of our own where all data is handled locally (on-premise), which offers advantages like having full control over it or faster access. But this also has drawbacks such as high costs since we have to maintain the entire infrastructure ourselves, ensure good scalability, and minimize the risk of failures that could reduce service availability.

On the other hand, you can choose to use someone else's infrastructure, usually by renting it. Here, the data is in the cloud, which provides greater scalability, reduced costs since you only pay for the infrastructure you use, broad geographic access with services like GCP or AWS, and backup services that minimize the risk of data loss, which would be potentially very expensive to achieve using local infrastructure.

Still, this approach also has drawbacks, such as the dependency on an internet connection to use the infrastructure as a service, or security and privacy issues since the data is in a place we don't know well.

Finally, keep in mind that these two types of solutions aren’t mutually exclusive. You can use them simultaneously in hybrid solutions where the most sensitive or valuable data is kept locally and the rest on external infrastructure, although this strongly depends on the project's requirements.

Chapter 4: Database Design
Now that you’ve learned about some existing database models and the technologies that support them, it's important to understand what database design means.

In short, database design refers to a database’s creation. When you have a project involving data, the first order of business is to consider is whether you actually need a database. This typically depends on factors like requirements provided by a client.

If you need a database, its design typically follows a series of stages. These stages start with the client's requirements, which determine what needs to be stored and how it needs to be stored. Then, the schema or structure that the data should follow once storage is planned. This allows you to further explore how to store and process the data computationally at a low level to optimize the most critical operations.

For example, in projects like product sales platforms, it may be more important to optimize operations related to product searches, while in others such as social networks, optimizing the writing of new posts may be more significant.

In addition to deciding the structure of the data, user requirements also help determine which data needs to be stored, as it's not always necessary to keep all available data in a database. Generally, only the data that might be retrieved or used in some operation is stored, although this strongly depends on the project's requirements and nature.

Database Design Levels
When you’re developing a data project and working on designing the database, you can divide it into a series of stages or design levels. These are related to the level of abstraction with which you can view the implementation of the database. Think of them as steps to follow to achieve a functional database that meets user requirements which are also considered part of the database design.

Apart from these design levels, there is a distinction based on the area of the development they are oriented towards, usually distinguishing three areas in which the different design levels are classified.

On one hand, there is the analysis of the client's needs and requirements, which determines what our information system must do.

Then we have the design of the database itself, which provides a description of the solution, its practical implementation, and the software/hardware components that form it.

Finally, we have the technology used for this implementation, where the tools, programs, and specific modules involved in the development are decided.

Now let’s look at the different design levels.

1. Analysis (Functional and Data Requirements)
This level is considered part of database design due to its influence on the other stages or levels. Here, information about the domain is first gathered, which can stem from clients, users, or any stakeholder with knowledge about the domain. The main goal is to obtain as much information as possible to then extract user requirements from it. These are a series of axioms that determine what the system must do to function according to the client's needs.

These requirements can be of many types, all studied in depth in the field of software engineering. A significant feature about them is that they determine what the system must do, not how it should do it, although in certain systems there are requirements for correctness or security that might restrict how the system should perform certain actions.

For example, if we design a database for a critical system like a nuclear power plant, it’s very likely that some of those requirements will require the system to respond to certain critical queries within a short time frame for safety reasons.

2. Conceptual Design (High-Level ERD/UML)
Once the requirements that the system or database must meet are clear, the conceptual design is responsible for describing how the data will be organized within the database. This is always done according to the database model you’ve selected for the project, as using NoSQL is different from using a structured database.

To correctly understand this level, let’s consider a case where the database being used is relational/structured. At this level, the data is first described, along with their possible associated constraints, such as data types, attribute domains, and so on. Then, software engineering tools like an entity-relationship diagram are used to describe the tables that comprise the database and their relationships. This helps us formalize the structure in which the data will be organized once the system is in production.

It’s important to remember that regardless of the tool used for this process (whether a diagram or any other representation method), the organization depicted in the diagram must later be translated into a software implementation, which heavily depends on the DBMS. Designing a structured database differs from designing a graph-oriented database, so you’ll need to select an appropriate tool at this level to represent the data organization.

So the main focus at this level, beyond understanding the requirements, is to organize how the information is stored according to the operations the system will support. You’ll also need to properly document the descriptions provided, whether with diagrams or other tools, so they are understandable later and can be implemented on a specific DBMS.

3. Logical Design (Relational Schema)
Assuming the database is structured, at this level, you’ll use the diagram you created in the previous level to implement the database schema on a DBMS. This means you define the tables that the database will have on the DBMS.

If you didn’t use a diagram in the previous level or the database is not structured, you’ll follow the same process – although instead of tables, you’ll use the appropriate structures, such as graphs. Ultimately, here the entity-relationship diagram is translated into a relational schema, as we will see later, which is responsible for representing the tables that exist in the database at the DBMS layer.

When dealing with tables (or the corresponding structure according to the database model you’re using), it’s easy to understand how the database is organizing the information. But this is only the high-level view, in that DBMSs show us how data is organized, since eventually everything has to be converted into low-level data structures and algorithms on files that work with information encoded in binary. In other words, although we see tables, internally the DBMS operates with other types of computational tools at a lower level, closer to the hardware, which do not necessarily have to resemble tables, graphs, key-value pairs, and so on.

This offers an advantage: when managing the database, you can do so by focusing on the tables it contains, without needing to worry about how the data is actually stored in memory (or how the data structures and algorithms used to implement the database operations are working).

In other words, the database, more specifically the DBMS, automatically translates table-level management into the lowest level management, closer to the hardware, which is called logical-physical independence. This allows us to manipulate the database by working directly with the tables, not with the content at the hardware level, which would complicate things.

Finally, at this level, you’ll often perform schema refinement. This refers to restructuring the schema with tables to make certain operations more efficient, or to improve certain aspects of the implementation according to the requirements. We do this because, when translating from the previous level to the logical one, you can modify certain design patterns to better use the tools provided by the DBMS, whether table-oriented or not.

4. Physical Design (Logical Indexes, Clustering, Partitions)
At this level, the DBMS automatically implements the schema we previously defined at the level closest to the hardware. It translates the set of tables and associations we defined into specific data structures like B-trees, indexes, and algorithms that support their operations. In essence, this level is the computational implementation of the DBMS, which manages disk memory or calls the operating system, among other details.

This implementation of our schema by the DBMS is automatic. We simply need to provide a definition based on the relational schema we created earlier, including the tables, associations between them, and the data we want to insert or delete.

With this, the DBMS translates these "relational" operations into low-level operations like assembly instructions. This helps us maintain logical-physical independence, as the DBMS implementation can be modified at any time without affecting our relational schema or its functionality. This lets us optimize the DBMS code without needing to rewrite all the "relational" programs that define the databases.

5. Storage Level (Block Formats, Disk Structures, and Access)
You can think of this level as a subset of the previous one, as it’s responsible for storing data in secondary memory according to the relational schema managed by the DBMS. It performs the necessary requests to the operating system to allocate memory and usually manages information on the disk at the byte level.

For this purpose, it employs low-level techniques that determine how available disk memory will be used, including the implementation of disk structures and the formatting of memory blocks, among others.



6. Implementation of Applications and Security (Views, Permissions, Procedures)
Finally, once the database is built, you can design new layers on top of it where you can install applications and services that facilitate the interaction with the database. That is, you can simplify its operation for the end user, for example by developing a web application in HTML, CSS, and JavaScript to obtain the data in a friendly way, instead of with SQL code.

Some of these layers are also oriented to guarantee the security of the data, establishing higher level access controls than the DBMS where the user must authenticate to access the data. You can also encrypt the data using some of the functionalities of these layers.

Chapter 5: Relational Model (Structured Data)
Now that you understand some of the processes we use to design databases, we will focus on the simplest databases, which are those that operate with structured data. These databases are usually called relational or structured. They are formally designed using the relational model, which is the formalization of the conceptual level used to design this type of database.

The reason relational databases are the simplest lies in the nature of the data they usually store and the constraints imposed on them, as we will see now. We’ll discuss both the conceptual and logical design levels simultaneously, where the fundamental elements of this type of system are mainly represented.

It’s important to differentiate between how these elements are viewed from the conceptual level and from the logical level, as they essentially refer to very similar, and sometimes equivalent, concepts – but formally they are different concepts. In a relational database, the information is structured in entities related to each other and composed of a series of attributes, which is the conceptual view of the model.

Table (Relation)
As mentioned before, structured data is that which follows a rigid schema and is organized in the form of tables. So the fundamental component for storing information in a relational database is the table, which is sometimes also called a relation. This component is part of the logical design, since we define it in the DBMS. So whenever we deal with tables, we are referring to the logical design level.

Here’s an example:

CityID	Name	Country	Population	Area
1	Madrid	Spain	3,223,000	604.3
2	Athens	Greece	664,046	38.96
3	New York	USA	8,398,748	783.8
4	Tokyo	Japan	13,929,286	2,191.1
5	Paris	France	2,140,526	105.4
Schema
The example City table above stores data about different cities. The table has a schema, which is a series of reserved data to describe the structure of the table. That is, the schema consists of the table name, which in this case is City, along with the name and type of all the attributes it has, corresponding to the columns.

For example, if we are storing cities in this table, the Name column corresponds to the Name attribute of each city, which is a property that city entities have, in addition to the associations between entities, which in certain contexts are also called properties. This attribute must have a type, such as string in this case, to determine what kind of data it will contain.

So the table name along with the names and types of the attributes form the schema of a table, which is mainly determined by user requirements. But it’s the database designers who decide how to model the domain entities, what attributes are necessary to include, and the types of each one.

Tuple
In addition to a schema, a table also has an instance, which is the set of tuples it contains at a given moment in time. Here, by tuple, we mean a row of the table, as we can mathematically view it as a tuple (value1, value2, value3…) where all the values for a certain city are present for all the table's attributes.

A peculiarity of the instance is that there can never be multiple identical tuples. This means, in this case, that there can’t be two or more cities that have the same values for all attributes at the same time. This restriction is imposed in the pure relational model, although we will see in practice that this restriction may not be followed to facilitate certain tasks.

This is the case because, in the pure relational model, the instance is considered a set of tuples, and mathematically, a set can’t have repeated elements. But in the practical implementation we will see, the instance is formally modeled with a multiset that does allow duplicates, as each tuple is internally associated with a value indicating how many times it’s repeated in the multiset.

Attribute Domain
Previously, we mentioned that each attribute has a domain, which allows the DBMS to determine how the data in that column will be stored. But we might have an attribute like Population where it doesn't make sense to store negative numbers, similar to Area.

To prevent these situations, the domain of the attribute can be restricted. For example, if we set Population to have only the INTEGER data type by default, it can take any value from the set/domain of integers. But if we only want it to take positive values, we need to add a constraint (which we’ll discuss later) so that the possible values for that attribute, meaning its domain, are only all positive integers.

Derived attribute
A special case of attributes is derived attributes. Their value is not stored, but is rather calculated from the value of other attributes.

Continuing with the example of the City table, suppose we have an attribute Density that should indicate the population density of a city. In this case, we can define it as a derived attribute, instead of calculating the values beforehand and inserting them into the database. Thus, every time Density is queried, the operation Population/Area will be performed, returning the value to the user in the corresponding tuple.

We can see a clearer example of this if we have an attribute BirthDate and we want to calculate the value of another attribute like Age. Here, we can calculate the attribute Age directly from BirthDate as if it were a "view" on that attribute. That is, we can see a birth date as if it were an age, from which we can derive the value of the attribute Age. We’ll discuss the concept of a view later in more detail at the implementation level.

Before moving on to the representation at the conceptual design level of a table, it's important to understand why a table is sometimes called a relation. A relation is a subset of the Cartesian product of the domains that the attributes have, but you can understand it more simply as a set of tuples that comply with a defined schema. For example:

Letter	Number
A	1
A	2
B	1
B	2
In this table, we can assume that the attributes Letter and Number have the domains {A, B} and {1, 2} respectively, so the entire set of possible tuples we can form with these domains are the tuples shown in the table itself.

These tuples come from the Cartesian product of both domains. So if we had larger domains, we would get a much broader cartesian product. A subset of its tuples is called a relation, and we can associate it as the instance of a table, which is why the term relation is sometimes used to refer to what is actually a table.

Conceptual Representation
Putting this aside, it's not as important to focus on formal details like the name relation, but rather to understand the structure of a table and how data is stored in it. So far, everything we've seen about tables refers to the logical design level, which is where we actually work with tables. But at the conceptual level, there is an element very similar to a table called an entity.

According to the conceptual level, a relational database is a set of entities, where each one can be likened to a table. Each entity has a series of attributes, each with a domain, where instead of attribute, it’s usually called a property at the conceptual level.

Entity City represented in an entity-relationship diagram. Image by author. 

Following the example of the City table from before, at the conceptual level, there is an entity called City, shown above in a UML (Unified Modeling Language) entity-relationship diagram, which is the most common way to formally represent this type of information.

Sometimes we can use Crow's foot notation for the diagram, but here we’ll use the same notation as a class diagram in software engineering for simplicity. It’s equivalent, which is why entities are sometimes called classes.

To correctly understand what an entity is, think of it as if it were the schema of the table, or rather a class in object oriented programming that serves as a template to instantiate tuples. Just keep in mind that at the conceptual level they aren’t called tuples but rather instances or occurrences of an entity.

Intuitively, we can see it as if the attributes represented in the entity were the actual values of the first row of the equivalent table – that is, its schema. In this way, if we have a schema (that is, a template), we can create instances of that entity/schema/template simply by assigning values to those attributes. So when we assign values to the properties of an entity, we have an entity occurrence, which at the logical level we can see as a tuple.

For example, the entity City can be "instantiated" in a "tuple" like [5, Paris, France, 2140526, 105.4]. But at the conceptual level we should call it an occurrence instead of a tuple, since “instance” might cause confusion with the concept of instance we discussed earlier at the logical level.

Entity: [CityID,Name,Country,Population,Area]    --->    Tuple=Occurence: [5,Paris,France,2140526,105.4]
So every time we see a box with a name and properties in an entity-relationship diagram, it refers to an entity that is logically equivalent to a table.

Regarding the concept of an instance we saw earlier, here it’s called an entity set, and it contains all the existing occurrences of that entity at a given point in time. In the diagram, we only see the template, not the set with the occurrences of that entity (think of the tuples of a table). In other words, the diagram at the conceptual level is used to see how the database is structured, not to see its specific instances or occurrences, which are more related to the logical level.

Regarding notation, in the entity-relationship diagram, the entity is represented in a box where all its properties (attributes) are listed by name and type. Here, the type does not have to match exactly the type offered by the DBMS in the logical design, as a translation from the conceptual to the logical level is done later, as we saw before.

To the left of each attribute, a - is usually placed to indicate that it’s a private attribute. But this concept is not relevant in this database context, as it comes from the uses given in software engineering to the class diagram notation we use here.

Lastly, attribute names are usually all in lowercase, although according to the style guide you follow, this can vary – like here, where we allow uppercase to minimize changes to attribute names when translating to logical design.

Repeating Group
Once you know what an entity, or table, is, and that the database is a set of them related to each other, you’ll need to consider an important restriction about the table itself as a storage structure.

CityID	Name	Country	Temperature
5	Paris	France	7,44,20,90,1
For example, if we have a City table similar to the one above where we only want to record the temperatures of the city at different points in time, the first option we might consider is to store all the temperatures that each city has or has had in a single Temperature attribute, all together.

But this is not allowed in structured databases for efficiency reasons (as well as formally, which you’ll see later). Specifically, this situation is known as a repeating group, and it occurs when we have to store an indeterminate number of values in an attribute.

For example, if we only need to store a maximum of 5 temperatures that a city can have, we could make the data type of Temperature an array of integers with a length of 5, which would be filled as we get temperature measurements. But if we don't know how many temperatures we will measure, we can’t set an upper limit on the size of the value we are going to store, so we can’t define a specific size for the length of the data type of that attribute. This creates a repeating group.

Anyway, even if we could set a size for data structures like an array, they are usually not allowed due to the uncertainty of the size the developer might set for that array (also considered a repeating group).

At the same time, this uncertainty is the reason why repeating groups pose a problem for the physical implementation of the database. Since we don't know how much space we’ll need to represent them, we might end up wasting lots of memory trying to manage this uncertainty, as well as fragmenting it, or complicating the implementation logic in an attempt to minimize the impact of this waste and memory fragmentation.

How to avoid a repeating group
One way to solve the problem of repeating groups is to store each temperature measurement in a separate tuple. If all measurements can’t be stored in a single attribute value, then one option is to duplicate the information of the other attributes to create multiple tuples, each storing a specific temperature measurement.

CityID	Name	Country	Temperature
5	Paris	France	7
5	Paris	France	44
5	Paris	France	20
5	Paris	France	90
5	Paris	France	1
As you can see, we have duplicated information to store each temperature measurement in a tuple, which avoids accumulating them all in a single value of the same tuple. But repeating data creates (unnecessary) redundancy in the database, which is a problem.

Redundancy is not an issue in all situations, as it can sometimes be good for ensuring data availability. But in this case, we can see that it’s it’s completely unnecessary. First, because it greatly increases the space needed to store city data by repeating the city’s information. Also, because having city data repeated so many times means that every time these data need to be modified, you’ll have to make changes to all tuples recording the temperatures, causing operations to take too long. And if the schema is modified to add or remove attributes, all data in their respective columns must be deleted – so if there is a lot of repeated data in them, those operations will also have high latency.

Data Inconsistency
On the other hand, if in the previous example we insert a temperature measurement and for some reason an error occurs during the operation, we might end up in a situation like the following:

CityID	Name	Country	Temperature
5	Paris	France	7
5	Paris	China	44
Here you can see that when inserting the measurement with a temperature of 44, an error occurred, and the tuple was recorded with an incorrect Country value. This is not common, but if we choose to solve the repetitive group problem this way, we will be inserting duplicate values more often than necessary, making it more likely for these types of errors to occur.

Having the same information duplicated but with contradictory values indicates that our database has an inconsistency. This happens when the same information is duplicated in various places in the database, and the values are contradictory, such as in this example where we have multiple temperature measurements for what appears to be the same city but with the incorrect country value.

To ensure that it’s an inconsistency, we should look at the key values that uniquely identify each tuple, which we will discuss later. But intuitively, we need to focus on those attribute values that allow us to uniquely identify a tuple. If those values repeat in several tuples and there is some inconsistency in the other attributes, then we have an inconsistency.

On the other hand, if in the last example the Country value of the second tuple were "France," we wouldn't have any inconsistency, even though the temperature values don't match. So it's important to understand that inconsistency mainly depends on the schema's semantics, meaning what each attribute signifies.

Finally, to solve the problem of repetitive groups, you’ll typically need to refine the schema – that is, to transform it. In this specific case, we’ll perform a normalization operation, which we’ll see how to do later. This involves separating a table like the one we had before with duplicated information into several tables:

CityID	Name	Country
5	Paris	France
ReadingID	CityID	Temperature
1	5	7
2	5	44
3	5	20
4	5	90
5	5	1
Now there is a City table very similar to the original, but with the difference that it only stores a record of the existing cities, not the temperatures recorded in them.

Also, there is another table we can call Readings, which contains the temperature measurements for each city. In this table, each tuple contains a measurement and an identifier that determines the city where the measurement was taken, which in this case is CityID.

For example, if the measurement was taken in Paris and that city has a CityID value of 5, then the CityID in the Readings table will be 5 for the measurements of that city. This avoids duplicating all the city information as happened before.

By doing this, we avoid the potential inconsistency problems that arose before, and we also save disk space by not duplicating unnecessary information. More importantly, it prevents the appearance of the repetitive group.

For this, we have had to "complicate" or rather enrich the database schema to some extent, meaning the tables that compose it and the schemas that form it. But the complexity in structured databases doesn’t come from being structured, but from the domain being modeled and its operations. In other words, the relational model of structured data is not complex by itself, as it is simply a model. What truly causes complexity is how we use that model to reflect the domain requirements.

Entity Associations
In the context of conceptual design, a relational database is not only made up of entities (tables), as this only allows us to model the existence of "objects" in the domain. Most of the time, these objects will have associations with each other, meaning they will be related.

So, in conceptual design, we have the concept of entity association, which describes how "objects" are linked to one another. This is essential for reflecting the actual structure of the information.

Entity-relationship diagram showing entities City and Person, where each city can have one or more people living in it. Image by author. 

For example, in a domain, we can have entities like the ones above, City and Person. These model the existence of people and cities in the domain. But besides the existence of the entities themselves, it's possible that they have relationships with each other that we can model in our diagram – such as a person living in a certain city.

In this case, we use an association to allow a person in our system to live in a city, meaning we use an association to model that relationship between both entities.

At first glance, we can see that the association is represented in the entity-relationship diagram as a relationship established between entities – but it's important to remember that entities are "templates" from which occurrences of entities are generated when implementing the system (that is, specific tuples). So when we introduce an association at the conceptual level, we have to view it in terms of the tuples that will later be generated from the related entities.

For example, here the relationship can occur between one occurrence (tuple) of a city and many occurrences of a person, since many people can live in a city. But the reverse may not be true depending on the domain requirements, which may determine that a person (occurrence of the entity Person, or tuple of the table Person) can only live in one city, as we’re assuming in this case.

Association Role
In an entity-relationship diagram, the notation of the association is usually represented with a line connecting two entities, known as a binary association. But there are higher-degree associations (which we won't cover here for simplicity) that relate an arbitrary number of entities in a single association.

A role and a direction are usually added to this line to clarify the semantics of the relationship. The role is a word or phrase written above the association line and denotes the role that an entity has in the represented relationship with respect to the direction defined alongside the role.

For example, in the diagram below we have an association between a person and a city. So in the association, the role given to the person is "lives" in the city with which they are associated, as the direction has been defined from the person to the city with the arrow next to the role. In other words, in this relationship between both entities, the function that the person performs is to "live" in the city with which they are associated.

Entity-relationship diagram showing City and Person, where each city has one or more residents. Image by author.

This role doesn't need to be included in all associations, nor is it necessary to establish a direction. But in some cases, it helps us understand the diagram and the domain, which is the goal of the diagram itself.

Also, the role isn't rigid and can be modeled in many ways. For example, in this case, we can reverse the direction of the association and say that the city has the role of "having residents," which are the people it’s associated with. This would model the existence of people living in the city.

Cardinality
Continuing with the different elements of an association, we have cardinality, which describes how many occurrences (tuples) of one entity can or should be associated with how many occurrences of another entity. We represent this with numbers on both sides of the association line that denote the minimum and maximum cardinality, respectively.

To understand this using the previous example, we know that a person can only live in one city, so a person entity will be associated with at most one city. In turn, we can also assume that every person must live in some city, meaning there are no people living in the woods outside of society. So since every person must be associated with exactly one city, the multiplicity we put on the city entity side is 1…1, which is simply written as 1.

Here, the first 1 is the minimum cardinality, indicating that each person must be associated with at least one city, while the other 1 is the maximum cardinality, indicating that each person can be associated with at most one city. For simplicity in the diagram, the number 1 is usually used to denote both cardinalities at once. Also, when we talk about people and cities here, we are referring to the actual occurrences of the entities, which at a logical level are tuples.

If we look at the other side of the association, we see it has The Role of Data in Today's Digital World (sometimes called multiplicity) 1…*, where 1 is the minimum cardinality, indicating that a city must be related to at least one person. This means that in all the cities within our domain, there must be at least one inhabitant.

On the other hand, the * in the maximum cardinality is a way to denote that there is no specific value that must be given to that cardinality – it can be any amount. This means a city can be associated with an arbitrary number of people, indicating that the cities in our domain can have any number of inhabitants.

Since the asterisk denotes any, unbounded amount, we don't have to worry about it being consistent with the minimum cardinality. That is, even if we set the minimum cardinality to 1, by using an asterisk for the maximum, we are indicating that the maximum can be any number from 1 to infinity. This means that cities will have at least one inhabitant and at most an infinite number.

From the minimum cardinality, we can introduce the concepts of optionality and obligation. For example, before we had minimum cardinalities greater than 0, which indicate that a person must always be associated with a city, or a city must always be associated with at least one person. This means that when occurrences of these entities are created, they must meet the restriction imposed by the minimum cardinality of being associated with some occurrence of the other entity. So at creation, it must be directly associated with the other entity that indicates the association, to respect the minimum cardinality.

To see this at the logical design level, we first need to introduce the tools of that level with which associations are implemented – although for now, we can view it by thinking in the object-oriented paradigm, where if we instantiate a person object, it must have a reference to another city object, and vice versa.

Entity-relationship diagram showing City and Person entities. Image by author. 

Regarding optionality, let's consider another possible case where a city can have an arbitrary number of residents, including being uninhabited, meaning empty, since we have set the minimum cardinality to 0 and the maximum to *. This can also be represented more simply by just using the asterisk, indicating an arbitrary amount including 0.

Now, let's also assume that a person can live in one or two cities, so their corresponding cardinality is modified to 1..2, indicating that a person must be associated with at least one city and at most two cities simultaneously at any point in their life cycle.

This occurs since the entity-relationship diagram is instantaneous, not historical, which means that what we see in the diagram is an instantaneous representation of our domain, not a representation of its life cycle or evolution over time.

So when we see that an association has a multiplicity of 1..2 as in this case, we must think that at any given moment, a person must be associated with at least one city and at most two cities. We shouldn’t think that a person must have been related to at least one city and at most two cities throughout their whole lifetime.

Here we can see that a city may have no residents due to the minimum cardinality of 0 that we have set on the person side, indicating that a city may not be associated with any person. With this, we can model optionality, which refers to allowing an association not to occur. That is, when we create a city from its entity (template), we don't have to associate it with a person, since it can be associated with 0 people at minimum. This means that it's not necessary to add a reference to any person because a city may be abandoned and have no residents.

Entity-relationship diagram showing City and Person entities. Image by author. 

To correctly understand optionality, we can modify the example again so that a person can be associated with either no city or one city, indicating that the person may not live in any city or may live in one. Also, on the other side of the association, we also change the maximum cardinality to 500, indicating that a city can have an arbitrary number of residents between 0 and 500, meaning it can be associated with any number of people from 0 to 500, inclusive. This means having residents is optional.

With this, it should be clear that we can set cardinalities as we want according to the domain and requirements – but we always need to ensure they are correct and make sense. For example, you can’t set a maximum cardinality that is strictly less than the minimum cardinality.

In this case, something peculiar happens: on both sides, we have a minimum cardinality of 0, meaning we have optionality. So when we create new instances of the entities, they don't have to be associated with instances of entities on the other side of the association. We can see this as if the association we modeled is entirely optional.

To conclude, although we can set any number for minimum and maximum cardinalities depending on the modeled domain, the most common ones are 1..1, 1..M, or M..N, where N and M can be arbitrary numbers, including 0 in the case of N..M, as long as they aren’t both 0 at the same time (because in that case, the association could not exist).

Recursive Associations
On the other hand, an association does not necessarily have to relate multiple entities. We can use it to model a relationship between occurrences of the same entity. For example, if we want to model the friendship relationship between people in our domain, we can use a recursive association in the entity Person:

Entity-relationship diagram where Person has a friendship relationship with itself. Image by author. 

First of all, it’s very convenient to establish a role in recursive associations, as it’s the simplest way to represent their semantics so we can easily understand them when looking at the diagram.

But in this case, it’s not as useful to specify the direction of the association since the friendship relationship can be considered symmetric. Here, we have modeled the friendship relationship so that one occurrence of Person can be associated with any number of other occurrences of Person, including none, which indicates that in our domain, a person (occurrence of Person entity) can have an arbitrary number of friends, including 0.

Regarding notation, it makes no difference to use 0..* or *, as they indicate the same thing – but we should always use the shortest and simplest notation to understand.

In summary, a recursive association is simply one where both related entities are the same. In this case, the friendship association necessarily relates people to people, meaning it establishes which people are friends with each other.

Associative Entity
Now that we know what associations are, let’s learn about the concept of an associative entity. In some cases it’s also called a property just like the associations themselves. In the following example, there are cities in a domain that can host from 1 to 500 inhabitants, as long as the implicit restriction of having at least one resident is respected. Also, a person can live in an arbitrary number of cities between 0 and 3.

Entity-relationship diagram showing City and Person entities. Image by author. 

The above conceptual diagram would model this situation. As it stands, we can’t store any information about the person's stay in the city, meaning we can’t save information like the dates they started living in that city or moved to another. If we try to do so, we’ll have several options that lead to certain problems in the database.

On one hand, we could choose to add attributes like StartDate and EndDate to the Person entity to determine the respective dates when a person started living in a city or moved to another. But this wouldn't even work if the multiplicity 0..3 of the city were 1..1, because over the person's lifetime, even though they can live in only one house at a time in the 1..1 case, it's possible that the person moves several times throughout their life. This would require multiple pairs (StartDate, EndDate) to be recorded. So since we need to store multiple pairs of these dates, a repeating group would be generated in the respective properties (attributes), forcing us to refine the schema.

On the other hand, we could store those attributes in the City entity, but we would encounter a very similar problem here. We would have to record multiple pairs of (StartDate, EndDate) values for each person, with the added complexity that a city can have many residents. This would also create a repeating group, along with the issue of associating each (StartDate, EndDate) pair with the correct person.

Entity-relationship diagram where City and Person are linked through Residence. Image by author. 

To address this situation, ideally, we should be able to store these attributes within the association itself. This way, when a person starts living in a city, their association would contain these attributes, and they could record the date the person started living in the city as well as the date they stop. This value (when they stop living in the city) can be left blank or set to "NULL" until they actually leave and the association is no longer valid.

To achieve this, at a conceptual level, associative entities are used. These are entities whose main purpose is to allow our database to store information about the associations between entities.

As you can see, associative classes are "related" to associations between entities, not directly with other entities, and they don't have multiplicity or roles. This is because they exist only when the association between several entities is actually established. For example, when a person starts living in a city, they associate with a city, and this association relates to an occurrence of the associative class where the respective attributes like StartDate and EndDate are stored.

So for each person-city association we have, there will also be an occurrence of the Residence entity with the values of its corresponding properties. Also, keep in mind that this association doesn't exist all the time, as the person may stop living in that city – so the association itself may cease to be valid or, rather, cease to exist conceptually.

But depending on how we translate the relational diagram to the logical design of the database, we might want to record the StartDate and EndDate values that the occurrence of the respective associative entity had.

If we want this, we will need to specify it in the logical model of the database or in the conceptual model with a note in the diagram's margin. This is because, at a conceptual level, there are no specific tools beyond notes to specify these kinds of details, which are more related to the logical design.

Aggregation and Composition
Since a UML entity-relationship diagram is used at the conceptual level, there are modifiers we can use in the associations to give them a particular meaning. But this has no effect at the logical level – meaning the introduction of these modifiers in the conceptual diagram doesn't imply any kind of change at the logical level. They are simply used to clarify the details of the modeled domain.

Entity-relationship diagram where City is made up of instances of Person and each person consists of a single Brain. Image by author. 

On one hand, an association can be of the aggregation type, like between Person and City, where aggregation is denoted by an unfilled diamond and signifies that a city can be composed of people. This means that the entity with the diamond is composed of entities on the other side of the association.

Also, in the specific case where we create and destroy entity occurrences at the same time, the aggregation becomes a composition, denoted by a filled diamond. It then works the same way as aggregation – the only difference being the meaning it conveys.

For example, in the above diagram we have modeled that a person is composed of a single brain. Since a person's brain can’t exist independently of the person, the association is denoted as a composition. This is because aggregation would allow the brain to exist independently, which is not possible.

If we look at it inversely, the composition does not prevent the person from existing without being related to a brain, although the 1..1 cardinality we have placed on both sides models this situation, requiring all people to have exactly one brain.

The important thing to understand is that both composition and aggregation are just associations with additional meaning. This means that they don’t influence the logical design of the database itself, much less at the implementation level.

Generalization and Specialization
Another feature of the relational model is that, besides modeling associations between entities as we have seen, it can also model other types of relationships between entities. This can be useful in many situations.

For example, if we have a domain where there are people who can be customers or employees, we can use a generalization and specialization relationship like the following:

Entity-relationship diagram with inheritance where Client and Employee are subclasses of Person. Image by author. 

Generalization-specialization relationships work the same way as in object orientation. We have a class like Person with a set of attributes, allowing for specializations of that class like Client or Employee, where all instances are also people but with more specific attributes.

In the case of Client, it’s a specialized entity derived from the Person entity, so it inherits all the attributes of its parent entity since a client is also a person. In addition to these inherited attributes, it has others specific to being a client. So when an instance of the Client entity is created, think of it as having all the attributes of both Client and Person at the same time. The same happens with Employee but with its respective attributes.

Venn diagram where Client and Employee are subsets of Person, with possible overlap. Image by author. 

If we look at it from a set theory perspective, first we have the entity Person, which gives rise to a set of entities that are people, meaning the occurrences of that entity. Within this entire set, it's possible that, in addition to occurrences of Person, there could be occurrences of Client, since every client is a person. So in the set of people, there will be some who are clients. This also happens with Employee, where in the set of people, there will also be employees, with all of them being people.

Also, nothing prevents a person from being both a client and an employee at the same time, so there will also be elements in the set that are both a client and an employee. But this detail is closer to the logical design of the database than to the conceptual representation of generalization and specialization presented here. In this case, these names indicate that classes like Person are more general than Client, which are their respective specializations.

Entity Association Pitfalls
When we are in the conceptual design stage and create the entity-relationship diagram, it's common to encounter association structures that initially seem correct but, when implemented in a DBMS, lead to ambiguities or unexpected problems that require us to refine the conceptual design. One of these structures is the Fan Trap:

Fan trap example. Image by author.

The Fan Trap appears when we have a "central" class like City that is associated in a "fan" shape with two others, Person and Pool, where each has maximum cardinality on its side. This means a city can be associated with many people and many pools at the same time.

This situation is initially correct, but the problem arises when we want to know which people from a certain city go to which pool. This becomes complicated because if we are given a certain person, we can know their city, as we have defined that a person can only live in one city. But the city can have many pools, so we don't know which specific pool the person goes to. We can only know which pools the city has where they live. Also, the city might have no pools, given the minimum cardinality of 0 on the pool side.

On the other hand, if we are given a pool, we can determine which city it belongs to. Then with that city, we can find out the group of people living there, which we can use to solve the previous question – but in a much more complex way.

To solve this problem, there are many alternatives, although the simplest in this case is to add an explicit association between Person and Pool to model the fact that a person goes to a pool. But if we’re not going to make these types of queries frequently, it might not be worthwhile to complicate the diagram.

Chasm trap example. Image by author. 

There is also the Chasm Trap, which is similar to a Fan Trap but with important differences. For example, in the diagram above, you can see a Chasm Trap. It occurs when we are given a city and asked to find the pools located in it. The only thing we can do is get the group of people living in that city and, from that group, identify some of the pools the city has.

In other words, each pool may or may not have an association with a person, since not all people go to the pool. So, if we try to find all the pools in a city by simply looking at the pools the city's residents go to, we might encounter situations where no resident of the city goes to the pool. Thus, all the pools will take advantage of the 0..30 cardinality on the Person side to not have any associated people, meaning no one goes to those pools.

So if there are pools that no one visits, we won't be able to find them through a group of people. This means that, given a city, we might not know all the pools it has, because if we solve the query this way, we can only be sure of knowing the pools that the city's residents visit. But if there's a pool that no one visits, then that pool won't be accessible through a person. In other words, people won't see those pools, since the 1..* relationship requires them to visit some pool – but it can still happen that no one visits a certain pool.

The solution to this problem is practically the same as for the Fan Trap, although there are many alternatives depending on the domain and requirements. There are also more situations that can lead to these problems or ambiguities which you can read more about here.

Keys
So far, we have talked about entities and associations at the conceptual level, as well as tables at the logical level. Continuing with the logical level, we have not yet introduced any mechanism to uniquely identify the tuples contained in a table. This can be very useful since tuples are data points – that is, occurrences of an entity, like people, cities, and so on.

Uniquely identifying them makes it easier to perform operations or queries on the table. It also allows us to implement associations between entities at the logical level through references between tables.

Keys are sets of attributes used to uniquely identify each tuple in a table. The combination of values in these attributes must be different for every tuple, so that no two tuples are the same.

To understand this concept, let’s start by looking at the different types of keys and their main utility.

Superkeys
Superkeys are sets of attributes that uniquely identify each tuple in a table. They are the most general type of key. As long as the combination of values for those attributes is unique for every tuple, the set of attributes qualifies as a superkey.

Here’s an example:

ID	SSN	Name	Birth	Email
30	74	Alice Johnson	1985-07-12	alice.johnson@example.com
22	59	Bob Smith	1990-03-05	bob.smith@example.org
95	10	Carol Davis	1978-11-23	carol.davis@example.net
21	32	David Brown	2001-01-30	david.brown@example.com
47	61	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
In this case, we have a table called Person where each row stores a person's data. Each person has a government ID number, as well as a Social Security Number (SSN), name, and other details.

A possible superkey would be the attributes {ID, Name}, because among all the people that exist, no two people can have the same name and the same government ID number. But if we choose only {ID} as a superkey and try to uniquely identify all the rows in the table, depending on the data in the rows, we might encounter a situation where two people have exactly the same name, with identical first and last names. In this case, we couldn't uniquely identify both by their name alone.

So by including the ID in the superkey, we can differentiate between the two people/rows, as they can’t have the same government ID. We could also have chosen {ID, SSN} or even {SSN, Name} as a superkey, since the combinations of values in those attributes are very unlikely to repeat among different people. It’s impossible, for example, for multiple people to have the same name and Social Security Number.

Here’s another way to look at this: if we choose {ID, Name} as a superkey, then there can't be multiple rows in the table with the same ID and Name values. In other words, if we choose that superkey, it's because we are sure that this situation won’t occur, ensuring that all rows have a unique combination of values for the ID and Name attributes.

This mainly depends on the domain, as identifying a superkey formally is not simple. It involves knowing all the domains and associated constraints of the attributes in detail, as well as the functional dependencies between them (which we’ll discuss later).

In summary, although you can identify a superkey by formal methods, we won’t go into detail about them here. They’re usually not simple, as they combine techniques like closure or backtracking, which aren't useful to explain for correctly understanding the concept of a superkey. So for now, it's enough to focus on the semantics of each attribute and stick to those attributes that we know can't be repeated in multiple rows, like identifying codes of entities, names, or specific properties they might have, and so on.

Lastly, regarding the above table, we have seen some of the possible superkeys that can exist. But if we want to find all of them, we’ll first assume that the attributes with repeated values in several tuples are Name, Birth, and Email, since multiple people can have the same name, email, or birth date. Considering that ID and SSN do not repeat because they are government identifiers, we would have the following sets as superkeys, ordered by their size or cardinality:

Cardinality 1: {ID}, {SSN}

Cardinality 2: {ID, SSN}, {ID, Name}, {ID, Birth}, {ID, Email}, {SSN, Name}, {SSN, Birth}, {SSN, Email}

Cardinality 3: {ID, SSN, Name}, {ID, SSN, Birth}, {ID, SSN, Email}, {ID, Name, Birth}, {ID, Name, Email}, {ID, Birth, Email}, {SSN, Name, Birth}, {SSN, Name, Email}, {SSN, Birth, Email}

Cardinality 4: {ID, SSN, Name, Birth}, {ID, SSN, Name, Email}, {ID, SSN, Birth, Email}, {ID, Name, Birth, Email}, {SSN, Name, Birth, Email}

Cardinality 5: {ID, SSN, Name, Birth, Email}

Candidate Keys
Next, we have candidate keys. Their main purpose is the same as superkeys, with the only difference being that in this case, they use the minimum number of attributes possible for identification.

For example, before, as a superkey, we could choose {ID, Name}, among other options. But that superkey contains the ID attribute, which represents the government identifier for each person, and we have legal assurance that it is unique for each person.

So, since we know that each person's ID is unique, as is their Social Security Number because it’s also a number related to government procedures, we can reduce the number of attributes needed to uniquely identify each tuple and choose a candidate key like {ID} or {SSN}. We could also consider {Email} as a candidate key, although we assume that several people could have the same email, so we do not count it as a candidate key.

As you can see, conceptually the candidate keys play the same role as superkeys, but here the goal is to achieve identification with fewer attributes, specifically with the minimum number possible. In this example, by considering candidate keys with a single attribute like {ID}, we have managed to uniquely identify tuples with the smallest possible number of attributes, since you can’t form any type of key with fewer than one attribute.

Also, to verify that a key is a candidate and not a superkey, you can check that there is no subset of attributes of the key that by itself forms a key.

For example, if we have a key like {ID, Name} and want to check if it is a candidate key, we just need to check all possible subsets of attributes it has, which are {ID} and {Name} (although there can be subsets with more attributes). And remember that several people can have the same name, but if we look at the subset {ID}, we will see that no person has the same ID as another.

So since there is a subset that can uniquely identify the tuples, it fulfills the fundamental property of any key. This means that the {ID, Name} we were checking is not a superkey, as there is a subset of its attributes that is a key.

If we repeat this process exhaustively, we are guaranteed to find a candidate key, that is, a minimal set of attributes that serves as a key to identify the tuples.

So basically, a candidate key is just a minimal superkey: it uniquely identifies each tuple, and if we remove any column from it, it no longer uniquely identifies tuples.

In practice, we rarely enumerate every superkey or worry about the labels. We just look for a set of attributes that uniquely identifies each tuple, preferably with as few attributes as possible. In design, at the logical level, we could define multiple candidate keys (and, implicitly, many superkeys), but the important step is choosing one candidate key as the primary key to uniquely identify tuples.

Primary Keys
Once we have all the candidate keys that exist (since there can be several depending on the domain and tables we are dealing with), we need to select one of them as the primary key to implement in the DBMS. This way, we can have a key that uniquely identifies the tuples. In other words, a table can have many candidate keys, but these keys are subsets of attributes that we analyze theoretically.

To make them practical and actually identify the tuples in a table, we need to implement one of them in the logical model. Basically, we need to tell the DBMS which of all the candidate keys is the primary one we’ve selected for identification.

With this, we can infer that the name "candidate key" comes from the fact that there can be many minimal subsets of attributes with which we can identify the tuples. But in practice, we only use one of them, which is the one we indicate to the DBMS, that is, the primary key.

In the previous example, from all the superkeys {ID, Name}, {SSN, Name}, {ID, Email}, and so on, we can derive the candidates {ID} and {SSN}, from which we can choose {ID} as the primary. You shouldn’t always make this choice arbitrarily, even though you technically have the option to do so. Rather, you should consider the technical details of the implementation, as well as the semantics of the attributes that form the key to keep it easy to understand, among other factors.

Entity-relationship diagram with the entity Person. Image by author. 

Even though the primary key is selected for use at the logical level, it can also be represented in the entity-relationship diagram at the conceptual level. If it consists of a single attribute, it’s marked with {id} next to its data type. But if the primary key is composite (meaning it’s made up of several attributes where each one is not enough to uniquely identify the tuples, but together with the other marked attributes it is), then all of them are marked with {ID}. As for candidate or superkeys, they aren’t specially marked in the diagram because there can be many.

Alternate Keys
Of all the candidate keys we have, we only choose one as the primary, leaving all the others aside. These keys that aren’t selected as primary are called alternate keys, and their main use is the same as that of a primary key: to uniquely identify the tuples in case the primary key is not accessible or it’s not convenient to use it.

You can also use alternate keys to improve the efficiency of certain operations or queries on the table, as indexes can be defined on them. But we won’t go into detail about this type of optimization technique here.

In our example, if the candidate keys were {ID} and {SSN} and we choose {ID} as the primary, then {SSN} will be the only alternate key we have.

Composite Keys
Another type of key is a composite key, which is defined as a candidate key composed strictly of more than one attribute because each attribute alone is not enough to uniquely identify the tuples in the table.

CityName	Country	Population	Area
Madrid	Spain	3,223,000	604.3
Athens	Greece	664,046	38.96
Nantes	France	320,732	65.19
Tokyo	Japan	13,929,286	2,191.1
Paris	France	2,140,526	105.4
San José	Costa Rica	333,980	44.6
San José	USA	1,013,240	469.7
For example, here we have a City table with information about cities around the world. As you can see, the attributes CityName and Country alone can’t uniquely identify each city, since there are cities in the world that share a country, like Nantes and Paris, and there are also cities with the same name that are located in different countries.

This means that we can’t use any of these attributes separately in a candidate key, as there are multiple cities with the same value in those attributes when viewed individually.

But if we look at them together and consider the composite key {CityName, Country}, we see that no city in our list located in the same country has the same name, so it meets the requirements to be a candidate key. It’s also a superkey, since all candidate keys are superkeys.

This way, we ensure that it’s indeed a composite key, which we can then select as the primary key. This is why sometimes in the definition of a composite key, the term primary key is used instead of candidate key.

Surrogate Keys
So far, we have seen keys formed by choosing a set of attributes from a table that can uniquely identify tuples. But sometimes this may not be possible.

Name	Birth date	Email
Alice Johnson	1985-07-12	alice.johnson@example.com
Bob Smith	1990-03-05	bob.smith@example.org
Carol Davis	1978-11-23	carol.davis@example.net
David Brown	2001-01-30	david.brown@example.com
Emily Wilson	1995-09-14	emily.wilson@example.co.uk
For example, in this Person table, we have the same attributes as before except for ID and SSN, which were the only government identifiers we could use to uniquely distinguish people or tuples in the table.

Now, no matter which subset of attributes we choose, it can’t serve as a key, since we assume there could be multiple people with the same name, born on the exact same date, and using the same email address (this is an assumption here and may not be true depending on the modeled domain).

Since we can’t choose a key with the attributes we have, we need to artificially generate an attribute that can serve as a key. This attribute is known as a surrogate key, and it consists of an attribute that contains sequential numeric values for all the tuples. This means that to ensure each one has a unique value in this attribute, they are numbered from 1 to infinity with integers, guaranteeing the key property.

SurrogateKey	Name	Birth	Email
1	Alice Johnson	1985-07-12	alice.johnson@example.com
2	Bob Smith	1990-03-05	bob.smith@example.org
3	Carol Davis	1978-11-23	carol.davis@example.net
4	David Brown	2001-01-30	david.brown@example.com
5	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
In addition to this auto-incremental approach, where we can see that the surrogate key is an integer value that increases as tuples are inserted into the table, there is also the possibility of the attribute assigning each tuple a UUID (Universally Unique Identifier), which is a 128-bit binary data type usually represented as a string that allow us to assign a unique value to each tuple.

SurrogateKey	Name	Birth	Email
e9e5a22b-d90c-4e5a-8d49-bbc24ff9335e	Alice Johnson	1985-07-12	alice.johnson@example.com
374d6cbe-fc29-4db0-91db-d21a1e2fef3c	Bob Smith	1990-03-05	bob.smith@example.org
57f182c5-47e2-4b71-b82c-63dc1795f9f5	Carol Davis	1978-11-23	carol.davis@example.net
a979dd61-daa4-4d88-a9f3-9a60c23d5b16	David Brown	2001-01-30	david.brown@example.com
179f4e15-0124-4a80-a25d-80e94a8e4ed9	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
Lastly, it’s important to note that the surrogate key is simply a mechanism to identify tuples, so it has no semantics in our domain. In other words, the values taken by the artificial attribute we have generated do not mean anything concerning the tuples or the domain in which they are represented.

Secondary Keys
The previous types of keys generally help solve the problem of uniquely identifying tuples. But besides identifying them, it’s important to operate on them and query them efficiently.

To do this, indexes are usually defined on attributes that do not necessarily identify the tables, such as the name or birth date of the previous Person table. By defining an index on one of these attributes, we can efficiently perform certain operations on the tuples of the table, all based on the values taken by the attributes on which we have defined an index. These attributes are called secondary keys, although we won’t go into detail about what an index is here.

Foreign key
To finish with the types of keys, the ones we have seen before mainly focus on solving the problem of uniquely identifying tuples, which is the purpose of keys, as well as contributing to the optimization of operations and queries on tables.

But keys also help implement certain elements of conceptual design on the logical design of the DBMS. Specifically, with the type of key we have yet to see, the foreign key, we can implement associations between entities at the logical level, which can occur in situations like this:

Entity-relationship diagram where a City is associated with many Person. Image by author. 

Here we return to the example where we conceptually model a domain with cities and people, where a person lives in exactly one city, and a city can have any number of people living in it, from 0 to infinity. Given the 1..1 multiplicity on the City side, every person must live in some city, but the 0..* multiplicity on the other side means cities may have no inhabitants.

The below diagram represents the conceptual design of our database, capturing certain details of the domain that we later need to transfer to the logical level. On one hand, we transfer the entities themselves to the logical level by creating a table for each entity directly:

ID	Name	Birth	Email
1	Alice Johnson	1985-07-12	alice.johnson@example.com
2	Bob Smith	1990-03-05	bob.smith@example.org
3	Carol Davis	1978-11-23	carol.davis@example.net
4	David Brown	2001-01-30	david.brown@example.com
5	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
CityID	Name	Country	Population	Area
1	Madrid	Spain	3,223,000	604.3
2	Athens	Greece	664,046	38.96
3	New York	USA	8,398,748	783.8
4	Tokyo	Japan	13,929,286	2,191.1
5	Paris	France	2,140,526	105.4
Given the tables for both entities, we now need to implement at the logical level the association we defined at the conceptual level. This means using a mechanism that allows us to know which city each person lives in or the people who live in a certain city.

If we think about this problem in terms of tables, we’ll see that the only way to do this is to add an additional attribute in one of the two tables so that this attribute takes as values the city where a person lives or the people who live in a certain city.

To understand this correctly, let's first assume that the primary key of the Person table is {PersonID}, which could be their government ID or an auto-incrementing surrogate key. Also, the primary key of the City table is the attribute {CityID}. This way, we can uniquely identify the tuples of City and Person using their primary keys, which take unique values for each of their tuples.

ID	CityID (FK)	Name	Birth	Email
1	5	Alice Johnson	1985-07-12	alice.johnson@example.com
2	5	Bob Smith	1990-03-05	bob.smith@example.org
3	4	Carol Davis	1978-11-23	carol.davis@example.net
4	2	David Brown	2001-01-30	david.brown@example.com
5	3	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
If we want to know the city where a person lives, we could add an attribute to the Person table so that the values it takes belong to the CityID attribute as shown above. That is, if the person "Alice Johnson" lives in the city "Paris," then in that row, the value of the new attribute CityID (FK) we added is 5, which corresponds to the CityID of the city "Paris" in its respective table. Similarly, if the person "Carol Davis" lives in the city of "Tokyo," then the new attribute will take the value 4, which corresponds to the CityID of that city in its respective table.

As you can see, the new attribute we added tells us which city the person represented in each row lives in, as it takes the primary key of the City table as its value. So, by knowing the CityID value, we can identify which city it is among all those stored in that table.

This additional attribute we add to represent the association is the foreign key. It mainly serves to implement associations between entities at the conceptual level, through attributes that serve as references or pointers to other tables. This is why it’s sometimes called an association pointer.

Before continuing, it's worth considering what would happen if, instead of placing the foreign key CityID (FK) in the Person table, a foreign key PersonID (FK) was placed in the City table. If we do this intending to reference all the people who are residents of a certain city, we would encounter a significant problem. That is, if we do this, we must keep in mind that a city can have an arbitrary number of residents, so in the value of its foreign key, we would have to store all the PersonIDs of its residents one after another in the same cell. This would result in a repeating group that is prohibited in the relational model.

So to avoid the appearance of this repeating group, we could refine or normalize our diagram, leaving it where we originally placed it in the first place, which is the attribute CityID (FK) in the Person table. This would be more complicated than simply changing the table where the foreign key is located.

Now that we understand the basis of what a foreign key is, it's important to note that, for an attribute to truly serve as a foreign key, it must reference an attribute in another table that is a primary key on its own.

In this case, the foreign key is composed of a single attribute, CityID (FK), which references CityID in the City table. If it referenced the Name attribute instead, there could be multiple different cities with the same name. This would mean that if we say a person lives in a certain city and use its name to identify it, we wouldn't be able to know exactly which city they live in if there are multiple cities with the same name.

That's why the foreign key references CityID, which we can guarantee uniquely identifies cities on its own, as it’s the primary key of City.

Composite Foreign key
Still, we don't always have domains and schemas as simple as these, where primary keys are a single attribute.

For example, we might have a diagram like the following, where there are people who own pools. Each person must own exactly one pool, but it's possible for several people to agree or partner up so that together they can own a pool. This means that each person will own a small percentage of the pool, which in this domain is not relevant. So a pool can be owned by an arbitrary number of people, including none, since there will be pools that aren’t yet owned by anyone.

Entity-relationship diagram where each Pool belongs to one person, and a person can have multiple pools. Image by author. 

Given the attributes of each entity, we can easily see that the primary key of Person is their {ID}, while to uniquely identify a pool, using just PoolName or CityName is not enough, since there could be multiple pools located in the same city or with the same name.

But if we assume that there can’t be multiple pools with the same name in the same city, we can establish a composite primary key as {PoolName, CityName}, where these attributes will uniquely identify each pool. When trying to translate this to the logical level, we first create the tables corresponding to both entities.

ID	Name	Birth	Email
1	Alice Johnson	1985-07-12	alice.johnson@example.com
2	Bob Smith	1990-03-05	bob.smith@example.org
3	Carol Davis	1978-11-23	carol.davis@example.net
4	David Brown	2001-01-30	david.brown@example.com
5	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
PoolName	CityName	Length	Width
Olympic Stadium Pool	Los Angeles	50.0	25.0
Community Center Pool	Chicago	25.0	12.5
Lakeside Aquatic Center	Seattle	33.3	15.0
Riverside Neighborhood Pool	Austin	30.0	10.0
Sunset Community Pool	Miami	25.0	10.0
Later, if we want to model the association between both entities with a foreign key, we first need to consider the cardinality of the association. On one hand, on the Person side, we have a cardinality of 0..*, indicating that a pool can belong to many people. On the other side of the association, we have a multiplicity of 1..1, indicating that a person can only have one pool.

With this, we can infer that if we place the foreign key in the Pool table, we would have to reference all the people who own each pool, resulting in repetitive groups in cases where there are multiple owners for the same pool (because we’d need to reference each and every owner from the same pool). That is, the pool would have an attribute whose value would be references to all its owners, and since there can be an arbitrary number of them, a repetitive group is formed.

To avoid this problem, whenever we have an association with cardinality 1 on one side and * on the other, or equivalents, we need a foreign key to model it at the logical level. Also, it should generally be placed in the table whose cardinality contains * as the maximum cardinality*, indicating an arbitrary amount. Here, by equivalents, we refer to cardinalities like 0..1, which we can treat similarly to 1..1, or 5.., which is equivalent to 0..\ because the maximum cardinality is still an arbitrary amount.

ID	PoolName (FK)	CityName (FK)	Name	Birth	Email
1	Olympic Stadium Pool	Los Angeles	Alice Johnson	1985-07-12	alice.johnson@example.com
2	Riverside Neighborhood Pool	Austin	Bob Smith	1990-03-05	bob.smith@example.org
3	Sunset Community Pool	Miami	Carol Davis	1978-11-23	carol.davis@example.net
4	Sunset Community Pool	Miami	David Brown	2001-01-30	david.brown@example.com
5	Olympic Stadium Pool	Los Angeles	Emily Wilson	1995-09-14	emily.wilson@example.co.uk
As you can see, in this case, the foreign key is placed in the Person table, which is the one with the * in its cardinality on the diagram, since each person can only own one pool. This prevents the foreign key from having to store an arbitrary number of references.

In this specific case, instead of a single attribute, we need to add PoolName (FK) and CityName (FK) because the primary key of Pool is not a single attribute but two. So the foreign key in Person will be a composite foreign key – meaning that instead of one attribute referencing another in a different table, there are two that simultaneously reference two attributes in another table.

For this to be valid, each attribute of the foreign key must reference an attribute of the primary key in the Pool table, so that together PoolName (FK) refers to PoolName, and CityName (FK) refers to the CityName attribute of Pool. So together they reference the entire primary key of Pool.

Finally, as we’ve just seen, foreign keys are a tool of logical design that we use to implement associations from the conceptual model. That's why in the conceptual model (in the entity-relationship diagram), we do not write the attributes that form the foreign keys. This is because at the conceptual level, the associations themselves indicate the relationships between entities. So even though tables have more attributes than we see in the diagram due to foreign keys, these extra attributes are never written at the conceptual level.

As for their naming, there are many style guides to follow. Here, we have added an (FK) to the attribute names to make it clear that they are foreign keys or part of one, although they can be named in any other way.

Weak Entities
Now that we’ve defined how foreign keys allow us to implement associations between entities, we’ll continue by analyzing a case where one of the associated entities can’t be identified on its own with its attributes. Instead, it needs a foreign key that references another entity to be correctly identified – this means that the entity is considered weak in identification.

Existence weakness
Before continuing, you should know that there are several types of weaknesses in this context. One is existence weakness, which means that an entity called weak can’t exist if there isn't another entity called owner with which it’s associated.

Entity-relationship diagram where each person is composed of one brain. Image by author. 

We can understand this with the previous example, where a person is composed of a brain, and a brain must always be part of a person. So, when an instance of the Brain entity is created, meaning a tuple representing a brain is created, a person must also be created to be associated with that person.

In summary, a brain can’t exist without the Person entity it’s related to. This leads to an existence weakness where we say the Brain entity is weak and the Person entity is the owner or strong. The composition allows Person to exist without a Brain, even though we prevent it here with cardinality.

Aside from this, when we have an association where all its cardinalities are 1..1, it’s very likely that we can combine those two entities into one, like Person, adding attributes like Neurons, instead of having two entities. But this doesn't always have to be done this way, as it depends on how we want to model the domain and the requirements.

Identification weakness
In addition to existence weakness, we can have an identification weakness. Here, by identification, we mean the mechanism by which each tuple in a table is uniquely distinguished from all others, as we have seen before with keys.

To understand this type of weakness, when it occurs, and how it’s managed, we can look at the following case:

Entity-relationship diagram where Residence is a weak entity that connects City and Person with start and end dates. Image by author. 

Here we have some entities:

City, which models cities in the domain

Person, which does the same for people, and

Residence, which models a person's stay in a specific city.

This means people can live in a city for a certain time and then move to another. So according to this diagram, they would leave behind an occurrence or tuple of Residence with the date they started living in the city and the date they moved to another.

Regarding cardinalities, we can see that a city can be related to many residences, as it may have or have had many inhabitants, while a residence is only related to one person because the residence focuses on recording that a certain person has lived in a certain city. So the 1..1 multiplicities force a residence to link a person with a city, as introducing optionality here would imply that a residence can link a city or person with "nothing," which doesn't make sense.

Meanwhile, on the Residence side, we have 0..* multiplicities with optionality because a person may not live in any city, or conversely, a city may have or have never had any inhabitants, so it may not be related to any occurrence of Residence.

Next, when we translate this diagram to the logical level, we first try to define the primary keys for all the entities or tables. In this case, for City and Person, it's straightforward, as we assume CityID is a unique identifier for each city, and ID is a unique government identifier for each person (tuple).

But when we define the primary key for Residence, we have several options. On one hand, we could choose {StartDate} or {EndDate} as the primary key, but this isn't feasible because multiple people might start living in the same or different cities on the same start date, end date, or both. So we can't even choose {StartDate, EndDate} as the primary key, since, in the worst-case scenario, multiple people might start and stop living in a city at the same time.

This means that the Residence entity needs the other entities it’s associated with to have a primary key and be identifiable. It's important to note that at the logical level, we would have two foreign keys in Residence due to its two associations with City and Person. Specifically, it has a foreign key CityID (FK) and another ID (FK) that model these associations, respectively.

We can infer this at a glance without "seeing" the logical model because we have associations with cardinalities 1..1 and 0… So on the 0.. side, there must be a foreign key to implement this association as we’ve seen before.

Given these foreign keys, we might consider choosing {CityID (FK)} or {ID (FK)} as primary keys, but this wouldn’t guarantee the identification of all tuples because multiple people can be living in one or several cities at the same time. Also, a city can have multiple residents simultaneously, leading to repeated values in the foreign key attributes for tuples that should be considered distinct.

We also can’t choose {CityID (FK), ID (FK)} as a key because a person may have moved to a city multiple times during different periods, even if they lived in other cities in between. This would result in multiple tuples with the same values in both foreign keys but different values in the dates.

Given this situation, the only option left is to consider a key that includes one of the date attributes of Residence and the foreign keys {CityID (FK)} or {ID (FK)}, since nothing prevents a person from having multiple residences at the same time (where each residence indicates they are living in a city). This is normal because we haven't restricted this situation in any way in the conceptual diagram.

So, since a person can live in multiple cities at once, to identify a Residence tuple, we need to know which person is living in which city, plus at what point in time they are doing so. This we can determine with StartDate or EndDate. One of the dates is sufficient here, because a person can only live in a city once at the same moment in time, meaning a person can’t start or stop living in the same city multiple times at the same moment.

So to sum up, if we want to uniquely identify the Residence entity, we need to select {StartDate, CityID (FK), ID (FK)} as the primary key, although we could also select {EndDate, CityID (FK), ID (FK)} as long as we are sure that EndDate always exists. If the end date is not defined until the person leaves the city, we couldn't consider EndDate for identifying Residence.

So we can see here that we can’t identify the entity without using the respective foreign keys. This means the entity is considered weak in identification, as it depends on the two entities City and Person, which in this context are considered the owners of the weak entity. In other words, the owner entities can be identified by themselves, while the weak entity depends on other entities for its identification.

To denote this in the entity-relationship diagram, we can use a «weak» role on the sides of the weak entity to indicate that the foreign keys of these associations are needed to identify the weak entity.

To correctly understand what weak identification means, we can now consider the same diagram as before. But now, let’s assume that a person can only live in one city at a given moment in time, unlike before when they could live in many cities at once. This restriction can’t be modeled with UML elements, so it's enough to add a textual note in the diagram to reflect the restriction.

Entity-relationship diagram where Residence is a weak entity that connects City and Person with start and end dates. Image by author. 

In this case, since a person can only live in one city at a time, we don't need to include the foreign key CityID (FK) in the primary key of Residence. If a person is living in a city at a given moment, they can't be living in another, so there won't be more tuples in the table with that person and that start and end date of residence.

Consequently, the primary key of Residence becomes {StartDate, ID (FK)}, for example. The only thing that changes besides this primary key is the conceptual diagram itself, where now the only owner entity of Residence is Person because the foreign key to City is no longer strictly necessary for its identification. So even though Residence remains weak, its only owner entity is Person. This is why the role "weak" is only written in the association that gives rise to the foreign key ID (FK), which is indeed in the primary key of Residence (unlike the previous scenario where we placed the role in both associations).

So as you can imagine, with the "weak" roles, we can not only know which entities are weak but also which entities own them. The role is always on the side of the association where the weak entity is found – that is, where the foreign key referencing the owner entity is located, which corresponds with the cardinality * seen before. Then on the other side of the association with the "weak" role, we find the owner entity.

Entity-relationship diagram where Residence connects City and Person with residency dates. Image by author. 

If we want to convert Residence into an entity that is not weak, we need to add enough attributes to identify it without relying on other entities. For example, if we add a surrogate key ResidenceID that works through auto-increment or UUID, then we can automatically identify each tuple of Residence uniquely, so the primary key of Residence would become {ResidenceID}, and the entity would no longer be weak.

Finally, if we consider the domain we initially proposed and its requirements, we see that Residence is weak in identification, needing both foreign keys to be identified. So in addition to being represented with the "weak" roles in both associations, it’s worth noting the possibility of representing it using an associative entity like the following:

Entity-relationship diagram where Residence connects cities and people, allowing multiple relationships on both sides. Image by author.

We can make the diagram this way in this situation because Residence has Person and City as owner entities. Since it’s linked with the association between both entities and needs both to be identified, it can be denoted as an associative entity.

But an associative entity and a weak entity are completely different concepts, as weakness in identification is a property of entities, while an associative entity is a way to represent entities in UML at a conceptual level.

For example, if Residence had only Person as an owner entity, then it would no longer make sense to represent it as an associative entity at the conceptual level. This is because it’s only a weak entity in identification with respect to one owner entity, Person, not two owner entities that can have an N:M association between them.

In addition to the representation as an associative entity, the cardinalities on both sides of the association must be 0..*, since it was previously stated that a city could have an arbitrary number of residences, where each one had only one person, necessarily. So if we represent Residence as an associative entity, the association between City and Person must have a 0..* on Person. This indicates that a city can be related to an arbitrary number of people through the Residence entity, with the same occurring in the reverse direction.

Navigability
In relation to the previous example and the concept of association or foreign key, it's sometimes important to analyze the navigability of our entity-relationship diagram before implementing the logical design of the database. This is because efficiency problems, ambiguities, or even the impossibility of performing certain operations or queries may arise.

To begin with, navigability refers to the capacity we have to “navigate” on the entity-relationship diagram through the associations between entities, or in other words, if we are located on a certain entity, it refers to the ability offered by the associations that affect that entity to navigate these associations and to retrieve information from other entities.

Entity-relationship diagram where Residence connects City and Person with residency dates. Image by author. 

To understand this with an example, we can refer to the last diagram from the previous section where we introduce a surrogate key to Residence. In that diagram, we have an entity Residence with two foreign keys pointing to City and Person. So if we are given a tuple from Residence, we can use its foreign keys to determine which tuple from City or Person is associated with the occurrence of the Residence entity. This allows us to navigate those associations to the corresponding classes.

This is useful, for example, when we query the database to find the person who lived in the city corresponding to that Residence. For this, we can look at the value of the foreign key ID (FK), which corresponds to an identifier of a person recorded in the Person table. This allows us to navigate from the Residence entity to the Person entity, meaning we’ve gotten information from the Person entity starting from Residence.

We can repeat this step multiple times, navigating from entity to entity through the diagram. But the important thing is to know which associations are navigable in a certain direction.

For example, if we are given a person, that tuple doesn't have any foreign keys, so with a tuple representing a person, we can't get information about any other entity in our diagram – not even Residence. If we only look at the values of the Person tuple, we won't know which Residence tuples are associated, because we would need to query and traverse the entire Residence table to find out.

To sum up, the Residence-Person association is not navigable in both directions – we can only go from Residence to Person, but not the other way around. The same applies to City.

Navigability is important, because it's useful to know the direction in which the diagram's associations can be navigated before implementing anything. If our system needs to support a query like obtaining the city where a person currently lives, it might be more efficient to add an association directly from Person to City instead of having to go through all the Residence tuples to resolve the query, which would be more efficient.

Entity-relationship diagram where Residence connects City and Person, with an additional direct relationship between them. Image by author. 

Although this association might seem redundant, if we need to focus heavily on optimizing the query we mentioned earlier, it may be worthwhile to "complicate" the diagram in this way so that certain critical queries in our system run faster.

It’s also important to note that a person may not live in any city, which is why the minimum cardinality of the new association on the City side is 0..1. This is because the foreign key resulting from this association may "not exist," as we will see later, representing that a certain person does not live in any city.

Finally, not everything relevant about navigability is related to efficiency, such as when detecting navigation cycles. If several exist, we would need to ensure in the implementation that the DBMS optimizer chooses the shortest one in the corresponding queries.

Navigability also helps us see if certain queries can be resolved, meaning if certain data can be obtained from the system based on some input. And keep in mind that this concept of navigability that we have introduced refers to navigability over the conceptual diagram itself, not to the possibility of obtaining information about other entities at the logical level, as we’ll see later.

Constraints
Continuing with the elements of the relational model, the only thing left to discuss are constraints. These are conditions imposed on the data to correctly model the domain and meet its requirements. They are a set of rules that must always be followed so that the stored data is correct, consistent, integral, and aligns with the semantics given by the domain.

We can define constraints both at the conceptual and logical levels. On one hand, in the conceptual model, constraints are mainly modeled using the tools provided by UML when creating the entity-relationship diagram.

For example, let’s say that in our domain we have a business rule or condition stating that a city can have a maximum of 500 inhabitants. Then if we model the domain with a diagram similar to those created earlier, we will have an association between person, inhabitant, and city, where we use the cardinality of that association (specifically the maximum cardinality) to represent the constraint of the maximum number of inhabitants.

But not all constraints can be modeled at the conceptual level with UML tools. For example, consider the case where we have a social network with people who can follow other people. We can model this with an entity Person and a recursive relationship where a person can follow an unlimited number of people, including the case where they follow no one.

Entity-relationship diagram where Person has a follow relationship with itself. Image by author. 

But nothing prevents a person from following themselves, which doesn't make much sense in a social network. So we could leave it as it is if the client doesn't specify otherwise. But if the domain itself or a requirement indicates that a person can’t follow themselves, we will need to add that restriction to the diagram.

Unfortunately, we can’t do this with the tools provided by UML, as there is no mechanism to indicate that this association can’t occur between the same occurrence (tuple) of the Person entity.

In this case, we have several options to reflect the restriction in the conceptual design. The first and simplest is to add a textual note on the margin of the diagram where we briefly explain the situation and indicate the rule that makes up the restriction. Notes in UML are standard elements consisting of a box with text where things that can’t be properly modeled with the diagram's own elements are specified.

On the other hand, instead of using a text note, which is less formal and more prone to misinterpretations or confusion, we can use a specific language to represent constraints like OCL (Object Constraint Language), where we define the restriction using the language's own code.

context Person
inv noSelfFollow:
    self.follows->forAll( p | p <> self )
Here, we won't go into detail about how constraints are modeled in OCL. The important thing is to know that there are constraints that we can’t directly represent with diagram elements, so they need to be reflected in the conceptual design using notes or specialized language code.

Data Integrity
As we’ve mentioned, constraints are validity conditions imposed on the data. They help ensure that, when stored in our database, they can be checked for correctness, consistency, and integrity, all verified automatically by the DBMS. This is because the constraints themselves are usually implemented at the logical level in the DBMS, which has specific functionalities to check constraints and ensure the correctness and integrity of the data.

So far, we have assumed that the data are stored correctly in their respective tables. We’ve also assumed that they respect the attribute domains, as well as many other details that can affect the validity of what is stored.

So to avoid issues, the database automatically checks the validity of the data, which differs from the correctness of the data. To understand the difference between these concepts, consider the following example:

CityID	Name	Country	Temperature (Kelvin)
5	Paris	France	280
1	Madrid	Spain	-3
Here we have a Temperature table that stores tuples with the temperatures in a city at different times. As you can guess, the temperature attribute is of type integer, which means it can hold any integer, including negatives. But temperatures can’t be negative if measured in Kelvin, so if we are measuring temperatures in Kelvin here, we must add a domain constraint like Temperature >= 0 to prevent the Temperature attribute from taking negative values. This is called a domain constraint.

Domain constraints, as you’ve just seen, are used to define the domains of table attributes, restricting the possible values they can take and ensuring that the stored data is of the appropriate type.

Given this restriction, we can see that the first tuple meets all the constraints, so it could be considered valid data. But with the information we have, we can’t ensure that this data is correct. That is, we have not taken a thermometer and measured the temperature in Paris, so we do not know if that 280 is the actual temperature in Paris or if it’s incorrect data. So even if data meets the constraints, we must ensure that it’s correct.

This is a very complicated task that we won’t go into detail about here. We can implement mechanisms for error detection and correction in data, or we can conduct audits to verify that the data corresponds to reality – that is, the domain. Or third parties can supervise the data, because if the person who took that measurement tells us that the 280 is not what they recorded with the thermometer, then we know that data is incorrect. Otherwise, we would have no way to guarantee its correctness.

On the other hand, in the second tuple, the temperature takes a negative value, so we can conclude that this data is not only incorrect but also invalid. It’s invalid because no Kelvin temperature can be negative, violating the domain constraint imposed earlier. It’s incorrect because if it’s invalid, then that value must necessarily be different from the true temperature of the city.

So now you know what it means for data to be erroneous or incorrect. You also understand domain constraints that can ensure data integrity in terms of data type and possible values that the attribute can take.

But data integrity goes beyond simply checking that data is in the correct format and within an attribute's domain. For example, data must be reliable and accurate, which we verify with its correctness. It must also be consistent, meaning there can’t be duplicate tuples with information that leads to contradictions as seen earlier. It must also have other high-level characteristics like availability, durability, data timeliness, security, and so on which we won’t delve into because they aren’t essential here.

Integrity Constraints
In addition to the previous characteristics, there is another one that’s essential for maintaining data integrity: completeness. In this context, completeness can have several meanings, with the simplest being that all data points are present in the database as tuples. This means all the "individuals" of the domain are represented in the database.

For instance, if we are storing a domain with 10 people and only see 9 tuples in a table like Person, we know that the data is not complete because the entire domain is not represented by the 9 tuples. On the other hand, completeness also means that each data point must necessarily have a value for each attribute of the table that defines it.

CityID	Name	Country	Population
1	Madrid	Spain	3,223,000
2	Athens	NULL	664,046
3	New York	USA	8,398,748
4	Tokyo	Japan	13,929,286
5	Paris	France	NULL
For example, if we have a domain with cities, and in our database we include a City table with these attributes, then each city (data point) we represent with a tuple must have a value for each of these attributes. This means that for the data to be complete, no cell in the table can be empty.

In the table above, you can see that the cities named "Athens" and "Paris" prevent the data from being complete, as one does not have a value in the Country attribute and the other in Population, respectively. Instead of leaving the corresponding cells empty, the special value NULL is stored in them to represent that they contain nothing.

To ensure the completeness property of the stored data, NULL values should be avoided in the tables. But we will later see that by default, DBMSs do not usually enforce the restriction that table values can’t be NULL. In other words, when we create a table by default, the values of the tuples can be NULL unless we define otherwise through a restriction.

We typically define this restriction at the attribute level, where we specify that the values in the column corresponding to that attribute can’t be NULL. So all tuples we save in the table must have a value other than NULL for that attribute.

This affects the attribute's domain, since by default, the special value NULL is included in the set of all values an attribute can take. But we can exclude this value from the set using a restriction.

In light of all this, and after introducing the concept of NULL, we can define integrity as a property that ensures that throughout its entire lifecycle, the stored data is valid, correct, consistent, complete, and reliable.

To ensure that all these characteristics are met (except for the last one, which is at a higher level), we use special types of constraints in the database, known as integrity constraints. In other words, we can categorize database constraints based on their purpose.

Some constraints dedicated to modeling business domain requirements and rules, while others are integrity constraints specifically aimed at enforcing the aforementioned integrity characteristics (but some of them may also indirectly model part of the business rules).

These last constraints are validity conditions automatically checked by the DBMS every time an operation is performed on the entity (table) or entities affected by these constraints, all with the goal of ensuring data integrity at all times.

These validity conditions, as we’ve seen, must be met for all stored tuples, ensuring that none of them can have an empty cell or a disallowed value. In other words, conditions can be defined at the attribute (column) level, although the tuples stored must adhere to these constraints. This is why they are checked for all of them. So when all the tuples stored in a table meet all the defined integrity constraints, the instance of that table is said to be legal.

Integrity constraints, depending on their logical purpose, can be classified into several types:

First, we have domain constraints. These are the ones we just discussed, and they mainly serve to define the data type of the attributes and their domain.

On one hand, implicit domain constraints include those that define the data type of the attributes, as this is something we must do when creating a table, not something we add later to limit the attribute's domain.

On the other hand, there are explicit domain constraints, which we add in addition to the data type definition to limit the values that attributes can take, such as preventing them from containing the special value NULL, or preventing an attribute that stores temperatures in Kelvin from taking negative values, as we have seen. We can also consider it implicit that the DBMS allows cells to take NULL values, which we can prevent by setting an explicit constraint.

Next, we have identification constraints. Regarding the identification of tuples, we previously saw that a primary key is chosen for each table so that its attributes can uniquely identify all the tuples stored in it. The explicit definition of a primary key is an integrity constraint that we define on the table.

But by doing this, the DBMS internally applies several sub-integrity constraints, one of which ensures that the combinations of values taken by the primary key attributes are all different (meaning unique). This is what characterizes a key. Also, none of the attributes can take NULL as a value, because if they could, there would be multiple tuples with the same value for the primary key.

ID	Name	Birth
1	Alice Johnson	1985-07-12
NULL	Bob Smith	1990-03-05
3	Carol Davis	1978-11-23
4	David Brown	2001-01-30
NULL	Emily Wilson	1995-09-14
For example, if our primary key is a single attribute {ID}, then it can’t take NULL as a value, because in that case, we could have multiple tuples with NULL in that attribute as seen above, preventing them from being uniquely identified.

Lastly, we have referential constraints. Related to the previous constraints are referential integrity constraints, which ensure that the relationships between tables are consistent at all times. These constraints are implicit, meaning the DBMS automatically ensures that they’re fulfilled. Still, we must explicitly define which attributes are foreign keys for it to do so.

Entity-relationship diagram where Pool is a weak entity dependent on City. Image by author. 

Here, by consistent, we’re not referring to the same concept as data consistency. Rather, we mean that a foreign key must reference a valid tuple in the table it points to.

For example, if we have a weak entity Pool whose logical table has a foreign key attribute like CityID (FK), then whenever this attribute references a city, it must contain a valid CityID value. This means it must exist in the City table. If the value doesn't exist, then it wouldn't be referencing any city.

Also, note that the foreign key attribute itself can be NULL by default unless we specify otherwise, because the foreign key constraint doesn't behave like the primary key constraint, which implicitly prevents NULL values. Instead, the foreign key constraint is solely focused on ensuring consistency in references, not on preventing NULL values.

To understand this, we need to look at the 1..1 multiplicity on the City side, which requires all pools to belong to exactly one city, ensuring no pool is "loose" or outside a city. This means all pools must have a value in their foreign key CityID (FK), as they must belong to one and only one city.

For this restriction (which we've conceptually modeled with a minimum cardinality) to be translated to the logical level, we need to explicitly indicate a domain integrity constraint on the CityID (FK) attribute so it can’t contain NULL values. This means it must always refer to a city. This, in turn, allows the Pool entity to be identified by the pool's name and the city where it's located, as the name can be repeated in several tuples/pools. But the combination of the name and city where they are located is assumed to never repeat in our domain. In other words, in the same city, there are no multiple pools with the same name.

Assuming this, if in our database we have a series of tuples in both tables and we want to delete a city from the record, then we need to check if there is any pool referencing that city. This would prevent the city record from being deleted to maintain integrity and ensure that the respective foreign key of the pool continues to reference an existing city.

To resolve this situation, there are many policies that we will see later, although the most common is to prevent the deletion operation from being executed or to also delete the pool record that references the city we want to delete. This could cause more recursive deletions if there are foreign keys pointing to Pool.

Entity-relationship diagram where a City can have none or one Pool, and a pool can belong to none or one city. Image by author. 

On the other hand, if the minimum cardinality on the City side is 0, this means that at the logical level, the foreign key of Pool may not exist – meaning the pool might not be in any city. So its foreign key can take the value NULL because it's the only simple way to implement that the foreign key itself "does not exist."

If we do this, we won't have to define the explicit constraint that the foreign key attribute is non-null, and when deleting a city record, we can set the deletion policy so that the foreign key in Pool is set to NULL.

As for the weakness in identifying Pool, it disappears here because it can't use its foreign key for identification since it can take the value NULL and the pool name can be repeated. Because of this, we decide to add a surrogate key PoolID to identify the Pool entity.

Finally, nothing prevents a foreign key from modeling a recursive relationship, meaning the DBMS implicitly allows it by default. So if we want to avoid situations where a tuple references itself, we must add explicit constraints, which we can categorize as referential integrity.

Chapter 6: Relational Schema Diagram
After introducing the relational model at the conceptual level, we must remember that this is the first level of database design. Now, based on the entity-relationship diagram, we need to determine the tables that will make up the database, as well as the keys they will have to identify and reference each other. We also need to define the constraints that ensure the validity and integrity of the data.

So even though we’ve already introduced certain concepts of logical design, here we’ll formalize the logical design itself through relational schema diagrams, sometimes called relational diagrams for simplicity.

Relational schema diagram where Pool references City through a foreign key. Image by author. 

As you can see, here we have a relational diagram representing the logical design associated with the last entity-relationship diagram from the previous section.

First, instead of entities, we have tables here, each with a series of attributes. If any attribute is used in a primary key, it’s underlined like PoolID or CityID, with all other attributes being "normal" table attributes. Also, foreign keys are represented directly with arrows. In this case, CityFK is a foreign key that references the CityID attribute of the City table because it’s a primary key, which is why it's denoted with an arrow pointing from the foreign key attribute to the corresponding attribute in the other table.

Regarding the foreign key, keep in mind that an attribute can only point to one other attribute – meaning CityFK can only have one arrow pointing to one attribute, not several, as the foreign key references a single attribute in another table. If we were asked to convert this relational diagram into an entity-relationship diagram, the foreign key itself would determine the cardinalities of the association (at least the maximum cardinalities, since, for that foreign key to make sense, at the conceptual level, it would translate to a pool being in only one city at most, while a city can have an arbitrary number of pools).

These types of diagrams aren’t standard like UML. They only need to meet the characteristics mentioned earlier. That's why, in many cases, tables are represented with squares similar to UML entities instead of being shown in textual format with Datalog.

But unlike UML diagrams, there are very few implicit restrictions here. Most restrictions need to be added with notes in the margins. For example, to indicate that an attribute can’t have a NULL value, we can’t do it with diagram elements – instead, it must be represented by other means, such as a note or a piece of code in OCL.

1-1 association
Given the nature of relational diagrams, we can infer that entities are directly transferred to the logical model with tables, where each entity corresponds to a table. But in addition to the tables, we have to implement the associations between entities at the logical level.

To do this, we start with the simplest case, which is an association where the maximum cardinality on both sides is 1, as in the example we saw earlier where we had an entity Person composed of an entity Brain, whose translation to a relational diagram would be as follows.

Relational schema diagram where Person and Brain reference each other through foreign keys. Image by author. 

As you can see, both entities are represented with tables, where the attributes of their primary keys are underlined. Also, even though they don't appear in the conceptual diagram, we need to reflect the existence of foreign key attributes used to implement the association itself.

So we’ve added attributes with names that best indicate that they are foreign keys. In this case, the name ends with FK, although you can use any name you like. So for a brain to be associated with a person, its corresponding foreign key refers to the primary key of the table that stores people. Since the other direction of the association is symmetrical, we do the same with the foreign key of Person (which refers to the primary key of Brain so that a person can be associated with their corresponding brain). We do this with foreign keys for simplicity and because it's the only way to determine which brain each person has and to whom each brain belongs.

Because of the 1-1 association, you typically shouldn’t leave this type of association due to the overhead caused by using multiple foreign keys referencing in both directions, and the redundancy at the conceptual level. If each person has one brain and only one, and vice versa, it's likely that both can be "merged" and modeled as a single concept, moving all the attributes that characterize Brain to the Person entity, for example. But there are other ways to refine the schema, or there are times when the domain or requirements force us to keep this type of relationship, in which case it would be perfectly valid.

1-M association
Another type of association we need to translate to the logical level is called 1-M (or 1-N), which refers to associations where the maximum cardinalities on both sides are 1 and * respectively, where M means an arbitrary amount.

Entity-relationship diagram where each House belongs to a single Person, who can have multiple houses. Image by author. 

For example, here we have a 1-M relationship between the entities House and Person, where a house must belong to a person, and a person can have an arbitrary number of houses, including none. At the logical level, we can represent this as:

Relational schema diagram where House references Person through a foreign key. Image by author. 

Just like before, we implement both entities with tables, and the 1-M association between them with a foreign key in the entity on the side where the maximum cardinality is *. Specifically, to avoid repetitive groups, we place the foreign key in house, since a house can only have one person as its owner. This means it won't be necessary to store an arbitrary number of references in the attribute of its foreign key – one is enough.

And as always, the foreign key refers to the primary key of Person, so that it can reference a value of an attribute that can uniquely identify a person, and thus determine the owner of a house.

Minimum cardinality issues
Regarding the previous entity-relationship diagram, we can see that the 1..1 side indicates that at a minimum, a house must always be associated with a person who will be its owner. This means that a house must always have an owner. But this is not realistic, as when a house is built, it may be without an owner for some time, causing the cardinality on that side of the association to become 0..1.

In turn, the minimum cardinality of 0 means that a house may not have an owner – so its foreign key should not exist while the house has no owner. To model this, attributes, including foreign keys, are allowed to take NULL as a value by default (as we’ve seen before). This way, to represent that the foreign key does not point anywhere, we simply choose not to restrict the possibility of it taking this NULL value. So when a house has no owner, its foreign key attribute will be NULL until it references a person – that is, a tuple in the Person table.

This situation, where a foreign key is allowed to take the NULL value, is not explicitly indicated in the relational diagram. Instead, it’s indicated when the opposite situation occurs – where if the foreign key can’t be NULL, we need to add a note clearly indicating this (as is the case in the original entity-relationship diagram we just saw).

On the other hand, the association in the diagram has a multiplicity of 0..* on the House side, indicating that a person doesn’t have to own any house. But if we had a minimum cardinality greater than 0, then this restriction would need to be defined with a note in the relational diagram, as well as with specific SQL tools (since there are no standard elements to model this type of requirement caused by minimum cardinalities in such a situation).

N-M association
To conclude with the types of associations according to their cardinality, the only one left to translate is N-M. In this case, N and M denote arbitrary quantities, meaning associations whose maximum cardinalities are both * at the same time.

Entity-relationship diagram where House and Person have a many-to-many relationship. Image by author. 

As an example, we could have a domain where a person can own many houses, and a house can be owned by many people at the same time. To model this situation at the conceptual level, the first thing we might think of is to create a diagram like this, where we only put an association with cardinality 0..* on both sides.

Conceptually this would be consistent, but logically it can’t be translated in any way. That is, if we have an association with a maximum cardinality of * on both sides and try to implement it logically using foreign keys as we’ve done so far, we’ll find that even if we put a foreign key in both entities referencing the entity on the other side of the association, the problem of the repeating group will always appear in both entities, regardless of what else we do.

To understand this, we can look at it conceptually. If a person has an indeterminate number of houses and we put a foreign key in Person referencing House, then that foreign key would need to contain references to each of the possible houses the person might have. Since it's not a fixed number, a repetitive group appears in the foreign key.

The same happens in reverse: if a house can have an arbitrary number of owners, then including a foreign key in House referencing Person would cause a repetitive group in the foreign key. So this type of association does not have a direct implementation at the logical level.

But in reality, these situations usually don't occur this way. Instead, it's common for there to be an intermediate class in the association that allows for its implementation at the logical level, as in the following example:

Entity-relationship diagram where Property connects houses and people with sale dates and prices. Image by author. 

Here, we have a situation similar to the previous one, where a person can own an arbitrary number of houses, and a house can be owned by an arbitrary number of people. The difference here is that we assume one of the domain requirements is to record when a person buys and sells a house, as well as the price at which it was bought. We don’t need the the selling price because it will be the purchase price for another occurrence of Property.

For this, we use an intermediate Property entity that stores this data, where we must keep in mind that SellDate should not "exist" until the house is actually sold, if it’s sold at all. So to translate this to the logical level, the simplest approach is to allow SellDate to be NULL until the house is sold.

Relational schema diagram where Property references House and Person through foreign keys. Image by author. 

As we can see, this situation can now be translated into a relational diagram, meaning at the logical level. This is because all entities can be represented as tables. And since the associations are of the 1-M type, we already know how to implement them using foreign keys, specifically in the Property entity referencing the primary keys of the other two entities, respectively.

This doesn't mean that whenever we have an N-M relationship, we need to introduce an intermediate entity to implement it. Sometimes we need an intermediate class to store information, as in this case, and in other situations, we might need to refine the schema because the N-M relationship doesn't best represent the domain.

But if we really need to implement an N-M relationship and we’re sure that this relationship is conceptually correct, we can always add an artificial intermediate entity that has no attributes other than the foreign keys of both associations (with both being the primary key), thus making it a weak entity in identification.

Entity-relationship diagram where Property is a weak entity that connects House and Person with sales data. Image by author. 

For example, considering the situation where we do need to store information in an intermediate class, we previously saw that Property had its own primary key, PropertyID, probably derived from a surrogate key. But if there is no surrogate key, we must try to identify the tuples of Property through their attributes. In this case, this isn’t possible given their semantics – meaning the significance of what they store – as there could be multiple tuples with the same dates, prices, and so on.

So, knowing that two foreign keys will appear in Property referencing House and Person when translated to the logical level, we can use them to define the primary key of Property using BuyDate and the foreign key attributes themselves.

We do this because if we only make the primary key consist of the foreign keys, then Property can’t be uniquely identified if a person buys and sells the same house during multiple different time periods. So we add BuyDate to the primary key to also distinguish by purchase date, because SellDate can be NULL (which violates the fundamental integrity constraint of primary keys that none of their attributes can be NULL). With this, the Property entity becomes weak in identification, which is why we’ve added «weak» to both sides, indicating that we need both foreign keys for identification.

Entity-relationship diagram where Property links houses and people with purchase and sale information. Image by author. 

Similarly, in this case, since the weakness in identification affects the entities on both sides (meaning we need the foreign keys referencing the entities on both sides of Property), it can be represented conceptually with an associative entity linked to the M-N association between House and Person. This is still equivalent to the previous diagram, with the only difference being that the intermediate class is represented differently.

Also, it’s important to note that if Property had a surrogate key and did not need foreign keys for its identification, this representation using an associative entity would not be valid. Ultimately, the associative entity is only valid to use in this context when the intermediate entity depends on the two linked entities for its identification, with these being its owning entities.

Relational schema diagram where Property references House and Person through foreign keys. Image by author. 

Regarding the logical-level translation of this last case, we. doit in the same way – the difference being that we no longer have the PropertyID attribute in Property. Also, its primary key is now {BuyDate, HouseFK, PersonFK}, so we underline all those attributes.

As a general rule, when a foreign key is underlined in a relational diagram, it indicates that the conceptual-level entity corresponding to the table is weak in identification. This lets us know how many entities it depends on – that is, its owning entities.

IS-A Hierarchy
After seeing how entities and associations from the relational model are translated to the logical level, let’s now understand how the special relationships of generalization and specialization among the entities themselves are translated.

To do this, we’ll start with an example of an IS-A hierarchy. This basically means that one or more entities, like CityPool, are a specialization of another more general entity, Pool. This is very similar to what happens in object-oriented programming with inheritance.

The inheritance hierarchy is called IS-A because if CityPool inherits from Pool, then it’s more specific than Pool. This means that every city pool is a pool, but it has specific attributes that characterize city pools, such as their maximum user capacity or the ticket price.

Entity-relationship diagram with inheritance where CityPool and OlympicPool are subclasses of Pool. Image by author. 

Before seeing how they are translated to the logical level, it's important to know that IS-A hierarchies have a series of specialization constraints that determine the "relationship" the parent entity (Pool here, sometimes called superclass) has with the specific entities. In other words, if we consider that entities are actually sets containing all their occurrences (tuples) (which we’ll call individuals here to keep it a more "general" concept), away from the details of the conceptual and logical model, then we can establish constraints like completeness or disjunction of a hierarchy.

To understand completeness using this example, we can first have hierarchies that are complete, where all individuals of the entity Pool must necessarily belong to the sets of individuals of one of the specific entities that inherit from the superclass Pool.

In this case, the superclass Pool is an entity that contains all existing pools. So some of them might be city pools, belonging to the set of individuals formed by the inheriting entity CityPool. Others might be Olympic pools, which belong to the set of individuals of OlympicPool.

In our model, we have only specified these two types of pools, while in reality, there are many other types of pools. In this hierarchy, they’d be represented by individuals in the set generated by Pool, as they don’t have any inheriting class to belong to. So in this case, our hierarchy would not be complete, but partial, since there are pools that do not belong to any inheriting entity.

On the other hand, disjunction refers to the possibility of individuals belonging to more than one inheriting entity at the same time. For example, in our case, a pool is either a city pool or an Olympic pool, or it’s neither of those types – so we will never find a pool that is both a city and an Olympic pool at the same time.

If we consider the sets of individuals of the inheriting entities, the hierarchy is considered disjoint when those sets are disjoint, as in this case where pools are either one type or the other, but not both at the same time. Conversely, in cases where the latter occurs, the hierarchy is called overlapping.

1 table
Knowing now that the hierarchy in our example is incomplete (called partial) and disjoint, we need to implement what’s shown in the entity-relationship diagram at the logical level.

We have several options for this. One option is to implement the entire IS-A hierarchy with a single table, Pool, that gathers all the attributes of the tables in the hierarchy.

Image by author. Relational schema where Pool includes attributes for capacity, price, and competition features.

As you can see, in this option we implement a table that contains all the attributes of the three tables, where PoolID functions as the primary key of the entire Pool table, since in the conceptual design specific identifiers aren’t usually assigned to inheriting entities unless required. This is why PoolID appears underlined. As for the rest, they work the same as if they were in their respective entities.

On one hand, this option has the advantage of using only one table for the entire hierarchy, which makes it easier to understand and maintain. It also avoids the possible redundancy of storing the same information in multiple tables.

But on the other hand, it presents significant problems. First, we have no simple way to distinguish a pool from a city pool or an Olympic pool, meaning the only way to know the specific type of pool that a tuple in the Pool table represents is to have some attributes be NULL.

For example, if a tuple represents a pool from the Pool entity, then all the attributes of CityPool and OlympicPool must be NULL so that the corresponding tuple only takes values in the attributes of the Pool entity. This lets us determine that the tuple represents an "individual" of the set of occurrences of the Pool entity.

The same thing happens when we try to distinguish city pools, where all the attributes of OlympicPool must be NULL, since CityPool inherits all the attributes of the Pool entity. So all those attributes plus those specific to CityPool will have values, while those of OlympicPool will be NULL to indicate that the pool is a city pool. This also happens when we want to know if a tuple represents an Olympic pool, where the attributes of CityPool will be NULL.

So if we implement the IS-A hierarchy with a single table, we will have the problem of distinguishing the types of pools – that is, knowing if a tuple represents an occurrence of the superclass entity or one of the inheriting entities. This could lead to a potentially large number of NULL values occupying unnecessary space in the table, even though working with such a table might be easy to understand.

Also, we can also consider the ease with which the schema can be extended or modified as an advantage. This is because if a foreign key is later added in our domain in any of the 3 tables of the hierarchy referencing another entity, it would simply be necessary to add a foreign key attribute to the Pool table. Similarly, if an external foreign key points to any of the entities in the hierarchy, it would only need to reference PoolID.

2 tables
To address the previous problem of distinction, another option we have for implementing the hierarchy is to use two tables, or as many as there are inheriting entities. The basis of this is that all inheriting entities have the same attributes as the superclass they inherit from, plus a series of specific attributes that characterize them.

So to logically distinguish the inheriting entities, we can implement specific tables for each one, where they have the same attributes as the superclass plus their own.

Relational schema where CityPool and OlympicPool inherit common attributes and add specific fields. Image by author. 

As you can see, in this option we implement the CityPool and OlympicPool tables, which are responsible for storing tuples that represent city pools and Olympic pools, respectively. Since each contains the same attributes as the subclass, even though they aren’t explicitly copied in the inheriting entities in the conceptual diagram, both have the same primary key, PoolID.

This implementation offers various advantages: first, we eliminate unnecessary NULL values used to distinguish pool types, at least those modeled through the inheriting entities. Also, the schema remains simple, being easily understandable and maintainable due to the semantics of each table.

But there is also a distinction problem here, as our hierarchy is not complete. This means that there will be pools that are neither city nor Olympic, so they can’t be represented with tuples from CityPool or OlympicPool. In other words, this option doesn’t work for representing incomplete hierarchies, as the only way we could represent a pool that is neither of these types would be to insert an identical tuple in both CityPool and OlympicPool with all attributes not belonging to the superclass set to NULL. But this would be very counterproductive in terms of memory usage, and would also be complicated to manage.

On the other hand, even if the hierarchy were complete, a possible disadvantage to consider is the repetition of the superclass attributes in all tables, where this repetition wastes space in our database.

But even if we have extra space and can afford to repeat all those attributes, if we want to gather all the data about all the pools (or individuals) that exist, we would need to collect the data present in all the tables, which may not be entirely efficient.

Lastly, if our conceptual model has a foreign key referencing the superclass entity Pool, we need to consider that the primary key of Pool has now been transferred to the two tables. This means that foreign key would have to reference both tables at once, which isn’t possible. So instead of referencing an attribute of one table, it would have to reference PoolID from both CityPool and OlympicPool at the same time. This would complicate or even make the implementation impossible at the logical level.

Regarding foreign keys, this option would indeed allow us to easily implement a foreign key in one of the entities, CityPool or OlympicPool, that references another entity (or even foreign keys that reference these entities in a straightforward manner).

Relational schema where OlympicPool inherits from Pool, adding competition and certification attributes. Image by author. 

But, if we insist on using two tables to implement the hierarchy, we could refine the logical schema to solve the problem of a foreign key referencing the superclass in this way.

As you can see above, we have two tables where one is exclusively dedicated to storing tuples that contain the attributes characterizing an Olympic pool. The other entity encompasses all pools, including city pools and Olympic pools. This is because an Olympic pool also inherits the attributes of the superclass, so to represent it in this schema, we create two tuples: one in Pool that stores the values of the superclass attributes, leaving the rest as NULL, and another tuple in OlympicPool that stores the remaining attributes, with its foreign key (which is also the primary key), referencing the corresponding tuple in Pool with the superclass attribute values.

The main advantage of this option is that it solves the problem of having an external foreign key referencing Pool – as in this case, it would simply need to point to the primary key {PoolID} of Pool, instead of several attributes at once as it did before.

But this leaves us with a significantly more complex schema to understand and work with, as the way to store a city pool is entirely different from storing an Olympic pool. This complicates certain operations like inserting an Olympic pool, where we’d need to create two tuples in Pool and OlympicPool so that the primary/foreign key of OlympicPool points to the tuple created in Pool. It also complicates counting the pools that are neither Olympic nor city pools in the system, where all those tuples in Pool with NULL in the attributes characterizing city pools must be found.

Finally, although we see that the primary key of OlympicPool is also foreign in this implementation, this doesn’t imply that conceptually it’s a weak entity in identification. There are many ways to implement the hierarchy, and this is not necessarily the one that must be carried out.

3 tables
So, if we have an incomplete hierarchy and really want to make sure that the implementation lets us distinguish between the different types of pools and identify those pools that don’t belong to any inheriting entity, we can use three tables – one for each entity, respectively.

Relational schema where CityPool and OlympicPool inherit from Pool, adding specific attributes. Image by author. 

The peculiarity of this schema is that since all pools contain the attributes of the superclass Pool, whenever there is a pool in our database, it will be represented by a tuple in the Pool table that contains only the values of the superclass attributes. And, if a pool is of a specific type, it will be represented not only by the Pool tuple but also by a tuple in one of the tables reserved for the inheriting entities, where each has a foreign key pointing to the primary key {PoolID} of Pool.

For example, a city pool can be represented by a tuple in CityPool (which only has the specific attributes that characterize it as a city pool) plus a foreign key pointing to a specific tuple in Pool that holds the values of the other inherited attributes.

The advantage of this schema is that it minimizes wasted space from duplicated information or the appearance of NULL values (as the only thing being "duplicated" here is the PoolID attribute as foreign keys in the inheriting entities). It’s also easy to understand because each entity is represented by a specific table at the logical level.

Also, the schema is easy to modify in cases where we need to add foreign keys to the entities, where it would simply require adding an additional foreign key attribute or implementing external foreign keys pointing to the entities themselves, which we can do by referencing their own primary and foreign keys {PoolID}.

If we add a new type of pool to the domain later, it’s easy to add a table very similar to the ones we already have. This is unlike the previous options we saw where adding a new type of pool would be more costly because of the elements that need modification. Also, having three tables makes it easy to model the constraints related to the completeness and disjunction of the hierarchy.

But this schema also presents certain problems. On one hand, if we have a city pool and want to know its name, we’ll need to access the Pool table to find its name, plus the CityPool table. This complicates the query and affects its efficiency and latency.

Aside from this, if we have a tuple from Pool and want to know if it’s a city pool, an Olympic pool, or neither, we’ll have to go through all the tuples in CityPool and OlympicPool to see if the foreign key of any of them points to the Pool tuple we are trying to identify.

Also, the presence of three tables is more complex than having just one or two, making the logical model somewhat more complicated to operate because there are more tables and more relationships between them.

When to Model Each Entity as a Table
These alternatives aren’t the only options for implementing an IS-A hierarchy at the logical level. Depending on the domain needs and requirements, we can choose other more appropriate schemas that are similar to those we’ve already discussed.

To summarize which is the best schema we can implement to model an IS-A hierarchy at the logical level, we need to know when it’s appropriate to introduce a table for each entity.

First, we have the superclass. This is useful to model with a specific table in cases where the hierarchy is incomplete, as in the example hierarchy. We saw that without a dedicated Pool table to represent occurrences of the superclass entity, it’s difficult to distinguish when a pool is of the generic type of the superclass or is instead of a specific type (like that of the inheriting entities). It’s also helpful to implement a table for the superclass when there is a foreign key pointing to the superclass entity itself. Otherwise, it’s very likely that we’ll have trouble knowing which attribute the foreign key should reference, as we saw before.

And to finish with the superclass, we should also implement a table for it when the hierarchy is non-disjoint or overlapping. For example, if a pool could be of several types at once and we didn't have the Pool table, we would be forced to duplicate information in specific tables for the inheriting entities. This would greatly complicate database operations.

So, with our Pool table, we can have tuples in the respective tables of the inheriting entities, all with their foreign keys pointing to the same Pool tuple, which simplifies queries.

If we have a Pool table where all existing pools are stored, it’s likely that we would want to efficiently know the type of a pool from a Pool tuple. Instead of having to go through all the tuples of the inheriting entities' tables, we can add an attribute in Pool that determines its type (if it has one). Or that will be NULL if the hierarchy is incomplete and doesn't belong to any type.

This is called an explicit discriminator. If there isn't one, we typically say that there is an implicit discriminator. These are the foreign keys of the other tables that we would have to go through to find out the type.

Regarding the inheriting entities, we should create a table for each one when they have many attributes, which would result in many NULL values if we were to implement this with just one or two tables. Besides the attributes, the inheriting entities themselves may have specific domain constraints that are greatly simplified at the logical level if we implement tables for each of them. This would avoid the need to apply constraints on just one or two tables, complicating the semantics of the constraints.

In short, the more entities we combine into a single table, the more NULL values we will encounter, since to distinguish them, the table attributes that do not correspond to the concept or entity we want to represent must be NULL, as if they don’t exist.

This would also complicate database operations, as operations would need to consider which attributes should or should not be NULL – as well as the constraints – which must account for the presence of NULL values to be verified.

On the other hand, if we know the hierarchy is complete, then instead of implementing a table for the superclass, we can decide to implement tables for each inheriting entity, where each one has the attributes of the superclass. But this option loses its purpose when we have a superclass with too many attributes, which would be repeated in several tables, potentially many.

Chapter 7: Normalization
When trying to translate an IS-A hierarchy to the logical level, it's very likely that we’ll end up with a design that exhibits redundancy. This is because the same information, such as that of the superclass, can end up being stored in multiple places. This poses multiple problems in a database. So to understand it, let's consider the following example:

PersonID	PersonName	CityID	CityName
1	Alice Johnson	1	Madrid
2	Bob Smith	5	Paris
3	Carol Davis	3	New York
4	David Brown	1	Madrid
5	Emily Wilson	5	Paris
Here we have a Person table that stores data about people – specifically their ID, name, and the city where they live. But to represent the city, all the attributes that our database stores about cities are included in the Person table itself. This makes it so that in one row we can know information about the person as well as information about the city where they live.

At first glance, this may seem convenient, since if we have a Person row, we not only have all the information about the person but also all the information about the respective city. This then lets us avoid having to look up this information in other tables. But this creates a significant redundancy problem.

According to the definition of redundancy, it means that the same information is stored in multiple places, that is, repeated unnecessarily. And this doesn't mean the information has to be in different tables. For example, in this example we have redundancy because the same city information can be stored multiple times in the same Person table (as is the case with the city "Paris" or "Madrid").

This actually leads to problems when inserting new cities into the database. If we only store them in this table but don't have any person living in the city we want to insert, we won't be able to insert it unless we do so in a row of the Person table with the rest of the attributes that don't characterize a city set to NULL. And this will greatly complicate database operations.

Redundancy also poses a problem for memory consumption, as duplicating all the information of a city for each person living there uses up unnecessary space. Similarly, if we want to delete a city or update its information, we have to do it for every instance where that information is repeated. This complicates operations and making them much less efficient.

For example, if we store a Population attribute in this table to represent each city's population, every time we update the population of a certain city, we have to do it for all the Person tuples. This becomes inefficient if many people live in that city, as we have to change the population data in all the tuples representing those people.

Just as it affects efficiency, redundancy also increases the chances of data inconsistency. If we forget to change one value when updating Population data, or if there's an error and a certain value in a tuple doesn't update, then that value will contradict the rest of the Population values in the repeated tuples for that city, causing an inconsistency.

To solve these types of situations, it's best to plan ahead by creating a good design at the conceptual level. We can try to separate concepts into entities that are distinct enough to avoid storing information about semantically different ideas in the same entity (as this could cause redundancy when moving to the logical level).

But if we reach the logical level with a certain diagram that we couldn't refine further at the conceptual level and we need to refine it, one of the transformations we can apply here is called decomposition.

Decomposition
PersonID	PersonName	CityID (FK)
1	Alice Johnson	1
2	Bob Smith	5
3	Carol Davis	3
4	David Brown	1
5	Emily Wilson	5
CityID	Name
1	Madrid
3	New York
5	Paris
Before looking into what decomposition involves, it's helpful to examine the specific problems of combining information about people and cities in a single entity.

For example, if we store many attributes of a city, a lot of space will be wasted if we have many people living in that city. This is because all the city's attribute values are unnecessarily repeated in all the tuples of the people living there.

Another potential problem related to memory waste could happen if we needed to insert a person into the database and we don’t know the city they live in. This would force us to leave all the city's attributes as NULL, wasting all the space those NULLs occupy.

Similarly, if we delete all the people living in "Madrid," for example, then our database will no longer contain any information about that city, as no one lives there. This means it isn't explicitly stored in the table. Lastly, we previously saw the problems that arose when updating the information of a certain city.

As a solution to these issues, we can apply decomposition. If you consider the example above, you may be able to see that this involves breaking down the Person table into several tables. On one hand, we keep the Person table dedicated to storing information about people. On the other, for the cities, we store all their information in a specific City table.

Once the information is separated into multiple tables, we can maintain the CityID (FK) attribute in the Person table, where it’s now converted into a foreign key that references the CityID of the new City table, indicating the city where the person lives.

As you can see in the example, decomposition involves replacing one table with two or more tables, each containing a subset of the attributes from the original. By combining them, we can retrieve the original attributes.

For instance, here we have split one table into two, where one retains all the attributes related to people and the other holds attributes related to cities. Together, these attributes form the original table we had. We do this mainly to solve problems caused by redundancy. Now, in the Person table, we only store an identifier for the city where the person lives, and in the City table, we store the city's information only once, allowing it to be used by more tables in the database.

But in order to do decomposition correctly, we must ensure that certain conditions are met. One is that the decomposition is lossless. This means that if we now take the two tables generated by the decomposition and combine all their information back into a single table, we should get the information we had in the original Person table before the decomposition.

So if we now take the resulting Person table and add the information provided by the tuple from the City table identified by the foreign key defined in the decomposition to each tuple, we should get the same information as we had in the original Person table before the decomposition – without losing any tuples or creating new ones.

This join operation easily shows that it returns the data we originally had before the decomposition. And this indicates that the decomposition was done without loss. But this doesn't guarantee it will be lossless for any possible tuple. To ensure this, we need to analyze the functional dependencies present among the table's attributes, which must also be preserved after decomposition.

Lastly, when performing the decomposition, we might receive queries in the database such as, given a person, obtaining information about the city they live in. To implement this query, we usually perform operations similar to the join we described earlier, which can be computationally expensive. So if it becomes so costly that it's impractical, we might consider not doing the decomposition. Or we could even doing a partial one, where we keep the city attributes that are queried most frequently in the Person table to make certain queries more efficient, even if some redundancy exists.

Functional dependency
Continuing with these conditions, to understand them correctly, you’ll need to know what functional dependencies are.

To introduce this concept, we can look at the simplest case, which is the attributes PersonID and PersonName of the Person table. These store a person's government identification number and their name, respectively. So if we find several tuples in the Person table with the same PersonID value, we would expect their respective PersonName values to also be the same. This is because if several tuples store information about people with the same ID, then they must necessarily be the same person (as we assume the government identification number is unique for each person).

So whenever there are several tuples with the same ID, we can say that the names of the people represented by those tuples must also be the same.

But the reverse does not have to be true, because if several different people have the same name, they will have the same name but different IDs. So if several tuples have the same PersonName, their respective PersonIDs do not have to match.

This situation we just saw is a case of functional dependency between PersonID and PersonName, specifically denoted as PersonID→PersonName, since it’s the PersonID attribute that uniquely determines the person's name.

PersonID	PersonName	CityID	CityName
1	Alice Johnson	1	Madrid
2	Bob Smith	5	Paris
3	Carol Davis	3	New York
4	David Brown	1	Madrid
5	Emily Wilson	5	Paris
Formally, we can define a functional dependency as a constraint or relationship that exists between two sets of attributes, such that the values taken by one set of attributes uniquely determine the values that the other set of attributes must take.

For example, using the same example without decomposing, we can see that there is a functional dependency between the set of attributes X={PersonID} and the set Y={PersonName}, denoted as X→Y. This means that for any pair of tuples in the table, if those tuples have the same values in the set of attributes X, then they must necessarily have the same values in the set of attributes Y.

But we don't discover this by simple observation. These dependencies are mainly given by the characteristics of the attributes and the domain we are modeling, as well as the requirements. That is, to discover these dependencies, we need to focus on the semantics of the attributes.

The formal definition of this concept states that a functional dependency is a relationship between sets of attributes, so they don't have to be single-attribute sets – they can contain any number of them, depending on the complexity of the dependency.

For example, if we assume that a person always lives in the same house and never moves, then we can say there is a functional dependency {PersonID}→{CityID}, as well as {PersonID}→{CityName}. This results in functional dependencies for all possible combinations of attributes on the right-hand side, which must take the same value for several tuples if they have the same value for the attributes on the left-hand side.

Specifically, this means that given the dependencies we know exist, the following also exist:

{PersonID}→{PersonName,CityID}

{PersonID}→{PersonName,CityName}

{PersonID}→{CityID,CityName}

{PersonID}→{CityID,CityName,PersonName}

This occurs due to the union property of functional dependencies, where if we have dependencies X→Y and X→Z, then the dependency X→(Y U Z) also exists, where the uppercase letters denote sets of attributes.

Without going into more detail about these properties, it's worth highlighting that this is one of Armstrong's inference rules, whose main purpose is to infer all the functional dependencies that exist in a table. Specifically, these inference rules ensure that, starting from a series of initial functional dependencies, all the dependencies that actually exist in a table can be inferred.

With this, the important thing to know is that functional dependencies can have multiple attributes in their sets. This in turn can lead to a classification of the dependencies based on the number of attributes they have in each set.

One of the main uses of functional dependencies is to determine if a decomposition is valid, meaning if all the functional dependencies are preserved.

For example, in the original table, there are the functional dependencies {PersonID}→{PersonName} and {PersonID}→{CityID}, primarily, or {CityID}→{CityName} because the identifier of a city uniquely determines the name of the city itself. So, considering these dependencies as a base, we can infer others like {PersonID}→{CityName} by transitivity using {PersonID}→{CityID} and {CityID}→{CityName}.

The ones we have considered as base are those generated directly by the domain's semantics. This means that if a city is uniquely identified by its CityID, then it doesn't make sense to consider {PersonID}→{CityName} as a base, since we have the other dependencies that relate the person's identifier with their name and city identifier, from which we can infer it.

In summary, the base dependencies are the most fundamental ones from which all others can be inferred. There is no single algorithm to find them all. Instead, it’s a more open process that we need to follow based on our domain, requirements, and the semantics of the attributes.

Once we’ve found the base dependencies, the important thing is to ensure that they are preserved after decomposing a table. We can see this in the resulting tables, where {PersonID}→{PersonName} remains in Person, as does {PersonID}→{CityID}, with the only peculiarity being that now CityID in Person is a foreign key, and {CityID}→{CityName}, which is preserved in the new City table after decomposition.

So by preserving all the base functional dependencies, we are assured that the decomposition of Person into Person and City is correct.

Finally, functional dependencies can have many more classifications besides being base or not. For example, in some of the formal definitions of the following normal forms that we’ll see, we often check if a functional dependency is trivial. This consists of those dependencies X→Y where all the attributes of set Y are present in set X.

For example, {A, B} → {A} is trivial because {A} ⊆ {A, B}, and {A, B} → {B, A} is also trivial because {B, A} ⊆ {A, B}. But {A} → {B} is not trivial because {B} ⊄ {A}, meaning there is an attribute in set {B} that is not present in set {A}.

Normal forms
After understanding what functional dependencies are, it's important to note that there are many other types of dependencies, such as multivalued, union, or inclusion dependencies. All of these also aim to eliminate or minimize the problems associated with data redundancy that we saw earlier through normal forms.

These are a series of refinement levels of a relational schema defined by increasingly strict conditions intended to eliminate or progressively minimize the issues caused by redundancy in a schema. Among all the levels, we will only look at those that use functional dependencies between attributes as criteria for their conditions. But there are others we won’t cover here whose criteria include multivalued or union dependencies.

1NF
First, we have the first normal form (1NF), whose main condition is that each attribute is atomic. This means that the table cells do not contain an arbitrary number of values, which we can also call the non-existence of repeating groups. But it also imposes basic conditions such as the requirement for a primary key in the table so that each tuple can be uniquely identified. This prohibits duplicate tuples, as well as attributes with duplicate names, meaning there can’t be columns with duplicate names.

These conditions must be met for all tables in a database schema to be in 1NF. In this case, we can easily verify them by ensuring that each cell contains exactly one value, that there are no duplicates in rows or columns, and that there is a primary key.

These last three conditions are allowed by a DBMS, which means that when implementing a table at the logical level, we can have duplicates or even not define any key – and although the database may function, its schema won’t be in 1NF. So if we find a table that does not meet the normal form conditions, we can apply certain transformations to normalize it and bring it to 1NF.

2NF
The first normal form focuses mainly on prohibiting repeating groups, which eliminates the possibility of redundancies at the cell level – but does not eliminate redundancies caused by functional dependencies.

Despite prohibiting the existence of duplicate tuples, we saw in a previous example that city information in a table could be unnecessarily duplicated in multiple different tuples because the people living in that city were different. This meets 1NF but presents redundancy problems.

To address these redundancy cases, we use the second normal form (2NF). It includes all the conditions of 1NF plus an additional stricter condition: all attributes that aren’t the primary key of a table must depend on the entire selected primary key for the table – meaning all its attributes, not just one. This prevents partial dependency on the primary key.

BikeID	Model	Brand	BrandCountry	PurchasePrice	OwnerName	OwnerEmail
1	Roadster	SpeedX	USA	1200	John Doe	john@example.com
2	TrailBlazer	MountainCo	Canada	1500	Alice Smith	alice@example.com
3	Roadster	SpeedX	USA	1150	Bob Lee	bob@example.org
4	CityCruiser	UrbanRide	USA	800	John Doe	john@example.com
5	EcoCruiser	GreenMotion	Germany	1300	Carol Johnson	carol@example.com
For example, here we have a Bike table whose primary key is {BikeID}, and the basic functional dependencies are {BikeID}→{Model}, {BikeID}→{PurchasePrice}, {BikeID}→{OwnerName}, {BikeID}→{OwnerEmail} because if BikeID uniquely identifies each bike, then the information about the model, price, and owner will directly depend on that attribute.

We also have the dependencies {Model}→{Brand}, {Model}→{BrandCountry}, and {OwnerEmail}→{OwnerName}, since knowing the bike model can uniquely determine its brand. We can also determine the owner's name from their email, which we can’t do in reverse because multiple people can have the same name and different email addresses.

Given these dependencies, since the primary key has only one attribute, we see that all others have a dependency on the entire primary key. This means that the primary key itself uniquely determines the rest of the table's attributes. So we can formally denote that, for all attributes A that aren’t the primary key, there’s the functional dependency {Primary Key}→A.

In this case, even though some dependencies are transitive, we can see that in the end, all attributes end up depending on the primary key. For example, with {BikeID}→{Model} and {Model}→{Brand}, we infer the dependency {BikeID}→{Brand}, which is not basic.

When this condition is met, the table is in 2NF, which avoids redundancies caused by attributes that depend only on part of the primary key, not the whole key.

This might not be as clear here because the primary key in the example has only one attribute, but sometimes we have primary keys with more attributes. In such cases, the rest of the table's attributes must depend on all the attributes in the primary key in order to be in 2NF (in addition to meeting the conditions of 1NF).

If they depend only on part of the key, there could be repeated values in those attributes. This would cause redundancy issues because it’s the entire primary key (all its attributes) that can uniquely identify each tuple.

3NF
Continuing with normal forms, 3NF is defined similarly. First, for a schema to be in 3NF, it must meet all the conditions of 2NF plus a specific one that states there can’t be functional dependencies between non-prime attributes.

Prime attributes are those that belong to any candidate key of the table. So we can restate the previous condition of 3NF by saying that no attribute that does not belong to any candidate key can functionally depend on any other attribute that does not belong to any candidate key.

For example, in the Bike table we had earlier, we assume that the only candidate key that exists is {BikeID}, since no other set of attributes can uniquely identify the tuples in the table. We can verify this by looking at the semantics of the attributes. So, seeing that there are functional dependencies like {Model}→{BrandCountry} between non-prime attributes, meaning they do not belong to any candidate key, we conclude that the table is not in 3NF, and we’ll need to normalize it.

BikeID	Model (FK)	PurchasePrice	OwnerEmail (FK)
1	Roadster	1200	john@example.com
2	TrailBlazer	1500	alice@example.com
3	Roadster	1150	bob@example.org
4	CityCruiser	800	john@example.com
5	EcoCruiser	1300	carol@example.com
Model	Brand	BrandCountry
Roadster	SpeedX	USA
TrailBlazer	MountainCo	Canada
Roadster	SpeedX	USA
CityCruiser	UrbanRide	USA
EcoCruiser	GreenMotion	Germany
OwnerEmail	OwnerName
john@example.com	John Doe
alice@example.com	Alice Smith
bob@example.org	Bob Lee
john@example.com	John Doe
carol@example.com	Carol Johnson
To normalize the table, we’ll need to apply an algorithm to the tables to convert the schema to 3NF, ensuring there are no functional dependencies between non-prime attributes.

To understand this algorithm, we’ll start with the original Bike table we had before. We’ll on the functional dependencies between prime attributes that break 3NF, that aren’t derived transitively from simpler ones, and whose set of attributes on the left side does not form a superkey.

For example, if we have {A}→{B}, {B}→{C}, and {A}→{C}, we do not consider {A}→{C} since it can be derived transitively from the other two. Specifically, the problematic ones in our example, which aren’t derived transitively and whose left side is not a superkey, are {Model}→{Brand}, {Model}→{BrandCountry}, and {OwnerEmail}→{OwnerName}, which are the base functional dependencies.

Now, we need to decompose the table guided by these functional dependencies. But as you can see, we can apply the union property of functional dependencies to know that the functional dependency {Model}→{Brand, BrandCountry} also exists. We derived it from the previous problematic ones to simplify the application of the algorithm.

In short, to make the algorithm easier to apply, whenever we see multiple functional dependencies with the same determinant (set of attributes on the left side), it’s useful to apply the union property mentioned earlier to simplify them into one.

So now we have that the problematic functional dependencies are {Model}→{Brand, BrandCountry} and {OwnerEmail}→{OwnerName}. We can create a specific table for each of them where its schema is made up of all the attributes of the dependency – that is, all the attributes on both sides. We can formally denote this as the union of both sets of attributes.

As you might guess, by doing this, the primary keys in the new tables will be the attributes of the determinants of these dependencies (which in this case are {Model} and {OwnerEmail}, respectively).

We also need to remove these attributes that we have separated into additional tables from the original Bike table, leaving only the attributes of the determinants of these dependencies and converting them into foreign keys to reference the corresponding primary keys of the new tables. By convention, the attributes that make up the primary key of a table are usually placed first on the left, like Model and OwnerEmail here.

After this process, we can see that all the functional dependencies that were previously problematic are now in new tables where their determinants are now primary keys. This avoids violating the condition imposed by 3NF.

Note that after applying this algorithm, we don’t need to apply it recursively to the tables generated by the decomposition, as there is a guarantee that the resulting schema is already in 3NF after applying this process. In summary, by applying this normal form to our schema using the described algorithm, known as the relational synthesis algorithm, we manage to avoid or minimize the occurrence of redundancies caused by transitive functional dependencies.

BCNF
The three previous normal forms are the most basic ones we can apply to a schema to eliminate most problems caused by redundancies. But there is another normal form in addition to 3NF that is more restrictive and ensures a better result in this regard, which is BCNF.

As we’ve seen, the normal forms become increasingly restrictive in the conditions they apply. In this case, BCNF stands for Boyce-Codd Normal Form, and it’s characterized by allowing only those functional dependencies X→Y in the tables where it’s true that either the dependency is trivial or X is a superkey of the table.

If these conditions are met, we can formally demonstrate that all the conditions of 3NF must also be automatically met (and so also 2NF and 1NF). We won’t perform this demonstration here, as the important thing is to know how to normalize a schema to adhere to the BCNF. So if we start with a schema like the one we originally had for the unnormalized Bike table, we can apply a specific algorithm to transform it to BCNF.

BikeID	Model	Brand	BrandCountry	PurchasePrice	OwnerName	OwnerEmail
1	Roadster	SpeedX	USA	1200	John Doe	john@example.com
2	TrailBlazer	MountainCo	Canada	1500	Alice Smith	alice@example.com
3	Roadster	SpeedX	USA	1150	Bob Lee	bob@example.org
4	CityCruiser	UrbanRide	USA	800	John Doe	john@example.com
5	EcoCruiser	GreenMotion	Germany	1300	Carol Johnson	carol@example.com
The algorithm to convert to BCNF is very similar to the one we looked at for 3NF. The difference is that here, the decomposition is done in more steps.

First, we need to identify the functional dependencies that prevent compliance with BCNF, which are exactly {Model}→{Brand}, {Model}→{BrandCountry}, and {OwnerEmail}→{OwnerName}. We choose these because, as you can see, {Model} can’t be a superkey, nor can {OwnerEmail} on its own. But in other functional dependencies like {BikeID}→{PurchasePrice}, we see that {BikeID} is indeed a superkey, as it’s actually the primary key of the table. So we don’t include those when applying the algorithm.

Also, keep in mind that a functional dependency X→Y can be trivial and meet the definition of BCNF even if X is not a superkey, meaning that the set of attributes Y is a subset of the set of attributes X.

Now, to simplify the application of the algorithm, we can focus on the determinant of the dependencies that break the normal form – that is, on the set of attributes on the left side, looking for several that have the same determinant. If there are several with the same determinant, as is the case with those that have {Model} on their left side, then we can use the union property of Armstrong's inference rules to simplify them all into one like {Model}→{Brand,BrandCountry}. Here', on the right side, we have gathered all the attributes from the right sides of the dependencies we had.

In this way, we reduce the number of dependencies to consider in the algorithm which simplifies its execution. This is the case since this step is not mandatory in this algorithm (nor in the conversion to 3NF), as it’s not part of the algorithm's definition itself, but rather something additional we do to simplify it without affecting its correctness.

Afterward, we end up with the dependencies {Model}→{Brand,BrandCountry} and {OwnerEmail}→{OwnerName}, which guide the decomposition we will perform on the table, similar to the 3NF conversion algorithm. But the main difference is that now we select the dependencies one by one and perform a decomposition for each, not all at once. Each time the table is decomposed, the dependencies and keys change, so we have to do it one by one to ensure that the recombination of the decomposed tables remains lossless.

Although we won't go into detail about why this happens, the important thing to remember is that we use this method because this algorithm doesn’t guarantee the preservation of all functional dependencies due to the conditions that define this normal form. These conditions are restrictive enough that, in certain situations, some dependencies may not be preserved after decomposition.

When selecting one of the dependencies like {Model}→{Brand,BrandCountry} (we can actually choose any of them), we decompose the Bike table guided by this functional dependency. We remove all the attributes on the right side of the dependency from the original table and make the attributes of the determinant (left side) foreign keys. These foreign keys point to the corresponding attributes of a new table where we store all the attributes involved in the dependency (meaning from both sides).

BikeID	Model (FK)	PurchasePrice	OwnerName	OwnerEmail
1	Roadster	1200	John Doe	john@example.com
2	TrailBlazer	1500	Alice Smith	alice@example.com
3	Roadster	1150	Bob Lee	bob@example.org
4	CityCruiser	800	John Doe	john@example.com
5	EcoCruiser	1300	Carol Johnson	carol@example.com
Model	Brand	BrandCountry
Roadster	SpeedX	USA
TrailBlazer	MountainCo	Canada
Roadster	SpeedX	USA
CityCruiser	UrbanRide	USA
EcoCruiser	GreenMotion	Germany
Formally, if our original table is the set of attributes R, then we keep R-{Brand,BrandCountry}, convert {Model} into the foreign key {Model (FK)} referencing the set of attributes {Model} of the new table generated by the decomposition, whose attributes are given by {Model}U{Brand,BrandCountry}, and whose primary key is the set {Model} that was previously in the determinant of the dependency.

Now, we repeat this process recursively on the resulting tables, as this decomposition has solved the problem caused by the dependency {Model}→{Brand,BrandCountry}. But we still have the dependency {OwnerEmail}→{OwnerName} in the Bike table. So we apply another decomposition step guided by the only remaining dependency that violates the BCNF conditions.

By doing this, we remove the set of attributes {OwnerName} from the Bike table and convert {OwnerEmail} into a foreign key that references the same set {OwnerEmail} but from the new table generated by the decomposition. In this case it’s formed by the attributes {OwnerEmail}U{OwnerName}={OwnerEmail,OwnerName}.

BikeID	Model (FK)	PurchasePrice	OwnerEmail (FK)
1	Roadster	1200	john@example.com
2	TrailBlazer	1500	alice@example.com
3	Roadster	1150	bob@example.org
4	CityCruiser	800	john@example.com
5	EcoCruiser	1300	carol@example.com
Model	Brand	BrandCountry
Roadster	SpeedX	USA
TrailBlazer	MountainCo	Canada
Roadster	SpeedX	USA
CityCruiser	UrbanRide	USA
EcoCruiser	GreenMotion	Germany
OwnerEmail	OwnerName
john@example.com	John Doe
alice@example.com	Alice Smith
bob@example.org	Bob Lee
john@example.com	John Doe
carol@example.com	Carol Johnson
As you can see, after these steps, the schema doesn’t have any functional dependency X→Y where X is not a superkey or the dependency itself is trivial. This is because when decomposing into tables, we define their primary keys as the determinants of the dependencies that originally did not comply with the normal form.

So after performing these steps for all dependencies that prevent the schema from adhering to BCNF, we end up with a normalized schema that does comply with BCNF. During the process, it’s possible that some of the generated tables still have functional dependencies that violate BCNF, which is why these steps are applied recursively. This means that decomposition is not only done from the original table, but it may also be necessary to decompose a table generated by previous steps, especially in more complex schemas.

In the example we have, the final schema that meets the BCNF conditions is exactly the same as the one we got when transforming it to BCNF. But this is a coincidence – in most practical cases, schemas tend to be more complex, and after converting them to 3NF, they may not comply with BCNF, or it may even be impossible to convert them to BCNF. That is, converting a schema to 3NF is always guaranteed to be possible, while there is no such guarantee for BCNF.

In short, BCNF is more restrictive than 3NF, which prevents redundancies caused by functional dependencies where a set of attributes that do not uniquely identify the tuples of a table determine the values of another set of attributes. This makes the information of the determining attributes redundant, similar to what happens in 3NF with transitive dependencies.

Also, being more restrictive, it may not be achievable if a table has multiple overlapping superkeys, as applying the BCNF decomposition algorithm would break the functional dependencies between attributes of different superkeys. So by relaxing the conditions of BCNF, we get 3NF, which correctly handles situations where overlapping superkeys exist, meaning they share some attribute.

Other normal forms
Besides the normal forms based on functional dependencies, which we have just seen, there are others that eliminate redundancies caused by different types of relationships between attributes or characteristics.

For example, 4NF deals with multivalued dependencies, 5NF with join dependencies, 6NF represents the highest level of normalization of a relational schema, and DKNF (Domain–Key Normal Form) also imposes the condition that all schema constraints must result solely from domain and key definitions, meaning it only allows domain and key constraints.

When to check compliance with each normal form?
Lastly, we’ve seen that each normal form establishes a series of characteristics that a database schema has to follow and the problems it aims to solve.

Practically speaking, the most important normal forms we need to ensure for almost any schema are 1NF and 2NF. In the case of 1NF, most DBMSs guarantee it automatically – but we have to design the conceptual model so that it avoids the appearance of repeating groups that don’t meet the conditions of 1NF. On the other hand, 2NF is essential for identifying tuples in tables, so we should make sure it’s met in a real project database.

Beyond these, if we’re working with a system that performs analytical queries like in OLTP, the database schema should also meet the conditions of 3NF, especially when the schema needs to handle queries or undergo updates frequently. This helps resolve these queries and updates as efficiently as possible.

Beyond 3NF, we’ll want to meet BCNF when business rules are very complex. That is, when data has to meet complex constraints, we can help minimize the impact of redundancy issues through BCNF conditions, as they are more restrictive than those of 3NF. Then, if our schema allows multivalued attributes or associations of degree higher than 2, it may be useful to check other types of normal forms like 4NF, 5NF, and so on.

Chapter 8: Query Languages
At this point, you’ve learned about all the elements with which we can organize or structure stored data in relational databases using the relational model. But in practice, we don’t only want to store data, as we could do that with simple files. We also need tools to manipulate and query these data. This means we need to use a query language.

In simple terms, query languages are designed to manipulate and query (or access) the data stored in a database through a set of operations. Querying is the most fundamental operation of all, because if we think about how some of the other operations work (like updating or deleting data, for example), we need to be able to select or query the data in order to perform any operations on them. So basically, almost any modification starts by first identifying which records will be affected by the operation.

The query languages we’ll learn about here are relational, meaning they are created to manipulate and query data in relational databases. Fundamentally, most of them base the logic of operations on table manipulations that result in another table. Then we can continue applying operations to that resulting table. So when we operate on a relational database, we are transforming tables into other tables until we reach a table with data that interests us.

Formal vs practical query languages
There are some query languages known as formal languages, which consist of theoretical definitions where operators or transformations that can be applied to tables are formally defined. This also helps optimize operations on them significantly, as these formal tools allow us to verify equivalences between operations or queries, enabling us to choose the one with the least computational cost among several equivalents.

On the other hand, to apply this to a database, there are practical query languages like SQL, which are implementations of formal query languages adapted to be used on real systems.

Although we call them languages, it's important not to confuse them with general-purpose languages. Query languages, as their name suggests, are dedicated to manipulating and querying data, not performing any type of computation. Examples of formal query languages include:

Relational algebra
This is a formal imperative language, which means that when we program in it, we must think about how to obtain the result we want. In other words, we define a sequence of operations using the language's operators that progressively transform the tables until we reach one or more resulting tables with the data we need.

This idea of a sequence of operations is very similar to how we’d actuall plan and execute a query in a practical query language like SQL. This, along with the similarity of formal operators to the statements offered by these practical languages, helps the end user optimize the query, verify its correctness formally, or demonstrate its equivalence with another query that requires fewer computational resources, among other uses.

Example: If we want to get all the ages from a Person table that are greater than 50, we can apply the relational algebra operators π Age ( σ (Age > 50) (Person) ) that we will see later. First, we filter all tuples that meet the condition of having an age >50 using the corresponding operator, and then we apply another operator to the resulting table with those tuples to keep only the ages of those tuples.
Relational calculus
Unlike the relational algebra, relational calculus is a declarative language. This means we program by thinking about the properties the result must have, not about which operators to apply to certain tables to achieve it. In other words, we don’t define something similar to an execution plan or sequence of operators to get the result. Instead, we simply declare the properties it must have to meet our needs, and the system itself finds an execution plan that produces exactly what we are looking for.

There are several ways to pose a query or modification on the data. One is based on Tuple Relational Calculus (TRC), where we declare conditions that the attributes of the tuples must meet to be included in our result. The other is Domain Relational Calculus (DRC), which involves using variables over the domains of the attributes to set conditions on them using a methodology similar to first-order logic.

Example: Following the same example as before, in TRC we would have something like { t.Age | Person(t) ∧ t.Age > 50 }, where we declare that the tuples t we want to obtain must belong to the Person table and have a value greater than 50 in the Age attribute. Meanwhile, in DRC we would have { ⟨a⟩ | ∃id ( Person(id, a) ∧ a > 50) }, where we are assuming that the table only stores an ID attribute and an Age attribute, because if more were stored, we would have to use more domain variables. In summary, here the conditions are imposed on the domain variables, which represent the values that the tuples take in their respective attributes.
Lastly, regardless of the formal language used, both have the same expressive capacity, which can be formally demonstrated, as both are constructed using first-order logic.

Chapter 9: SQL (Structured Query Language)
In addition to formal languages, there are implementations like Structured Query Language (SQL) that are based on the operations of these formal languages. They allow us to manipulate and query data through relational database management systems (DBMS).

Specifically, SQL is a commercially used language with various standards, to which various functionalities have been added over time. Most systems have versions installed that are newer than SQL-92. But that version already includes all the necessary functions to perform the vast majority of operations needed on a database, so it’s the standard we’ll explore here.

SQL is a declarative language, where we define what data we want to get, not the exact sequence of operations to get it. The DBMS does the latter internally by translating the statements we write into relational algebra operations, which transform the tables through an execution plan until reaching the final resulting table.

Before proceeding with the elements that make up the SQL language itself, we should distinguish these elements or statements based on their purpose or application area.

On one hand, we have the statements that form the Data Definition Language (DDL), which is a set of statements dedicated to managing the tables in the database (such as their creation, deletion, modification, and so on).

Then we have the Data Control Language (DCL), which is another set of language statements dedicated to controlling user permissions in the database, managing who can read or modify the tables.

On the other hand, we have the Data Manipulation Language (DML). Its statements are oriented towards managing the data contained in the tables, such as insertion, deletion, transformation, or querying.

Apart from these sets of statements or instructions, we can also consider the Transaction Control Language (TCL), which are a series of statements that allow us to manage transactions that occur in the database. Here, we will focus only on the first three sets, which contain the most fundamental instructions.

DDL
To start with SQL, the most basic thing we can do is create, modify, and delete tables in the database. This means that we use instructions that allow us to define our logical design in the DBMS.

Here, we’ll use PostgreSQL as the DBMS, although these examples can be applied to any DBMS that supports the SQL-92 standard, which is what we will focus on.

Entity-relationship diagram where Rental is a weak entity that connects Person and Bike with rental data. Image by author. 

For these examples, let's assume a domain where people rent bicycles. We have a weak entity called Rental that models when a person has rented a certain bike, and the attribute Duration represents the number of rental days.

As we have seen in previous examples, the primary key of Rental is composed of the rental date and the foreign keys that identify the person who rented a certain bike. This makes Rental weak in identification because both foreign keys are needed to uniquely identify the tuples in that table.

Also, in our domain, we prohibit a person from renting a bike when it’s already being rented by someone else. This means that although everyone can rent as many bikes as they want, they can’t rent one that is already being used by another person or by themselves.

Relational schema diagram where Rental references Bike and Person through foreign keys. Image by author. 

When translating it to the logical level, we simply underline the attributes that are keys and add the foreign key attributes in Rental, as they aren’t represented at the conceptual level. These are underlined because the Rental entity is weak in identification, as we just discussed.

So if we want to implement this logical design in a DBMS like PostgreSQL, we first need to install and configure it on a machine. Then, once we’ve opened it in a terminal, we can navigate it with different commands:

\? Shows help about the DBMS commands.

\! [command] Executes an operating system terminal command.

\h [command] Shows help about the SQL syntax, that is, its statements like \h CREATE TABLE.

\q Closes the DBMS, which can also be done with exit.

\l or \list Lists all available databases.

\c <databaseName> or \connect <databaseName> Connects to the database with the given name.

\conninfo Shows information about the current database connection (host, port, user, database).

\dn Lists all schemas, which are groupings of elements like tables, views, types, and so on.

\dt Shows the tables of the database we are connected to.

\dv Similar to the previous command, this one shows the views.

\di Lists the indexes.

\df Lists the functions.

\d[+] object Describes the object whose name we provide as an input argument (table, view, function, and so on). With + it includes additional details.

SQL CODE; In the terminal, we can execute SQL code, always ending with a semicolon.

\timing Used to turn on/off the measurement of query execution time.

\copy table TO 'file.csv' CSV HEADER; Exports table to a CSV file.

\copy table FROM 'file.csv' CSV HEADER; Imports data from a CSV file into a table without emptying it first.

\i path/file.sql Executes an SQL script saved in a file with a .sql extension to avoid having to copy and paste lengthy SQL code into the terminal.

When you’re using a DBMS, you should check its documentation to see if it’s case sensitive or not. In this case, PostgreSQL is not case sensitive unless we write the identifiers of the elements in quotes (except for the SQL clauses, which are never interpreted as case sensitive). This means we can write SQL statements in either uppercase or lowercase, interchangeably.

CREATE
Once we have entered the DBMS, the first thing we can do is create elements using the CREATE statement. There are many elements we can create, but the most important one for now is DATABASE, which allows us to create a new database.

CREATE DATABASE sampledb;
If we enter this command directly into the terminal, we will create an empty database that we can connect to using the previous PostgreSQL commands. Once we are in the database, we can create the tables of our logical design with the following:

CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255),
    Birth DATE,
    Email VARCHAR(255)
);
CREATE TABLE Bike (
    BikeID INT,
    Model VARCHAR(255),
    Weight DOUBLE PRECISION
);
CREATE TABLE Rental (
    PersonFK INT,
    BikeFK INT,
    RentalDate DATE,
    Duration INT,
    Price DOUBLE PRECISION
); --Important, don't forget the ; after each statement--
As you can see, when creating tables, you need to specify the schema for each one. Don’t confuse this with what PostgreSQL calls a schema at the DBMS level. In PostgreSQL, a schema is a namespace within the database that groups and isolates elements like tables, views, functions, and so on. This makes it easier to organize, manage, and control permissions, and avoid name conflicts.

Here, the table schema refers to the attributes that define it, which is why we declare their names along with their data types, including:

Data Type	Category	Description
BIT	Exact Numeric	Fixed-size bit string (for example, BIT(1) stores a single 0 or 1).
TINYINT	Exact Numeric	Integer typically from –128 to 127 (1 byte).
SMALLINT	Exact Numeric	Integer typically from –32,768 to 32,767 (2 bytes).
INTEGER / INT	Exact Numeric	Integer typically from –2,147,483,648 to 2,147,483,647 (4 bytes).
BIGINT	Exact Numeric	Integer typically from –9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (8 bytes).
DECIMAL(p, s)	Exact Numeric	Fixed-point number with precision p and scale s (for example, money).
NUMERIC(p, s)	Exact Numeric	Synonym for DECIMAL, same fixed-point behavior.
FLOAT(p)	Approximate Numeric	Floating-point with precision of at least p bits.
REAL	Approximate Numeric	Single-precision (typically 24-bit) floating-point.
DOUBLE PRECISION	Approximate Numeric	Double-precision (typically 53-bit) floating-point.
CHAR(n)	Character String	Fixed-length text of exactly n characters (padded if shorter).
VARCHAR(n)	Character String	Variable-length text up to n characters (no padding).
CLOB	Character String	Character Large Object for very long text (for example, articles).
BINARY(n)	Binary String	Fixed-length binary data of exactly n bytes.
VARBINARY(n)	Binary String	Variable-length binary data up to n bytes.
BLOB	Binary String	Binary Large Object for large binary data (for example, images).
DATE	Date/Time	Calendar date in YYYY-MM-DD format.
TIME(p)	Date/Time	Time of day HH:MM:SS[.fraction] with p fractional seconds.
TIMESTAMP(p)	Date/Time	Combined date and time with fractional seconds precision p.
INTERVAL	Date/Time	Period of time (for example, INTERVAL '1-2' YEAR TO MONTH).
BOOLEAN	Boolean	Logical value TRUE, FALSE, or UNKNOWN (NULL).
XML	Other Standard	Stores XML document or fragment.
JSON	Other Standard	Stores JSON-formatted text for semi-structured data.
UUID	Other Standard	128-bit universally unique identifier (for example, 550e8400-e29b-41d4-a716-446655440000).
In this list, we can see some like BLOB that at first glance allow storing an arbitrary amount of data in a single cell, as the BLOB can be as large as we want. This might seem like it poses a repetitive group problem. But when a column stores BLOB data, it doesn't store multiple BLOBs in the same cell, but only one. This makes the DBMS responsible for managing the disk storage of this type of data efficiently.

In other words, we can see this as if the BLOB itself is not stored in a table cell, but rather a memory pointer is stored pointing to another memory area where the entire BLOB is stored (although the exact technique used heavily depends on the DBMS).

Also, if we look at other data types like VARCHAR for storing text, we’ll see that it’s essential to specify a fixed size for the data type, so that all texts stored in that column can have an arbitrary length that does not exceed the defined size in the data type.

Besides creating databases and tables, we might want to create a custom data type like ageDataType or colorDataType, which we can do using CREATE DOMAIN.

CREATE DOMAIN ageDataType AS INTEGER CHECK (VALUE >= 0 AND VALUE <= 150);
CREATE DOMAIN colorDataType AS VARCHAR(8) CHECK (VALUE IN ('red', 'green', 'BlUe'));
Here we just created new data types called ageDataType and colorDataType, where the first one is used to represent ages and the other colors. We could do this by imposing constraints on the values that columns can take, rather than defining a new data type, or rather a domain. But if there are many attributes with the same constraints on their domain, meaning they have the same domain like color or age, then it makes sense to define a custom one.

We mainly do this using the CHECK statement, which as we'll see is used to define constraints (in this case on the values of the data type we define as a base when creating a new domain. Above we used INTEGER and VARCHAR(8) respectively.).

ALTER
In addition to creating elements like tables or databases, we can also modify them using the ALTER statement. For example, if we forgot to add the AuxEmail column to the Person table, we can use the following statement to add it after the table has been created.

ALTER TABLE Person
  ADD COLUMN AuxEmail VARCHAR(255);
As you can see, we first specify the table where we want to add the new column, and then we specify the name and type of that attribute. But it's important to consider the value assigned to its cells when this table extension occurs.

By default, SQL allows NULL values in the table, so it will fill those values with NULL if there is content in the table. But if we want to assign a custom default value to the cells of the new column instead of NULL when there is data already inserted in the table, we can add the default value property to the column we are adding:

ALTER TABLE Person
  ADD COLUMN AuxEmail VARCHAR(255) DEFAULT 'noEmail@gmail.com';
This way, when we insert a tuple and leave the AuxEmail value undefined, the DBMS will automatically fill the cell for that attribute with its default value. This also applies when adding the column itself when there is already data in the table. We can also remove this default value property using:

ALTER TABLE Person
  ALTER COLUMN Email DROP DEFAULT;
Similarly, ALTER also allows us to remove an attribute:

ALTER TABLE Person
  DROP COLUMN Email;
Change the data type of an attribute:

ALTER TABLE Person
  ALTER COLUMN Name TYPE CHAR(25);
And rename elements, among many other actions:

ALTER TABLE Person
  ALTER COLUMN Birth RENAME TO BirthDate; --Renames the column Birth of the table Person--
ALTER TABLE Person
  RENAME TO People; --Renames the table Person--
ALTER DATABASE sampledb
  RENAME TO otherName; --Renames the database sampledb--
In short, ALTER allows us to modify elements that have already been created in the database without deleting and recreating them with the changes. Otherwise, we would have to export the data stored in those elements and reinsert it into the new schemas, which would be inefficient.

DROP
We can remove elements with the DROP statement. Its operation is very simple, as we just need to specify the name of the element to remove, such as the database we just created:

DROP DATABASE samplebd;
When executing this statement, SQL tries to delete the database, although we might get an error if we are connected to it. Besides simply deleting it, we can check if it exists before trying to delete it with:

DROP DATABASE IF EXISTS sampledb;
Similarly, we can have a schema like this example where there are foreign keys in Rental that reference or point to other tables like Bike and Person.

If we delete Rental, nothing would happen since no foreign key points to Rental. But if we want to delete one of the other two tables, a referential integrity problem will arise. For example, deleting Bike would leave the foreign key reference in Rental that points to Bike orphaned. So to delete Bike and all the constraints or SQL elements that depend on Bike, meaning those that reference it, we can use CASCADE:

DROP TABLE Bike CASCADE;
By doing this, not only would Bike be deleted, but also the foreign key constraint in Rental that we haven't introduced yet, as well as all others that point to Bike.

It's important to note that the CASCADE in a DROP statement is not related to the CASCADE we can define in a CREATE statement to set deletion or insertion policies. If, instead of deleting an entire table, we only delete certain tuples, we might end up with a situation where a tuple has a foreign key value that doesn't correspond to any tuple in the referenced table because we deleted it. We can establish deletion policies where the tuples pointing to the deleted one are also removed, or similar actions.

INSERT
To insert tuples into tables, we use the INSERT statement, where we specify the name of the table where we want to insert, as well as the attributes of its schema and the values to insert into the new tuple.

INSERT INTO Person (PersonID, Name, Birth, Email)
VALUES (5, 'Carol Johnson', '1985-07-15', 'carol@example.com');

INSERT INTO Bike (BikeID, Model, Weight)
VALUES (5, 'EcoCruiser', 14.2);

INSERT INTO Rental (PersonFK, BikeFK, RentalDate, Duration, Price)
VALUES (5, 5, '2025-07-10', 3, 25.50);
But, if we don't have some of the values for the tuple, we can omit them by inserting values only for the attributes we do have. We can even insert a tuple with DEFAULT values for certain attributes. But this only works if a default value was defined when creating the table or added with an ALTER statement.

INSERT INTO Bike (BikeID, Model)
VALUES (6, 'Speedster');
INSERT INTO Bike (BikeID, Model, Weight)
VALUES (7, 'Commuter', DEFAULT);
DELETE
To delete tuples, you can use DELETE, which at a logical level is very similar to the SELECT clause that we will see later (that’s used to retrieve data in response to database queries).

To use DELETE, we impose a set of conditions that the tuples in one or more tables must meet to be selected. Those tuples that meet the conditions are then deleted by DELETE.

DELETE FROM Rental
WHERE PersonFK = 5
  AND BikeFK   = 5
  AND RentalDate = '2025-07-10';
For example, here all tuples with a value of 5 in PersonFK and BikeFK and a rental date of 2025-07-10 will be deleted.

UPDATE
Similarly, we can update the values of tuples using UPDATE. We first select the tuples that will be affected by the change we want to make by imposing conditions on them, and then we use SET to change one of their attribute values or apply a transformation.

UPDATE Bike
SET Weight = 13.8
WHERE BikeID = 5;

UPDATE Person
SET Email = 'carol.johnson@example.com'
WHERE PersonID = 5;
Constraints implementation
Given these DDL statements, we can create different elements where data is stored. But as we’ve seen, in most domains we model, we need to implement a series of constraints to ensure that the data adheres to the requirements of our problem. (This is in addition to the integrity constraints inherent in the relational model, such as the existence of keys.)

Although this distinction is not as strong in SQL, most constraints we impose help ensure data integrity, whether they refer to the relational model's own rules or the business rules of our problem.

To implement constraints in SQL, we can start with the simplest ones: constraints that affect a single table. These are usually implemented using the CHECK statement within another statement like CREATE TABLE, where a condition is specified that all tuples in a table must meet whenever we modify it by inserting, modifying, or deleting its tuples.

CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255),
    Birth DATE,
    Email VARCHAR(255),
    CHECK (Birth <= CURRENT_DATE)
);
CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255),
    Birth DATE,
    Email VARCHAR(255),
    CONSTRAINT BirthConstraint CHECK (Birth <= CURRENT_DATE)
);
For example, we can assume that a person's birth date is always validated before being saved in the database. If a user enters an invalid date in the application layer, the application itself will generate an error and prevent saving an invalid date in the database. But it's still a good idea to add this type of constraint to ensure data integrity.

In this case, a person can’t be born on a date later than the current date, which we can get in SQL with CURRENT_DATE. So, we define a constraint where the Birth attribute must be less than or equal to the current date for all rows in the Person table.

These constraints are usually defined below the attribute declaration, and we can also give them a specific name using CONSTRAINT. This declares the constraint and assigns it a name we can use to identify it. We can add this name not only to a CHECK constraint but also to any similar declaration, such as PRIMARY KEY, FOREIGN KEY, or UNIQUE, among others.

Continuing with constraints on a specific table, if we need to ensure that an attribute can’t take NULL values, we can use either a CHECK or a NOT NULL along with declaring the corresponding attribute (to which we can also give a specific name using CONSTRAINT).

CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255),
    Birth DATE NOT NULL,
    Email VARCHAR(255)
);
CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255),
    Birth DATE CONSTRAINT BirthNotNull NOT NULL,
    Email VARCHAR(255)
);
CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255),
    Birth DATE,
    Email VARCHAR(255),
    CONSTRAINT BirthNotNull CHECK (Birth IS NOT NULL)
);
These three ways are equivalent if we want to require people to save their birth date in the database, preventing NULL values in the respective column.

The main difference between using CHECK and putting NOT NULL next to the attribute declaration is that if we use CHECK, we have to write a condition in parentheses similar to how we do it in a SQL query that describes the condition we want to impose, as long as this query only affects the attributes of the table we are working on.

In contrast, NOT NULL next to an attribute is an implicit way to indicate this restriction. It's important to note that a SQL condition can involve complete subqueries, providing very useful computing power to express most restrictions.

After understanding what CHECK involves, we can see how almost any domain restriction on attributes can be specified in one of these statements. However, SQL offers us more functionalities, such as setting a default value for attributes with DEFAULT.

CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255) DEFAULT 'No name',
    Birth DATE,
    Email VARCHAR(255)
);
CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255) CONSTRAINT NameDefaultValue DEFAULT 'No name', --We can name the default value too--
    Birth DATE,
    Email VARCHAR(255)
);
As we’ve seen before, we use DEFAULT so that when a tuple is inserted with a missing value for a certain attribute, if that attribute has a default value defined, the tuple will be inserted with that default value in the corresponding attribute instead of NULL.

This is important because if we include the NOT NULL restriction and don’t define a default value for an attribute, the DBMS may generate an error here. This also applies when a new attribute is added to the table using ALTER, where we can define a default value at the same time.

CREATE TABLE Bike (
    BikeID INT,
    Model VARCHAR(255),
    Weight DOUBLE PRECISION,
    CONSTRAINT ModelValues CHECK (Model IN ('Model1', 'Model2', 'Model3'))
);
As a curiosity, if we want to explicitly define the possible values an attribute can take, we can use a CHECK like the one above. This is the same expression we use when creating a new domain with CREATE DOMAIN. We can then assign it as the data type to the Model attribute. So we have the option to create a custom domain for an attribute or define a constraint with CHECK to model its domain (although in most cases, it's better to use CREATE DOMAIN for better maintainability).

Continuing with constraints that affect a single table, we also have those more related to data integrity concerning the relational model. For example, to uniquely identify the tuples of a table, we have candidate keys in the relational model, which we can declare in SQL using UNIQUE in combination with NOT NULL.

CREATE TABLE Bike (
    BikeID INT,
    Model VARCHAR(255),
    Weight DOUBLE PRECISION,
    UNIQUE (Model)
);
For example, if we assume that in our problem there aren't multiple different bikes with the same model name, then we can use Model as a candidate key to uniquely identify all the tuples in the table.

So to explicitly declare that Model can serve for tuple identification, we use UNIQUE. This indicates that all the values that this attribute takes in (all the tuples of the table) must be different.

But UNIQUE allows the attribute to take the value NULL at most once – so if we want to ensure that we declare it as a candidate key, we might consider adding a NOT NULL constraint in case we don't accept NULL as a valid value for a candidate key.

We can also apply this to more than one attribute, where UNIQUE would determine that the combination of values of all those attributes included in the constraint must be different in all the tuples of the table.

The main usefulness of UNIQUE is that it ensures certain attributes meet the definition of a candidate key. So, if we insert multiple tuples with the same repeated values in attributes that form a candidate key defined with UNIQUE, the DBMS will generate an error. But beyond this, we don’t have to define all candidate keys that exist unless the domain or problem requirements force us to do so.

Usually, we’d just define the primary key of a table with PRIMARY KEY, without needing it to be a selected candidate key.

CREATE TABLE Person (
    PersonID INT,
    Name VARCHAR(255) DEFAULT 'No name',
    Birth DATE,
    Email VARCHAR(255),
    CONSTRAINT PersonPK PRIMARY KEY (PersonID) --The constraint is named PersonPK--
);
When we introduce the primary key constraint on a set of attributes, we are implicitly declaring that these attributes can’t contain NULL values, and the combinations of values they take must all be unique in the table's tuples (just like with UNIQUE).

It’s as if we’re implicitly defining UNIQUE and NOT NULL on the attributes that form the primary key, making sure that they meet all the necessary conditions to truly form a primary key (which can also be referenced by a foreign key).

To declare the existence of foreign keys, we use FOREIGN KEY on the attributes that constitute it.

CREATE TABLE Rental (
    PersonFK INT,
    BikeFK INT,
    RentalDate DATE,
    Duration INT,
    Price DOUBLE PRECISION,
    CONSTRAINT RentalPK PRIMARY KEY (PersonFK, BikeFK, RentalDate),
    CONSTRAINT FK_Rental_Person FOREIGN KEY (PersonFK) REFERENCES Person(PersonID) ON DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT FK_Rental_Bike FOREIGN KEY (BikeFK) REFERENCES Bike(BikeID) ON DELETE
    SET NULL ON UPDATE CASCADE
);
As you can see, declaring foreign keys is very similar to primary keys, except that we. usethe FOREIGN KEY statement. But for the DBMS to ensure referential integrity in the database, we need to define what happens when inserting, updating, or deleting tuples from tables that are referenced by foreign keys.

To understand this, the simplest case is when a tuple is inserted into a table like Rental, where values must be provided for its foreign keys. By default (NO ACTION), SQL allows a foreign key to take NULL values, meaning NULL satisfies the foreign key constraint. But in this case, we should add a NOT NULL constraint on these attributes because, in the conceptual model, a Rental entity was related to at least one Bike entity and one Person entity, as indicated by the minimum cardinality.

So if we insert a tuple with a NULL value in the foreign key attribute and we had the NOT NULL constraint, we’d receive an error. On the other hand, if we insert a value that is not NULL but doesn’t exist in the attribute of the table we are referencing, then the DBMS won’t allow that insertion either – as that foreign key won’t be referencing an existing tuple in the table it points to.

To indicate where it points, we use REFERENCES in the FOREIGN KEY constraint itself, where the table and the attribute the foreign key should point to are specified. Specifically, this attribute must be the primary key of the referenced table to avoid referential integrity errors. This means that the foreign key points to an attribute that can truly uniquely identify the tuples of the referenced table.

Afterward, if we try to delete a tuple from the Bike or Person table that is referenced by a tuple in the Rental table, we can set several deletion policies.

First, by deleting the tuple from Bike or Person, we would have a tuple in Rental that does not reference any valid tuple from another table, creating a referential integrity problem due to an orphaned reference.

One option to solve this is to also delete the tuple in the Rental table and recursively delete the tuples that point to the tuples being removed by this process. We declare this with ON DELETE CASCADE. But if we want to keep the tuple in Rental, instead of deleting it, we can assign a particular value to the foreign key that no longer points to any valid tuple (such as NULL or the default value DEFAULT). We declare this with ON DELETE SET [value], where [value] can be any value that the attribute can take.

But we need to be careful with NULL, because if the foreign key attribute is also part of the primary key, as in this example, it will conflict with the implicit PRIMARY KEY constraint that prevents it from being NULL.

We aren’t required to declare ON DELETE in these constraints, so if we don't, the default action (called NO ACTION) will be executed. This means rejecting the deletion of the tuple in Bike or Person, and showing an error to the user.

Similarly, this issue can also occur when updating a tuple, so the same ON DELETE mechanism applies to tuple modifications, which we can define with ON UPDATE.

Finally, a foreign key can reference the same table it’s in, and using the CASCADE policy is completely valid. This is because it recursively deletes tuples that cause referential integrity issues, not entire tables. Even if there are tuples that reference themselves, this poses no problem, as the DBMS can handle these edge cases.

These are the basic constraints that we can apply to a single table, although there are more advanced tools that help ensure data integrity or even optimize its manipulation and querying.

But there are some constraints that don’t only affect one table in the schema but can involve conditions on multiple tables. To implement them, we have several options, such as assertions, which are conditions very similar to CHECK that are verified every time any of the tables involved in the condition are modified.

CREATE ASSERTION RentalEmailConstraint CHECK (
    NOT EXISTS (
        SELECT 1
        FROM Rental r
            JOIN Person p ON r.PersonFK = p.PersonID
        WHERE p.Email IS NULL
    )
);
For example, here we create an assertion that checks we haven’t rented a bike to any person who doesn't have an Email defined. For this type of constraint, we usually use complete SQL queries within the CHECK, as they are more complex to model than the CHECK constraints we place on a single table.

We could also do this in the table CHECK constraints instead of using assertions, although it would often be more complex to model.

Lastly, besides assertions, we can implement constraints on multiple tables with triggers, which are statements composed of an event, a condition, and an action. When the defined event occurs, the condition that constitutes the constraint is checked, and depending on whether it’s true or false, a certain action is executed or not on the database.

Now that we know how to set constraints on a relational schema, we can refine the logical implementation of our example by adding the necessary constraints, resulting in the following code:

DROP TABLE IF EXISTS Rental;
DROP TABLE IF EXISTS Bike;
DROP TABLE IF EXISTS Person;
CREATE TABLE Person (
    PersonID INT NOT NULL,
    Name VARCHAR(50) NOT NULL DEFAULT 'No name',
    Birth DATE NOT NULL,
    Email VARCHAR(50) NOT NULL UNIQUE,
    CONSTRAINT PersonPK PRIMARY KEY (PersonID),
    CONSTRAINT ConstraintPersonBirth CHECK (Birth <= CURRENT_DATE)
);
CREATE TABLE Bike (
    BikeID INT NOT NULL,
    Model VARCHAR(50) NOT NULL,
    Weight DOUBLE PRECISION NOT NULL,
    --This constraint is redundant due to the definition of PRIMARY KEY constraint--
    UNIQUE (BikeID),
    CONSTRAINT BikePK PRIMARY KEY (BikeID),
    CONSTRAINT ConstraintBikeWeight CHECK (Weight > 0) --Weight must be positive--
);
CREATE TABLE Rental (
    PersonFK INT NOT NULL,
    BikeFK INT NOT NULL,
    RentalDate DATE NOT NULL,
    Duration INT NOT NULL,
    Price DOUBLE PRECISION NOT NULL,
    CONSTRAINT RentalPK PRIMARY KEY (PersonFK, BikeFK, RentalDate),
    CONSTRAINT FKRentalPerson FOREIGN KEY (PersonFK) REFERENCES Person(PersonID) ON DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT FKRentalBike FOREIGN KEY (BikeFK) REFERENCES Bike(BikeID) ON DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT ConstraintRentalDuration CHECK (Duration > 0),
    CONSTRAINT ConstraintRentalPrice CHECK (Price >= 0),
    CONSTRAINT ConstraintRentalDate CHECK (RentalDate <= CURRENT_DATE)
);
As you can see, in the creation script, we have added some DROP statements to remove the tables before creating the final ones with all the correct constraints. We usually do this when there is no data in the tables, as a DROP would delete everything stored in them. Also, when we delete several tables that are related through foreign keys, we want to avoid the DBMS generating referential integrity errors. Because of this, it’s common to first delete the tables that do not have any foreign keys pointing to them, and then continue with the rest.

DCL
Now that you’ve seen how to define the basic elements of the relational model in a DBMS with SQL, we should consider the security with which these operations are performed (as well as those we’ll see in DML). After all, not all database users may have good intentions when operating on the DBMS.

So in DCL, we can define a series of statements for managing users, roles, and permissions, which establish who can do what on the database.

User roles
The first thing we can do is create roles, which, as the name suggests, is a role assigned to a database user that determines what they can or can’t do with the database. Basically, the role functions as a set of permissions.

By default, a user role has basic login permission on a database. So to simplify this section, we can assume that when a user wants access to the database, it’s enough to give them a role with login permission (although these mechanisms may depend on the DBMS we are using).

CREATE ROLE user1 WITH LOGIN PASSWORD 'userPassword';
DROP ROLE user1; --If we want to remove the role--
So even though it can authenticate to the DBMS using the password we define here, a role won’t have permission to connect to any database by default. But we can change that and give that permission with GRANT:

GRANT CONNECT ON DATABASE sampledb TO user1;
By default, the user won't be able to do anything else other than connect. So by using GRANT in the following way, we can give the necessary permissions to execute any necessary statements on certain elements of the database.

GRANT SELECT, UPDATE
ON TABLE Rental
TO user1;
For example, here we are giving permission to execute the SELECT and UPDATE statements on the Rental table.

Or if we want to give all possible permissions to do anything on an element, we can use ALL, like this:

GRANT ALL PRIVILEGES
ON TABLE Bike
TO user1;
Or, if we want to be more precise, we can even control which columns of a table certain statements can be executed on:

GRANT SELECT (PersonID, Name)
ON TABLE Person
TO user1;
Similarly, if instead of using GRANT we use REVOKE, we remove certain permissions that the role has:

REVOKE ALL PRIVILEGES
ON TABLE Bike
FROM user1;
This is just a part of what can be controlled for a role in a database using DCL statements, as security is a critical aspect.

DML
After setting up user permissions to control what a user can do in the database, we have enough elements to start manipulating and querying the data. So now it’s time to introduce the set of statements that make up the DML of SQL, which mainly handles the management of stored data.

CRUD
To understand data management, you should think about how you’ll operate on them. This is guided by the needs of the user or end client. From this arises the CRUD pattern (Create, Retrieve, Update, and Delete), which defines the fundamental operations performed on the data of a real project and that the database must support.

As you can see from its acronym, at the most fundamental level in our database, new data can be inserted (Create), queried once stored (Retrieve), and can also be modified (Update) or deleted (Delete) when they are no longer useful for the domain.

Of all these operations, the most important one is querying the data. If we think about it, any service provided to the end user can be reduced to a query on stored data.

For example, simply viewing saved information means it has to be retrieved through a query. Really any metric that needs to be calculated on the data also involves querying and then computing on it. So even though DML involves a wide variety of statements with diverse objectives, we will focus here on those that form the fundamental blocks for performing queries – CRUD.

When working with relational databases, there’s a certain the mechanism that queries follow to obtain the data we request from the DBMS.

First, we have a series of tables where information is stored in tuples. These we will call base tables, meaning the ones we initially create with CREATE TABLE. We don’t modify these base tables directly – instead, we apply a series of operations to them, many from relational algebra, resulting in intermediate tables. These intermediate tables pass through the sequence of operators until we reach a final table with the results we asked for.

In other words, a query consists of obtaining a resulting table with data from a set of base tables.

From a formal perspective, this is sometimes interpreted in relational algebra as if the query were a relational tree where the leaf nodes are the base tables. As operators, which can be either unary or binary, are applied, new intermediate tables are generated, representing the intermediate nodes of the tree until reaching the root node, which is the final table, or the query result.

With this, we can see each operator as if it were a mathematical function that takes one or more tables as input, performs a certain operation on them, and returns another table as output.

In contrast, when we program in SQL, we don't directly use these relational operators, as they are formal tools that support data querying. Instead, we use a series of DML statements, some of which resemble relational operators but are actually meant to be combined with other statements to form a query.

SQL is not a formal language like relational algebra – it’s an implementation based on this formal language, as well as on relational calculus, which allows us to abstract certain formal details. So when we’re executing a SQL query, the DBMS will transform it from a sequence of SQL statements into an execution plan more similar to a sequence of relational algebra operators. Then it’s internally resolved with advanced techniques that work on the formal operators themselves.

It's also important to note that most of the optimization is done by the DBMS when analyzing the structure of the query. Despite this, we should always try to "help" the DBMS optimizer by writing SQL queries that aim to minimize its workload. For this, there are certain techniques you should follow (but that we won’t cover in detail here).

Before introducing DML statements, it's a good idea to have the schema loaded with the Person, Bike, and Rental tables, as well as some sample data. In addition to creating the tables, to ensure that the queries return some data and we can verify they actually work, you’ll need to insert data into them using INSERT.

SELECT and FROM
The first statements we'll look at for building a query are the most basic ones: SELECT and FROM. You can’t construct a SQL query without a FROM, as it’s used to determine from which table the data will be gotten. Here’s how it works:

SELECT *
FROM Bike;
For example, if we run this query, it will return all the tuples stored in the Bike table. This is because we must provide one and only one table to FROM, in this case, Bike, from which the data will be obtained. Then, after getting the data from that table, SELECT * is used to select the data from all its columns, which is what we will return to the user.

Although we can only use one table in the FROM, we can actually perform a series of operations on several base tables and use that result as the table in the FROM. In other words, we can make the result of a SQL query, which is itself a table, the table used in the FROM, as shown here:

SELECT *
FROM (SELECT * FROM Bike);
This isn’t common to do with such a simple example, but it’s useful to show that we can provide anything we can build with a SQL query as the table to FROM (since the result of all the queries we can construct is actually a table).

When trying to transfer the functionality of these statements to relational algebra operators, we’ll see that there is no specific operator for FROM that does something similar.

But for SELECT there is an operator that does almost the same thing. Specifically, in relational algebra, there is the projection operator π(Table, ListAttributes). It takes as input a table with data and a list of some of its attributes, and returns another table constructed from the input where only the attributes in the list are kept – with all the data from their columns – discarding the rest of the attributes not appearing in the list.

This is exactly what SELECT does: we have an input table given by the FROM clause, and then we define a series of attributes we want the resulting table to have, discarding the rest.

SELECT Name, Birth
FROM Person;
For example, when FROM gets the data from the Person table, it provides it as input to SELECT. This then returns a table where only the attributes Name and Birth that we listed are present, with all the data from their columns. If we need to get all the attributes, we can use SELECT *, and we’ll get the input table with all its attributes and data as it was received.

Aliases
Another operator that we have in SQL in an almost equivalent form is the renaming operator. As its name suggests, we use it to provide alternative names to the tables or attributes we use, to avoid ambiguity problems or to shorten long names.

SELECT P.Name, Birth AS B
FROM Person P;
In relational algebra, the operator is denoted as ρ(Object, Alias), and its function is to assign an alias to an object, which can be either a table or an attribute.

In SQL, there are several ways to use it. On one hand, in the FROM clause, we can use AS [alias] or directly place the alias name after the table or tables involved in the query. This lets us refer to them by their alias instead of their full name, especially if we use the same one multiple times.

Also, in the SELECT clause, we should use AS to avoid ambiguities when assigning aliases to the attributes we’re going to return. The main utility here is to rename the returned attributes to have more descriptive or context-appropriate names.

For example, instead of returning the attribute Birth, its data is returned with the name B, which is shorter, while the Name attribute from table P is returned with the same name it has at the time of performing the SELECT.

DISTINCT
Another important statement is DISTINCT, which we use to remove duplicate tuples from the query result. To understand this, it's important to note that SQL doesn’t use sets to represent the tuples of a table. Instead, the tuples are represented in a multiset, allowing for identical tuples, especially in intermediate tables where primary key constraints and others don’t apply. So if we want the result to have no duplicate tuples, we need to add DISTINCT at the beginning of the attribute list in the SELECT statement.

SELECT DISTINCT P.Name 
FROM Person P;
When executing this query, we should see fewer names because some people have the same name. Also, DISTINCT is not only used at the beginning of the attribute list. We can also use it to count or perform aggregation operations that affect only non-repeated values, as we’ll see later.

This statement doesn’t have a direct equivalent with any relational algebra operator, as relational algebra formally works with sets where duplicate tuples do not exist, eliminating the need for a specific operator to remove duplicates.

WHERE
With what we've seen so far, we can retrieve data from tables, even removing duplicates or unnecessary attributes for the result – but we haven't introduced a way to keep only those tuples that meet certain conditions.

This is precisely what the WHERE clause in SQL does, which has a very similar relational algebra operator called the selection operator (don’t confuse with SELECT) and denoted as σ(Table,Condition). This operator takes a table with data and a condition applied to the tuples stored in the table, so that only those tuples that meet the condition are considered in the output table provided by the operator.

In other words, all operators output a resulting table, which in this case has exactly the same schema as the input table, with the difference that the output table only contains those tuples that meet the condition we have given to the operator. This lets us perform more complex filtering on the stored data, such as retrieving rentals that have a price higher than a certain amount.

For example, by executing the following query, we’ll get all the tuples from Rental that have a price greater than 10. Specifically, we will get all their attributes, since we used * in the SELECT statement.

SELECT * 
FROM Rental AS R 
WHERE R.Price > 10;
There are many possible conditions we can use in the WHERE clause. First, we can compare numeric attributes and strings with operators like >, <, <=, or <>. These check when two things are different. When using these operators on strings, we are performing alphabetical comparisons.

SELECT * 
FROM Rental 
WHERE Price > 50 AND Duration <> 7;
--The <> operator means values of the Duration attribute that differ from 7--

SELECT Name 
FROM Person 
WHERE Name > 'M';

SELECT * 
FROM Person 
WHERE Name = 'Carol King';
As you can see, the operators work the same with numbers as with text. But when using them with text, like in the comparison Name > 'M', we get all the tuples with a Name value that is lexicographically after 'M'.

There are many options we can set for conditions regarding text values. For example, there are functions like LOWER() and UPPER() that convert text to lowercase and uppercase, respectively. We can also use LIKE to compare text with a pattern similar to a regular expression, where we have wildcard characters % and _ (% denotes an arbitrary number of characters and _ a single character).

We can also use the BETWEEN operator to check if a text is lexicographically between two others, but we can use it to compare other data types as well.

SELECT * 
FROM Person 
WHERE Email LIKE '%@example.com';

SELECT * 
FROM Person 
WHERE LOWER(Name) = 'carol king';

SELECT * 
FROM Person 
WHERE Name BETWEEN 'A' AND 'M';

SELECT * 
FROM Rental 
WHERE RentalDate BETWEEN '2025-06-01' AND '2025-06-30';
Continuing with text operations, we also have the SIMILAR operator from the SQL-99 standard, which allows comparing text with regular expressions, using the same wildcard characters as in LIKE. But these regular expressions aren’t the ones we find in POSIX or Perl – they are simply expressions formed by the LIKE wildcard characters with a series of logical operators similar to those of conventional regular expressions.

SELECT * 
FROM Person 
WHERE Name SIMILAR TO '(John|Jane)%'; --Match names starting with John or Jane--

SELECT * 
FROM Bike 
WHERE Model SIMILAR TO '%[0-9]'; --Bike models ending in a number between 0 and 9--
In addition to these operators, there are also the logical operators AND, OR, and NOT, which let us describe more complex conditions.

SELECT * 
FROM Rental 
WHERE (RentalDate BETWEEN '2025-07-01' AND '2025-07-31') AND (Price > 50);

SELECT * 
FROM Bike 
WHERE Weight < 9.0 OR Model LIKE '%Trek%';
--Parentheses are not mandatory, but highly recommended--

SELECT 1 AS ColumnOfOnes
FROM Bike 
WHERE NOT (Weight > 10.0);
Here we can see how in the SELECT clause of the last query, instead of returning an attribute, we return a literal, which is a numeric value of 1. If we look at the result, we’ll get a table with a single attribute, ColumnOfOnes, which is what we want to get by putting it in the SELECT list.

As for the tuples, it returns as many as there are in Bike that meet the WHERE condition, although we won't see their values. Instead, each tuple will only have the value 1 for the attribute ColumnOfOnes, which is what we've named these 1 values.

SELECT *, (Price / Duration) AS Ratio 
FROM Rental 
WHERE (Price / Duration) > 5;

SELECT *, (Price*1.0 / Duration) AS Ratio 
FROM Rental 
WHERE (Price*1.0 / Duration) > 5;
When we’re using arithmetic operators, it's important to consider the data types being used. We have all the usual arithmetic operators +, -, *, and /. But when using division, if we don't perform any explicit casting, the division might be done as an integer division, providing a rounded result that may be far from what we need.

To get an exact division with all decimals, we can multiply either of the operands by 1.0 to force the DBMS to treat it as a decimal value. But we always have the option to multiply the operation by a certain amount like 100 so that the final result is an integer instead of a decimal, especially when calculating ratios.

Of course, in addition to arithmetic operations, SQL offers a series of functions that allow us to perform more advanced mathematical operations like the following:

SELECT
  ABS(-3.5)      AS abs,
  CEIL(2.1)      AS ceil,
  FLOOR(2.9)     AS floor,
  ROUND(2.345,2) AS round,
  TRUNC(2.345,1) AS trunc,
  SQRT(16)       AS sqrt,
  POWER(3,4)     AS power,
  MOD(17,5)      AS mod;

SELECT 
  EXP(1)       AS e_to_1, --The number e raised to the 1 power--
  LN(10)       AS ln10,
  LOG(10,100)  AS logBase10Of100; --Logarithm base 10 of the number 100--

SELECT
  SIN(PI()/2)   AS sin90deg,
  COS(0)        AS cos0deg,
  TAN(PI()/4)   AS tan45deg;
On the other hand, SQL allows performing bit-level logical operations, such as a bitwise AND of the binary representation of two numbers, or a shift of their bits, among others.

SELECT
  9  & 5   AS bitwiseAnd,
  9  | 5   AS bitwiseOr,
  9  # 5   AS bitwiseXor,
  1 << 3   AS shiftLeft,
  16 >> 2  AS shiftRight;
Finally, if we want to check whether an attribute contains the value NULL or not, we can’t use the = operator. Instead, we have to use a specific operator called IS for this comparison:

SELECT * 
FROM Person 
WHERE Email IS NOT NULL; --NULL can't be compared with = operator, but with IS --
UNION, INTERSECT, and EXCEPT
There are other relational algebra operators that are useful and have equivalent SQL statements, like those that operate on sets of tuples. So far, we have treated tables as if they were multisets because SQL allows duplicate tuples by default. But there are situations where it’s clearer to use operations on tables by treating them as if they were sets of tuples.

SELECT BikeFK AS BikeID 
FROM Rental 
WHERE Duration > 3 
UNION 
SELECT BikeFK 
FROM Rental 
WHERE Price <= 15;
For example, when we make a query, it returns a table with tuples, which we can see as a set of tuples. So, if we have several queries that return tables with the same number of columns and all of them have compatible data types (meaning they’re either the same or convertible by the DBMS), then we can perform a set operation between them, like a union of both sets of tuples. This in turn results in another set of tuples containing all those from both initial sets.

We do this using the UNION operator, which by default removes duplicate tuples since it treats the tables as sets of tuples. In this specific example, we’re performing a union between a set of tuples with the schema (BikeID) and another (BikeFK). Since both schemas have the same number of attributes with the same data types, regardless of their names, we can perform their union, resulting in a final table that contains all the tuples from both, removing duplicates.

SELECT PersonFK, RentalDate AS DateName 
FROM Rental 
WHERE RentalDate < '2025-01-01' 
INTERSECT 
SELECT PersonFK, RentalDate AS DateName2 /*This name is not preserved, the above one does*/ 
FROM Rental 
WHERE RentalDate > '2024-01-01';
Besides performing a union, we can also carry out other common set operations like intersection or difference. For example, with INTERSECT, we only keep the tuples that are in both sets of tuples, removing duplicates, as long as we’ve made sure that both sets are valid for performing a set operation between them.

This means that to apply INTERSECT, we have to ensure that the schema of both sets is compatible, both in the number of columns, in this case, 2, and in their respective data types. As for the names, we see here that it doesn't matter what the attributes are called, since the result will always retain the schema name from the first set in the operation.

SELECT PersonFK, RentalDate 
FROM Rental 
WHERE RentalDate < '2025-01-01' 
EXCEPT ALL
SELECT PersonFK, RentalDate 
FROM Rental 
WHERE RentalDate > '2024-01-01';
Lastly, we can also calculate the difference between several sets with EXCEPT, which in some DBMS is called MINUS. This is the only operator where the order of the sets matters, meaning the one above discards the tuples that exist in the set below, so we are left with all the tuples that are in the first set but not in the second. Like the previous ones, this operator also removes duplicate tuples, so if we need to keep them, we have to add ALL after the set operator.

Nested query
We talked about nested queries back at the beginning as a way to use the result of one query within another query. Essentially, that's what it is, but SQL provides a series of specific operators that are useful when working with nested queries in a WHERE clause for example, since they can’t only be placed in the FROM clause.

SELECT *
FROM (
    SELECT PersonFK,
      RentalDate
    FROM Rental
    WHERE RentalDate > '2024-01-01'
  ) AS T
WHERE T.RentalDate <= '2024-06-06';
To start, nested queries take advantage of the fact that a query always returns a table, allowing us to use that result as an intermediate table in another query's computation.

For example, here we first get the tuples from Rental with a date later than 2024 in the subquery of the FROM clause. Then in the “outer” query, we assign the alias T to the result of this subquery, from which we get all its tuples with a date earlier than '2024-06-06'.

SELECT *
FROM Rental R
WHERE R.RentalDate > '2024-01-01'AND R.RentalDate <= '2024-06-06';
As you might guess, when doing this, SQL internally first resolves the subquery in the FROM clause. This means it retrieves all the tuples that the subquery needs to return, and then applies the filter defined in the WHERE clause to all of them. So a condition is first evaluated on all the tuples from Rental, and then another condition is applied to all the resulting tuples from the query. This creates extra work (computation) to first obtain and potentially store in memory the tuples from the subquery and then filter them again.

On the other hand, this query could have been resolved more simply, as shown above. Here, the Rental table is used directly in the FROM clause, and filtering is applied with the two conditions on RentalDate "together" in a single WHERE clause. This means that only the tuples from Rental need to be traversed, instead of traversing them and then having to filter the tuples from a subquery again. This saves unnecessary computation as well as possible memory that the DBMS might use to store the resulting tuples from the subquery in memory.

With this example, we’ve seen that the same query can be resolved in a more or less computationally efficient way depending on how we plan to implement it. Although, generally, all modern DBMS have the Optimizer component in their architecture, which automatically applies certain optimization techniques to the query without us having to worry about it. We won’t go into detail about these techniques here.

In turn, nesting these queries allows us to solve more complex problems with the help of operators like EXISTS. Specifically, we mainly use EXISTS in a WHERE statement before a nested query to check if the nested query contains any tuples or not. In other words, if we consider it as a multiset of tuples, EXISTS tells us whether that multiset is empty or not.

SELECT B.*
FROM Bike AS B
WHERE EXISTS (
    SELECT *
    FROM Rental AS R
    WHERE R.BikeFK = B.BikeID
  );
For example, to find out which bikes from Bike have been rented at least once, we select all those tuples from Bike that have a tuple in Rental associated with the bike we are checking.

To understand this, you need to keep in mind that a SQL query is usually executed by scanning the tuples of the tables from top to bottom. So the WHERE clause of the outer query is actually executed for each bike in Bike, which is the table we traverse in the FROM clause.

So for each bike, we execute a nested query that returns all rentals of that bike, as it keeps the tuples from Rental whose foreign key BikeFK points to the BikeID attribute of the table with alias B. This is called correlated nesting because we’re using the table from the outer query in the nested query. This means we’re forcing SQL to recalculate it each time the WHERE condition is checked on a tuple from Bike.

With this, if the nested query contains any tuple, it implies that the bike has been rented at least once. And we can detect this with EXISTS, which checks if the resulting table from the nested query returns any tuple.

Since we’re simply interested in knowing if it contains any tuple, we don’t need to return any specific attribute in the nested query, although it’s generally considered good practice to return *, or a constant like 1.

Another way to solve the previous query with a different operator is by using IN. This operator checks if a certain value or tuple is contained in a column or table.

SELECT B.*
FROM Bike AS B
WHERE B.BikeID IN (
    SELECT Rental.BikeFK
    FROM Rental
  );
For example, in this case, we build a nested query in the WHERE clause that contains only the foreign key BikeFK from the Rental table, where all the BikeID values referenced by the rental tuples are found. In the outer query, all the tuples from Bike are traversed. It checks a condition where the BikeID from the Bike table must belong to the resulting table from the nested query to be considered a bike that’s been rented at least once.

So to solve this query, we need to know, for each bike, if its primary key BikeID is referenced by the corresponding foreign key of any tuple in Rental.

For this, we can use EXISTS as before to check if there is any tuple in Rental that references the specific primary key value of Bike, or we can use IN to directly check if the primary key value BikeID of the bike we are traversing in the outer query is present in the foreign key column of Rental that we get with the nested query.

Continuing with the equivalent ways to solve the previous query, we can also replace the IN operator with \=ANY. Intuitively, we can understand this as checking if the value B.BikeID is equal to any of the values in the column that we got with the nested query (which is equivalent to what the IN operator does).

SELECT B.*
FROM Bike AS B
WHERE B.BikeID = ANY (
    SELECT Rental.BikeFK
    FROM Rental
  );
In other words, conceptually, checking if something belongs to a set is equivalent to checking if it’s equal to any of the elements contained in the set. Ultimately, the ANY operator allows us to check if a certain value meets a condition with respect to any of the values stored in a nested query – that is, in a multiset, since we can do it with tuples as well as values.

SELECT B.*
FROM Bike AS B
WHERE (1, B.BikeID) = ANY (
    SELECT R.PersonFK, R.BikeFK
    FROM Rental R
  );
For example, instead of checking if a specific value of a single attribute is in the column from the nested query, we can perform the check with a complete tuple.

Here, the nested query returns the foreign key values of the tuples from Rental, so in the outer query, we can check which bikes have been rented at least once by the person with the primary key PersonID=1. Or put another way, for each tuple in Bike, we check if there is any tuple in the nested query table in the form (1, B.BikeID). This would indicate that the person with the primary key PersonID=1 has rented the bike at least once.

Lastly, the IN operator is also equivalent to the NOT <> ALL operation, which is more complicated to understand. Essentially, we want to check if the tuple (1, B.BikeID) is contained in the result of the nested query.

SELECT B.*
FROM Bike AS B
WHERE NOT (1, B.BikeID) <> ALL (
    SELECT R.PersonFK, R.BikeFK
    FROM Rental R
  );
With <> ALL, we check if the tuple is different from each and every tuple stored in the nested query. Then, by negating that result with NOT, we can determine if that condition is not met (that is, the tuple is not different from each and every tuple in the nested query). This would mean it’s equal to at least one of them, or in other words, it’s contained in the multiset returned by the nested query.

To understand the ALL operator, we can try to get the bike with the lowest weight in the entire Bike table. To do this, with a nested query, we can get all the weights from the Bike table. Then in the outer query, we can go through all the tuples in Bike and check if each one’s weight B.Weight is less than or equal to each weight gotten with the nested query using <= ALL.

SELECT *
FROM   Bike B
WHERE  B.Weight <= ALL (
    SELECT Weight
    FROM   Bike
);
If this is true, then that weight will match the lowest in the entire table, so the WHERE condition will be TRUE, and the corresponding tuple from Bike will be returned in our outer query.

In SQL, conditions usually return TRUE or FALSE values depending on whether they are met. But when comparing with NULL values, UNKNOWN is returned, since there are times when a nested query unexpectedly returns NULL values. This causes conditions that compare with those values to not result in logical truth values, but in the special value UNKNOWN.

JOIN
The JOIN operators also have an equivalent in relational algebra. Their main purpose is to gather information spread across multiple tables so that all the data can be operated on in a single intermediate table.

For example, when we look at the information in the Rental table, we see that it has foreign keys referencing Bike and Person, but the Rental table itself doesn’t contain all the information we might need about the bikes or the people. So, if we want to query the rentals and the names of the people involved in those rentals, we’ll need to apply a JOIN operation on both tables.

SELECT *
FROM Rental, Person;
There are several types of JOIN, all of which have an almost direct equivalence in relational algebra operators. The simplest one is the implicit JOIN shown above, which is denoted by using multiple tables in a FROM statement separated by commas. We can use as many tables as we want here, as long as there are no ambiguities in their names.

Note that if we perform an implicit JOIN of a table with itself, we’ll need to assign different aliases to the different uses we make of it.

Before seeing what the query does, it's useful to understand the Cartesian product operation in detail, as it’s the foundation of all SQL JOIN operators.

The Cartesian product is a mathematical operation that takes two sets as input, which in SQL are tables or multisets with tuples, such as table A with tuples {{a},{b},{c}}, and table B with tuples {{1},{2},{3}}. As output, the operation generates a new multiset of tuples where each row of A is combined with each row of B, resulting in the table or multiset A×B={{a,1},{a,2},{a,3},{b,1},{b,2},{b,3},{c,1},{c,2},{c,3}}.

As you can see, if table A has n tuples and table B has m tuples, the Cartesian product will generate n*m tuples, where each one takes values from all the attributes of table A and table B (since the result of the operation includes all possible “pairings“ we can make between tuples from both tables).

So going back to our query, as you can see in the result, the implicit JOIN performs the Cartesian product of the two tables. It doesn't matter if their names repeat, as each repetition can be accessed through a different alias.

Regarding the tuples it contains, we see that the Cartesian product returns tuples where each possible tuple of Rental is combined with each possible tuple of Person. This forms tuples with values in all the attributes of the resulting JOIN table.

The implicit join has no filtering criteria or additional functionality – it simply returns the complete Cartesian product of the tables involved in the operation.

Its name, implicit, comes from the fact that the JOIN operator and the type of JOIN we want to perform aren’t explicitly written. Instead, it's enough to list several tables separated by a comma in the FROM clause.

In addition to the implicit JOIN, we also have the explicit JOIN. It can be of various types depending on the filtering or conditions applied to the Cartesian product.

For example, instead of performing a Cartesian product between both tables with an implicit join, we can also do it explicitly with a CROSS JOIN. This does exactly the same thing but with explicit syntax: we specify the JOIN operation to perform and its type, CROSS. This indicates the execution of a Cartesian product like the previous one.

SELECT *
FROM Rental CROSS JOIN Person;
Besides the CROSS type, there are other types that provide additional functionalities to the JOIN, allowing us to filter the tuples we get from a Cartesian product.

For example, so far with the Cartesian product, we have obtained all combinations of tuples from Rental and Person. If there are N tuples in Rental and M tuples in Person, then the Cartesian product will return N*M tuples – meaning all possible combinations of tuples from both tables we are working with.

If we look at the resulting table from this operation, we will see that some values of different attributes like PersonPK and PersonID match in the same tuple. This means a tuple from Rental has been combined with a tuple from Person so that this is the person referenced by the foreign key in Rental. In other words, we have a tuple that not only contains the information from Rental but also has the information from the Person tuple representing the person who made that rental – and it’s been"concatenated" or combined with it.

So if we want to keep only those tuples from the Cartesian product where PersonFK matches PersonID from the Person table, we could apply a condition in a WHERE clause to filter those tuples. But by doing this, we would first create all possible tuples from the Cartesian product and then go through them all to filter, which is not entirely optimal.

There are specific types of JOINs that can help us perform this filtering more efficiently:

SELECT *
FROM Rental AS R CROSS JOIN Person AS P
WHERE R.PersonFK=P.PersonID;

SELECT *
FROM Rental R INNER JOIN Person AS P ON R.PersonFK=P.PersonID;
To implement this query, we can use a condition in a WHERE clause, or we can use an INNER JOIN, which allows us to set a condition in the ON clause.

If we use a WHERE clause, we’ll be filtering all the tuples obtained from the complete Cartesian product resulting from the CROSS JOIN using a condition. But to avoid creating the entire Cartesian product (which isn’t efficient), we can use an explicit INNER JOIN. Here, we can provide a condition in the ON clause so that only the tuples from the Cartesian product that meet that condition are actually constructed.

In the ON clause of an INNER JOIN, we can put any type of condition on the tuples we want to get. But there are times when these conditions are simple and only involve equality between attributes, which may even have the same name.

SELECT *
FROM Person P1 CROSS JOIN Person P2
WHERE P1.PersonID=P2.PersonID;

SELECT *
FROM Person P1 INNER JOIN Person P2 ON P1.PersonID=P2.PersonID;
For example, if we perform the Cartesian product between the Person table and itself, and we want to keep only those tuples where the PersonID attributes of both tables match, we can use an INNER JOIN with the condition that the PersonIDs of both tables being combined are equal. This way, only the tuples that meet this condition will be constructed (unlike the previous query where using a CROSS JOIN implies constructing all tuples of the Cartesian product, which requires more computation).

In these types of situations, instead of using an INNER JOIN, we can take advantage of another type of JOIN like the NATURAL JOIN. This returns only those tuples where the values of all attributes with the same name match.

SELECT *
FROM Person P1 NATURAL JOIN Person P2;

SELECT *
FROM Person P1
  NATURAL JOIN (
    SELECT PersonID,
      Name AS Name2,
      Birth AS Birth2,
      Email AS Email2
    FROM Person
  );
To understand this, we can perform a NATURAL JOIN between the Person table and itself. First, if we don't rename any attribute, then all will have the same name in both tables – so the NATURAL JOIN will impose an equality condition for each attribute. This means that it’ll return only those tuples that satisfy P1.PersonID=P2.PersonID, P1.Name=P2.Name, and so on for the rest of the attributes, since they have the same name despite being in tables with different aliases. This will result in the same Person table, as the NATURAL JOIN, in addition to imposing these conditions, "merges" attributes that meet these conditions. So if they have the same name, it leaves only one occurrence of them, not both (as happens in other types of JOINs).

But if we rename the attributes of one of the tables except for PersonID, we’ll see that NATURAL JOIN only imposes the equality condition P1.PersonID=Person.PersonID, since PersonID is the only attribute that’s exactly the same in both tables.

In the resulting table, we’ll get the same as before but with the renamed attributes included, as they aren’t discarded or subjected to any condition that makes them unnecessary. Even if we rename PersonID as well, we’ll get the Cartesian product of Person with itself – because if none of the attributes have the same name in both tables, then NATURAL JOIN doesn’t impose any equality condition.

Another option we have to impose equality conditions on attributes with the same name in both tables is to use an INNER JOIN. Instead of declaring conditions in an ON clause, we use a USING clause where we define the attributes on which equality conditions are imposed. These must have exactly the same name in both tables.

SELECT *
FROM Person P1 INNER JOIN Person P2 USING (PersonID);
For example, in the query above, we are getting the tuples from the Cartesian product of Person with itself that satisfy P1.PersonID=P2.PersonID.

The main difference with NATURAL JOIN is that NATURAL JOIN tries to impose this equality condition on all possible attributes with the same name. But with an INNER JOIN and USING, we decide which equality conditions are imposed on which attributes (as long as they have the same name in both tables). Otherwise, the DBMS might generate an error.

Also, when we use USING in combination with an INNER JOIN, only one occurrence of the attributes with the same name appears in the resulting table, just like with NATURAL JOIN.

Lastly, it’s important to note that when using ON to declare a condition, no attribute is removed from the resulting table of the JOIN operation, since the condition can be very diverse in nature. This means it doesn't necessarily have to be an equality between several attributes.

But when you’re using USING in combination with an INNER JOIN (and imposing an equality condition on the attributes declared in the USING clause), all repetitions of those attributes will be removed from the resulting table. So, if we impose an equality condition on several attributes with the same name, all but one of their occurrences will be deleted.

For example, in a table with two attributes called PersonID but coming from different tables or elements with different aliases (same Person table but different alias), USING would remove one of their occurrences. This would leave only one PersonID attribute in the resulting JOIN table, while ON would not remove any of the occurrences. And. this would result in the final table containing both original PersonID attributes.

SELECT *
FROM Person P LEFT JOIN Rental R ON R.PersonFK = P.PersonID;
Continuing with the types of JOIN, there might be a case where a person has never rented a bike, so there won't be any tuple in the Rental table referencing that person. This is possible due to the minimum multiplicities on the Rental side in the entity-relationship diagram (that don’t require any person to have rented a bike).

So if we want to build a table that shows information about all people along with information about all the rentals they’ve made, the first thing we may think of is performing an INNER JOIN between them. And we’d add a certain equality condition on the foreign key attribute of Rental that references the primary key of the Person table.

But there may be people who have never rented. abike, so if we do an INNER JOIN, the information about these people won’t appear in the table. To make sure that they appear, we need to use an OUTER JOIN instead of an INNER JOIN. We also need to specify which table we want to force to have its data appear by putting LEFT or RIGHT before the type of OUTER JOIN (or we can simply use LEFT JOIN, for example).

This way, if we use LEFT JOIN, we’re forcing the data from the table on the left of the JOIN to appear in the resulting table. If they have no match in the table on the right (meaning if they have no rental), then the other attributes will be filled with NULL values, as we saw in the result of the previous query.

SELECT *
FROM Rental R RIGHT OUTER JOIN Person P ON R.PersonFK = P.PersonID;
In the same way, if we use RIGHT JOIN and reverse the order of the tables, we’ll do the same but force the data from the table on the right to appear in the resulting table, filling the attributes of the left table with NULL in case there’s no match.

But if we don't reverse the order and use RIGHT JOIN, then since all rental tuples must reference a person, no tuple will need to be filled with NULL because all of them will have associated person information in the Person table.

Finally, if we want to use both RIGHT and LEFT in a join and force the data from both tables to appear (which would fill in NULL on the side that corresponds to each tuple), we can use a FULL JOIN.

SELECT *
FROM Person P JOIN Rental R ON R.PersonFK = P.PersonID;
In this last type of JOIN, we've seen that specifying OUTER is optional when using RIGHT, LEFT, or FULL. But by default, if nothing is specified, the JOIN operator is treated as an INNER type, requiring a condition with ON or USING afterward.

Aggregation
With joins, we can now combine several tables and gather their information into one. But there are still certain operations we can't do easily, like counting the rows in a table, summing the values of a column, calculating their average, and so on.

All operations of this nature that involve values from a multiset (table) of tuples are called aggregation operations. Their goal is to perform a calculation on a series of tuples and are the basis of analytical queries.

SELECT COUNT(*) AS rentalCount,
  SUM(Price) AS income,
  AVG(Price) AS averageRentalPrice,
  MAX(Price) AS maxRentalPrice,
  MIN(Price) AS minRentalPrice
FROM Rental;
SQL offers a number of them (which don’t have a direct equivalent with relational algebra operators): COUNT(), SUM(), AVG(), MIN(), and MAX().

COUNT()
We can use COUNT() to count how many rows are in a table, including tuples where all values are NULL. So by declaring COUNT(*) in the SELECT clause, we’ll get the number of tuples in the table specified in the FROM clause.

SELECT COUNT(*), COUNT(Price), COUNT(DISTINCT Price)
FROM Rental;
But the function can also perform aggregation on a specific column. So instead of counting tuples, it counts how many values exist in a certain attribute, including duplicate values and ignoring NULLs.

So if we want to count only how many distinct values there are in Price, we can use DISTINCT as shown above.

As for the column names we get from these operations, it's not mandatory to assign them an alias and rename them, but it's very convenient for identifying which calculation is stored in each column of the resulting table.

SELECT COUNT((PersonFK, BikeFK))
FROM Rental;
In addition to a single attribute, COUNT() can count how many combinations of values from a certain set of attributes are in the table. Specifically, in this example, we are counting how many (PersonFK, BikeFK) values are in the table. This may not match the total number of tuples since NULLs are ignored here, unlike in the COUNT(*) operation where they are also considered. We can also use DISTINCT here, as long as the attributes whose value combinations we want to count are in parentheses.

SELECT SUM(2*Price), AVG(Price)
FROM Rental;
SUM()
SUM() calculates the sum of a certain numeric attribute of a table, or an attribute that can be converted to numeric. It takes as input the attribute from which we want to get the sum of all values present in the table. Note that, besides the attribute, SUM() accepts expressions that result in a single attribute. That is, if instead of Price we provide 2*Price, or Price+Price, then those operations will be summing a series of attributes whose result will be stored in a single attribute. This is given as input to SUM().

If all the values of the attribute are NULL, SUM() returns 0. Unlike COUNT(), in this case, we can’t sum several attributes at once, meaning SUM() only takes one attribute as input, regardless of whether we get it through an arithmetic expression.

AVG()
Similarly, AVG() calculates the average of the values taken by a single attribute, ignoring NULLs. Unlike SUM(), this function returns NULL when all the values of the input attribute are NULL, since internally it can be calculated as SUM()/COUNT().

So if SUM() returns 0 when counting an attribute full of NULLs and COUNT() ignores those NULL values, the average will be 0/0, which is undefined – causing AVG() to return NULL. It’s also important to note that if we use DISTINCT, both the sum and the average will be different.

SELECT MIN(Price), MAX(Price)
FROM Rental;
MIN() and MAX()
Finally, the MIN() and MAX() operations take an attribute as input and return the minimum or maximum value found in the tuples stored in the table, respectively. If all the values of that attribute are NULL, they also return NULL, as a coherent minimum or maximum value can’t be established since NULLs are ignored.

GROUP BY
If we try to use aggregate functions in the SELECT clause along with other attributes, the DBMS will give us an error because these types of functions are usually used together with the GROUP BY statement (this also doesn't have a direct equivalent in relational algebra).

To understand how GROUP BY works, we can calculate the sum of all the rental prices that a certain person has made in the system.

SELECT SUM(Price)
FROM Rental R
WHERE R.PersonFK=5;
To do this, we access the Rental table and use a WHERE clause to filter all rental tuples for a certain person using their foreign key that references the person making the rental. Then, with SUM, we get the sum of the Price attribute from the final table, which contains the prices of all rentals made by that person.

If we wanted to do it by name instead of PersonID, we would need to do a JOIN with the Person table and filter by the Name attribute of Person (although this isn’t important for understanding GROUP BY).

SELECT SUM(Price) AS PriceSum
FROM Rental R INNER JOIN Person P ON R.PersonFK=P.PersonID
WHERE P.Name='Carol King';
Now, if we want to calculate this value for the rest of the people in the database who have ever rented a bike at least once, we would have to run this query multiple times for each person in the system, which isn’t practical. Instead, we can take advantage of the fact that the Rental table itself has the foreign key PersonFK for people who have rented bikes – and we can use this to calculate this sum for all of them more simply using GROUP BY.

SELECT R.PersonFK, SUM(Price) AS PriceSum 
FROM Rental R 
GROUP BY R.PersonFK;
As you can see, this query returns all the people who have ever rented a bike – meaning those referenced from the Rental table. For each one, it calculates the sum of the prices of their rentals. This is possible thanks to GROUP BY, which groups all the tuples in the Rental table by the PersonFK attribute.

Since each person can have multiple rentals in the Rental table, we need to get all the tuples that reference each person and group them so that we can perform an aggregation operation like SUM() on one of the attributes.

In this case, we do the grouping with the PersonFK attribute, which identifies the person who made the rental. So since all the tuples in Rental with the same value in that attribute belong to the same person, they are grouped by that attribute to form groups of tuples, one for each person.

With this, we can then return the attribute that was grouped (which must be included in the SELECT when using GROUP BY) along with the results of the aggregation operations calculated on those groups.

SELECT DISTINCT Price
FROM Rental;

SELECT Price
FROM Rental
GROUP BY Price;
When we use GROUP BY and partition the tuples of the table into groups, each group is "identified" or represented by one value of the attribute we are grouping by. This means that when we return a result to the user, for each group, they receive a single tuple where the attribute used for grouping takes the value of the "representative" of that group, instead of receiving multiple tuples per group.

For example, to get all the distinct prices from the Rental table, we can use DISTINCT directly, or we can also group by that attribute, which results in forming different groups of tuples, one for each distinct price. Finally, when returning Price after grouping, the distinct values of Price that form the different groups of tuples are returned, meaning only the distinct Price values are obtained.

It’s also worth noting that we can group by several attributes at once, not just one. In this case, we would generate groups of tuples based on the unique combinations of values those attributes take in the table.

Finally, when we use the GROUP BY statement in a query, we might want to filter and keep only the tuples whose aggregation operation results meet a certain condition. For example, to get only the people whose total rental price sum is greater than 100, we might think of using a WHERE clause with the following condition:

SELECT R.PersonFK, SUM(Price) AS PriceSum
FROM Rental R
WHERE PriceSum > 100
GROUP BY R.PersonFK;

SELECT R.PersonFK, SUM(Price) AS PriceSum
FROM Rental R
GROUP BY R.PersonFK
HAVING SUM(Price) > 100;
But if we use that condition in the WHERE clause, the DBMS will give us an error because we can’t impose conditions on the aggregation calculations in the groups in a WHERE clause. We also can’t refer to them with the alias we give them, since the alias is applied at the end of the query when the result is provided to the user.

So instead of using WHERE, when we want to implement this type of condition, we use HAVING. Instead of the alias, we use the expression SUM(Price) itself to refer to the sum of Price in each group. Using WHERE isn’t prohibited, because before doing the grouping, we can filter the data that appears in the FROM table, thus grouping fewer tuples.

ORDER BY
Finally, if we want to sort the tuples of a table, we can use the ORDER BY clause. It lets us we specify one or more attributes on which the sorting is performed as well as a direction (which can be ASC or DESC for ascending or descending order, respectively).

SELECT *
FROM Person
ORDER BY Name ASC;

SELECT *
FROM Person
ORDER BY (PersonID, Name) ASC;
In sorting, certain attributes have higher priority. Those we place more to the left are sorted first, as in this last query that sorts the tuples of Person by their PersonID values and then by name.

So using all these clauses, we can start making SQL queries to get almost any type of result we need. As we have seen, queries are composed of a series of statements or clauses where each one performs a certain action on the tuples of a table.

These statements usually follow an order of appearance in the query that is important to follow to avoid DBMS errors. The order is as follows:

SELECT

FROM

WHERE

GROUP BY

HAVING

ORDER BY

But at a low level, the execution of these statements or equivalent relational algebra operators follows a different order than the one we use when writing the query. It is as follows:

FROM

JOIN … ON

WHERE

GROUP BY

HAVING

SELECT

ORDER BY

First, data is fetched from a table with the FROM clause, which may need to perform certain JOIN operations between multiple tables to have the data ready. Then, the data is filtered using the conditions we set in the WHERE clause, if we use it. After that, the tuples are grouped and filtered again if we use GROUP BY. Finally, the SELECT clause is applied to extract the attributes we are interested in from the final table, which we rename and order if necessary.

So as you can see, when we write a SQL query, we must use the clauses in a specific order. But we should keep in mind that the DBMS, at the physical and storage level, doesn’t execute these statements in the same order we write them. In fact, we don't have to worry too much about this internal order because it’s transparent (that is, handled automatically and hidden) to the user. This means we don't have direct control or "see" how the execution of the clauses is carried out internally by the DBMS, inspect the plan with EXPLAIN/EXPLAIN ANALYZE.

Regarding the internal execution order, the DBMS usually reorders, combines, or transforms the clauses into others, all while constructing a physical execution plan for the query. This involves generating a plan for the operations and internal resources needed to execute it optimally (hence the reordering).

This is important to know when constructing a query, as the way you program it can affect the efficiency of the query, even though the DBMS can help by automating much of the optimization process. You don’t have to use all these statements in a query, of course. But those you do use should respect the order in which they should be written, otherwise, the DBMS will likely end up throwing an error.

Views
To finish with DML, let's look at a possible application of queries when defining DDL elements in SQL. Originally, we saw that DDL statements allowed us to create databases, tables, and similar elements. One of them worth highlighting is views, which are virtual tables that let us abstract information from the tables in a database.

Our database is made up of a schema or set of tables where the information is stored, but we might need to "view" that information differently than how it's defined in the schema itself. For this, we define a view that lets us query that information from the database using a different structure than the one used to store it.

CREATE VIEW RentalOverview AS
SELECT P.PersonID AS PersonID,
  P.Name AS ClientName,
  CURRENT_DATE - P.Birth AS ClientAge,
  B.BikeID AS BikeID,
  B.Model AS BikeModel,
  R.RentalDate AS RentalDate,
  R.Duration AS RentalDurationDays,
  R.Price AS RentalTotalPrice
FROM Rental R
  JOIN Person P ON R.PersonFK = P.PersonID
  JOIN Bike B ON R.BikeFK = B.BikeID;
SELECT *
FROM RentalOverview;
For example, in our database, we have the tables Rental, Bike, and Person, but for convenience or requirements, we might need to see all that information from the tables integrated into a single table with attributes (PersonID, ClientName, ClientAge, BikeID, BikeModel, RentalDate, RentalDurationDays, RentalTotalPrice).

By default, every time we want to see this integrated information, we would have to manually run a query (or several, depending on the circumstances) to get and integrate that information into a table.

But to simplify this process, there are views that allow us to define a "virtual" table containing the integrated information. So, whenever we need that integrated information, we can refer to the virtual table (and this is built using the query we would have had to run manually to construct it). This query is the definition with which we declare a view, and the view itself saves us from having to run it manually to get the integrated information.

That's why we create a new view in the database that acts as a virtual table (meaning it doesn't actually store any information). This is because a view is a table that receives user queries, but to resolve them, it has to fetch information from different tables in the database.

So, as you can see in the view above, the virtual table RentalOverview is defined with a SQL query on the tables that do store information. So when we query RentalOverview, the DBMS is actually transforming our query using the view's definition to obtain the attribute ClientName, for example, which is defined as the name of the person who rented a bike.

In this specific case, our view is gathering all the information from the three tables into one, so when we query it, we have the complete information about the person, bike, and rental that occurred. We don't have to perform the JOINs ourselves, as they are part of the view's definition.

SELECT *
FROM RentalOverview;
When querying the virtual table, we’ll get information derived from the base tables, which is shown to us according to the schema we defined in the view. For example, in the database, the birth date of people is stored in the Birth attribute. But the view shows that data differently, displaying age instead of the birth date. Both refer to the same information but are viewed in different ways.

Database Administration
At the logical level where we implement the database with SQL, we need to perform ongoing database maintenance (in addition to data modeling, modification, and querying). This ensures that our data and services are available, optimizes query performance, and provides certain guarantees of security and integrity. This process is part of what is considered database administration, which is a task carried out by experts.

Database users
Before introducing the concept of administration, let’s talk about the different types of users that might use a database. Each of them has a certain objective, responsibilities, and competencies.

To start, we have the client user, who uses the services provided by the database. We can see this type of user as an average user of mobile or web applications, or on any platform, using a series of services that involve a database.

Then, we have the developer user, who is dedicated to technically implementing the infrastructure, both software and hardware, that supports the applications and services. Developer users are also responsible for defining the business logic of the database, its structure, requirements, and so on. In short, they follow the different design stages we saw at the beginning, especially the conceptual and logical design, although they don’t interact with the DBMS. They simply propose the schema that the data should follow for a specialist to implement on a DBMS.

This specialist is the database administrator user, who is responsible for implementing the logical design of the database on a DBMS. To do this, they perform tasks such as choosing the appropriate DBMS for the project in question, installing it, and keeping it updated. They create the database, tables, and other logical elements, manage the security of the DBMS by defining roles, permissions, and security policies, and monitor the database's performance to ensure its availability. They also provide technical support to other types of users and define data backup protocols.

So basically, the administrator is in charge of the implementation during the logical design stage, as well as subsequent stages of possible physical design and storage. They’re also responsible for maintaining the DBMS. Among all these tasks, one of the most critical is optimizing the queries users might make to the system and refining the schemas if necessary to improve performance.

Database metadata
So far, we have only considered that the database is responsible for storing information (data). These are ultimately generated by the project or application that the database supports, such as the tuples of the tables.

But in addition to these data, the database contains a series of metadata used to manage the data. Essentially, metadata primarily serves to describe another piece of data or provide additional information that helps organize it within the database. Here’s an example:

Name	Birth	Email
Alice Johnson	1985-07-12	alice.johnson@example.com
Bob Smith	1990-03-05	bob.smith@example.org
Carol Davis	1978-11-23	carol.davis@example.net
David Brown	2001-01-30	david.brown@example.com
Emily Wilson	1995-09-14	emily.wilson@example.co.uk
To understand the idea of metadata, we can introduce the concept of a schema as metadata. In a table, we have a table name, which is metadata that describes the table. This allows us to know which table we are referring to when using that name in a query or other situations.

Besides the name, all tables have a header composed of the names of the attributes located in the first row, which make up the table's schema. These names are used to refer to the attributes or columns, just as the table name is used to refer to the table itself as an object. So the schema is part of the metadata, as it provides meaning to the data stored in the columns, allowing them to be organized.

In other words, if we didn't have the first row with the attribute names, we would have no information about the stored data, as we would lack their semantics. This is precisely what the schema provides as metadata, which lets us manage them.

Apart from table and attribute names, tables usually have associated technical metadata from the DBMS. This metadata indicates the users who own the table or have certain permissions to perform actions on it. It also contains the creation and last modification dates of the table to ensure data security, existing connections, or information about events or locks for managing concurrency.

The table as an object does not store its name and all metadata within itself, but rather in specific places within the DBMS. These specific places are reserved tables for the DBMS called dictionaries, or sometimes catalogs. They utilize the structured nature of the DBMS to store this metadata in a simple way, similar to the storage of the actual data.

Since these places are tables, they also have a name, schema, and metadata, stored in the DBMS in physical data structures, not in other tables. As for their schemas, they are specially referred to as metaschemas.

The metadata in a DBMS varies significantly depending on the specific DBMS we’re using. But in all of them, we’ll always find fundamental information about the database we have implemented, like its name, table names, schemas, constraints, and so on.

Specifically, in PostgreSQL, we can find them in the "schemas" pg_catalog and information_schema. Here, PostgreSQL refers to a "schema" as a logical container that holds certain tables, views, and similar elements of a database, where many of them are responsible for storing metadata. So a logical container is nothing more than a folder used to group elements to make them more hierarchical and organized.

On one hand, pg_catalog is the internal catalog of PostgreSQL, which means it contains all the information necessary to manage the DBMS's operation. But this catalog is very technical and dense, as it’s aimed at managing the entire operation of the system, involving a lot of details that aren’t always necessary for an administrator.

Becuase of this, there’s a standard abstraction of this logical container called information_schema, introduced with the SQL-92 standard, which primarily serves to abstract the specific details related to the DBMS's operation and provide the database administrator with a series of views to better visualize and manage the metadata.

To know what pg_catalog contains, you can use commands like \dt pg_catalog.* to see the tables, views, or generally the elements it contains. Among all of them, the most important are:

pg_catalog.pg_class: Stores metadata of database objects, such as tables or views, among others.

pg_catalog.pg_namespace: Stores the names of the schemas (logical containers) of the DBMS

pg_catalog.pg_attribute: Stores the names of the attributes of tables or views, meaning their schemas, as well as their data types or user-defined domains.

pg_catalog.pg_type: Stores the default data types and user-defined types.

pg_catalog.pg_attrdef: Stores the default values defined for the attributes.

pg_catalog.pg_constraint: Stores the definitions of constraints on tables, such as PRIMARY KEY, UNIQUE, FOREIGN KEY, CHECK, and EXCLUSION, including information about the table they apply to (conrelid), the columns involved (conkey), the update and delete actions on foreign keys (confupdtype, confdeltype), and the name of the constraint (conname), among others.

pg_catalog.pg_stat_activity: Provides real-time information about active sessions on the PostgreSQL server.

As you can see, if we explore the content of pg_catalog, we’ll find that it’s very dense and detailed. That's why we have the standard alternative information_schema, which simplifies metadata management. It works similarly to pg_catalog, serving as a logical container that provides views of the DBMS tables we've seen before to abstract their functionality.

The most significant ones are:

information_schema.tables: Stores a list of all the tables and views in the database.

information_schema.columns: Stores metadata for all the columns of all tables and views.

information_schema.table_constraints: Stores a list of all table-level constraints (primary key, unique, foreign, check...).

information_schema.key_column_usage: Stores a list of columns involved in key constraints (primary, unique, or foreign).

information_schema.referential_constraints: Stores metadata about FOREIGN KEY constraints, such as actions triggered after a deletion or update, among others.

To query the information contained in all these tables or views, you can simply use queries as if you were retrieving data from any other user table. But keep in mind that many of them also contain metadata about the DBMS dictionary or catalog tables themselves, which can complicate understanding the results.

SELECT *
FROM information_schema.tables
WHERE table_name='rental';

SELECT *
FROM pg_catalog.pg_class
WHERE relname = 'bike';

SELECT *
FROM pg_catalog.pg_stat_activity;

/*Get metadata of the PRIMARY KEY constraints we named with "PK"*/
SELECT*
FROM pg_catalog.pg_constraint
WHERE conname LIKE '%pk%';

SELECT*
FROM pg_catalog.pg_constraint
WHERE conname LIKE '%pk%';

SELECT *
FROM information_schema.table_constraints
WHERE constraint_name LIKE '%pk%';
Chapter 10: Database Design Process Example
So far, you’ve learned about the entire relational model and some basic SQL. Now you can create a relational database on the PostgreSQL DBMS, manage it, and perform queries on it. So let’s apply all this knowledge to a real-world use case.

Database Levels
To do this, we need to remember some of the different levels of the database design process. First, we have the analysis phase where we gather the project requirements from the end user or client. Then we create a conceptual design, which we subsequently transform into a logical design that we can implement on a DBMS.

These are the main levels we need to worry about here. But in addition to these, we have the physical level, which focuses on the internal representation of the logical model implementation of the database in the DBMS using DBMS objects like indexes. We also have the storage level, which is the closest to the hardware, and is mainly dedicated to organizing the disk files that implement the database functionality on the DBMS. Lastly, we also have the application design level that aims to provide the database as a service to the user.

We won’t cover these additional levels in this example due to their complexity and because they aren’t as closely related to the actual database design.

The Database Design Process
When faced with a real problem that requires designing a database, the first thing we need to do is gather as much information as possible from the user or client. We do this to formalize the requirements of the system we’re going to build.

We can interview the client, survey potential users of the service, or use other similar methods. In this case, we won’t directly perform any of these tasks. Instead, we’ll assume that we have certain requirements, and from them we’ve been able to construct an entity-relationship diagram that captures them and correctly models the domain of our system. Let’s say it looks like this:

Entity-relationship diagram that we will work with in this chapter. It represents various entities and relationships, where entities include "Vehicle," "Person," "Car," "Pool," "City," and others. Image by author. 

As you can see from the diagram above (you can enlarge it by opening it in a new tab), the project we’ll work on in this example is an extension of the bike rental domain we’ve used so far.

In addition to a bike rental service, we’ll include other elements that may be present in a real world database model, such as vehicles, places, cities, and so on. We’ll also include actions that can be performed between these elements, like owning a car, residing in a city, booking a cruise trip, or getting a bus pass, among others.

When we’re building this diagram, our most significant decisions involve which concepts are modeled with entities, which are represented through relationships, and which aren’t worth including in our system.

From the entire domain, it's common to encounter a lot of information provided by the client or users that doesn't directly help us model the system, as they don’t expect it to be stored in the database. So all concepts related to information that is not intended to be stored persistently are usually not included in the design.

As for the other issues, they are very subjective, and there is no set of rules to follow to know unequivocally which concepts to model with entities or relationships – or even to determine the degree of these relationships (which in this context we will assume is always 2 to avoid complicating the design with relationships involving more than two classes).

Entity-Relationship to Logical Model
But to understand how we can and should make these design decisions, it's useful to understand the purpose of each entity in this entity-relationship diagram, as well as the meaning of the elements it comprises or relates to. We also need to understand how it’s been translated to the logical design level.

Before explaining each of the entities, below is the relational diagram we have after the entire logical design phase:

Relational diagram derived from the previous entity-relationship one. Image by author. 

This diagram is what we will gradually build as we transform entities into tables. Make sure you have both this diagram and the entity-relationship diagram open in separate windows so you can refer to them during the following chapters. This will make everything we discuss easier to understand.

As you can see in the diagram above, it includes some modifications like foreign keys pointing to "loose" attributes such as Sanction.SanctionID, instead of the same attribute in the table of the diagram. This aims to prevent the foreign key arrows from crossing excessively. Although this isn’t a standard way to represent the relational logical model, as long as its meaning is specified it’s completely valid.

Some constraints aren’t modeled in the system due to their complexity, which we’ll see as we explain all the entities. That's why there are no notes included in the relational diagram, and we don’t indicate the attributes that can or can’t be NULL. These are helpful to show in the diagram, but it’s not required.

Lastly, during the explanations, we’ll show the SQL code used to create each table. You’ll find the SQL script for creating the entire database at the very end, after I’ve explained all the entities. This is because we’re not going to discuss them in the order they need to be created, in order to respect referential integrity constraints that would cause errors in the DBMS if tables were created in a different order.

Person entity
First, we have the entity Person, whose main goal is to model the existence of people in our system. It's important to note that in our domain, there are physical people, where each one is a physical entity that we can abstract through the concept of a person, which has a set of associated characteristics. In other words, even though there are many different people, they all share a set of characteristics that define them as people.

These characteristics are what we’ll model as the attributes of the Person entity. These can then be "instantiated," as we saw earlier, resulting in a set of entity occurrences – or in other words, specific people defined by the values of their characteristics or attributes.

To better understand this, we can translate this entity to the logical design level, where, being a single entity, we model it with a single table named Person with the corresponding attributes and data types that match the characteristics of people. In this way, the table schema will be the structure that defines "all people," like a template, while the specific people whose information we want to store in the system correspond to the tuples of the table, which will be inserted as we register people in the system.

For the attributes of the entity, we’ll include those that need to be stored persistently, such as name, date of birth, email, and so on. Among all of them, we choose PersonID as the primary key, which we assume holds the person's government ID. But to illustrate the concept of surrogate key in SQL, in the implementation on the DBMS, we’ll implement the PersonID attribute as a surrogate key instead of the person's actual ID (since both can uniquely identify each person). So each tuple in Person will have a unique and distinct value in that attribute, serving as a superkey, candidate key, and ultimately being selected as the primary key.

In addition to the attributes represented in the entity-relationship diagram, the table we use to model the Person entity has other attributes that help implement associations with other entities, specifically foreign keys.

If we look only at the entity-relationship diagram, we will see a series of associations that "leave" or "enter" the Person entity. In other words, all the relationships this entity has are 1-*, which means the maximum cardinalities on both sides are 1 and *, respectively. These maximum cardinalities tell us how many occurrences of the entities can be related to each other. So with this information, we can determine where to place the foreign keys and which attributes they should reference from specific entities.

In the case of Person, we have 12 associations with such multiplicities, of which only one is a relationship where the "many" side (the side with the maximum cardinality *) is in the Person entity itself. This means that to implement the association between Person and CruiseLine, for example, at the logical level, there should be a foreign key on the many side pointing to the entity on the 1 side. Otherwise, if we place the foreign key in CruiseLine and have it reference Person, its attribute could contain an arbitrary number of references to people, leading to the appearance of a repeating group.

On the other hand, the other 11 associations have the "1 side" in Person, indicating that there are 11 entities that must have a foreign key pointing to Person.

Thus, we know that Person has a foreign key pointing to CruiseLine, even though the attributes that make it up do not appear explicitly in the conceptual diagram. And, since the foreign key has to reference tuples from CruiseLine, it will consist of as many attributes as the primary key of CruiseLine, with the same data types, respectively.

This happens because the foreign key must uniquely reference tuples. So the values of the foreign key attributes should allow us to go to the CruiseLine table, look at the columns of its primary key attributes, and easily find the referenced tuple. So the foreign key in Person will have 2 attributes, not just one.

CREATE TABLE Person (
    PersonID SERIAL PRIMARY KEY,
    Name VARCHAR(32) NOT NULL,
    Birth DATE NOT NULL CHECK (Birth < CURRENT_DATE),
    Email VARCHAR(32) NOT NULL,
    Phone BIGINT NOT NULL CHECK (Phone > 0),
    Nationality VARCHAR(32) NOT NULL,
    NameFK VARCHAR(32),
    FoundationDateFK DATE,
    FOREIGN KEY (NameFK, FoundationDateFK) REFERENCES CruiseLine(Name, FoundationDate)
);
Furthermore, as you can see in its DDL, the attributes (NameFK, FoundationDateFK) that make up the foreign key don’t have the NOT NULL constraint. This is because the foreign key in Person may not reference any tuple from CruiseLine due to the minimum multiplicity of the association on the CruiseLine side (which, being 0, implies that a person might not be a customer of any cruise line).

Semantically, this association, implemented with the foreign key, represents the possibility that a person can be a customer of a certain cruise line, where if they aren’t a customer of any, their foreign key will be NULL.

At the same time, a cruise line does not necessarily have to have any customers, as it can be related to zero people at a minimum, according to the minimum multiplicity on the other side. So with both minimum multiplicities at 0, the association as a whole becomes optional, meaning it may not exist at all, as there is nothing that requires it to exist.

If we look at the relational diagram, to represent this entity or table, it's enough to write it in Datalog notation, with its name and attributes. The only thing to keep in mind is that the attributes that make up the primary key are underlined, and those that represent foreign keys each have an arrow coming from them pointing to the corresponding attribute of the primary key of the entity or table they reference.

In cases like this where the foreign key is composite, each of its attributes has an arrow pointing to the corresponding attribute of the referenced entity. But the order in which the attributes are written in this diagram is not entirely relevant – meaning we can write them in any order as long as we correctly represent which are primary or foreign keys.

Regarding the DDL, since we will consider PersonID as a surrogate key, we declare it as SERIAL so the column stores auto-incrementing values. This way, to uniquely identify each tuple, the attribute will use an integer value that increases by one as tuples are inserted. This allows us to differentiate all of them by that number.

We’ll specify the primary key with PRIMARY KEY, which we can place directly in the attribute declaration if it’s not composite. We’ll specify the foreign key with FOREIGN KEY, indicating which attributes reference the primary key of CruiseLine.

The only thing to be careful about is the order of the attributes. Although you can arrange the foreign key in any order in FOREIGN KEY, in REFERENCES, we must ensure that the attributes of the primary key of CruiseLine are in the same order as those of the foreign key in order to be referenced correctly.

For example, if NameFK should reference Name, then those attributes will occupy the same position in the tuples where we declare the foreign key and the primary key it points to, without needing to appear in a specific position, as long as the correspondence is maintained.

Now let’s look at what a Person can do.

Rental entity
In our domain, people can rent bikes, and for each bike rental, we want to store certain information like the time the rental occurred, the duration in hours, the price per hour, and so on. So if we modeled this as an M-N association between Bike and Person, we couldn't store all this information unless we used an associative entity (which is only valid when the entity itself is weak in identification). But here we prefer to use a surrogate key to uniquely identify the rentals, which avoids making the entity representing them weak in identification.

This is necessary because each rental requires storing associated information, in addition to the person and the bike involved. So an we’ll introduce an entity that relates to both Bike and Person through 1-* associations (each Rental associates a bike with a person), storing information about that "event." Then, as it has two associations with the many side in Rental, this entity will have two foreign keys – one to implement each association. One will reference the primary key of the Bike entity and the other the primary key of the Person entity.

Here, we need to distinguish between both foreign keys, as each is composed of one attribute, unlike the previous case where Person had only one foreign key composed of several attributes. That is, regardless of the attributes that comprise each foreign key, it’s important to distinguish that one aims to uniquely identify a bike while the other uniquely identifies a person.

CREATE TABLE Rental (
    RentalID SERIAL PRIMARY KEY,
    StartTimestamp TIMESTAMP NOT NULL,
    Duration INT NOT NULL CHECK (Duration >= 0), /*Duration of the rental period in hours*/
    HourPrice DOUBLE PRECISION NOT NULL CHECK (HourPrice >= 0),
    BikeFK INT NOT NULL,
    PersonFK INT NOT NULL,
    FOREIGN KEY (BikeFK) REFERENCES Bike(BikeID),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID)
);
When writing your DDL, the attributes are declared the same as before – the main difference here being that each foreign key has its own FOREIGN KEY constraint, which references the primary key attribute of the corresponding table. This is the case because here, both Bike and Person have primary keys with a single attribute.

Another important detail to consider is the minimum multiplicity on the Person and Bike sides in the associations of the conceptual diagram, where the 1 side of the associations has a minimum multiplicity of 1. This means that a Rental must always be associated with a person and a bike, so their foreign keys can never be NULL. This is why the NOT NULL constraint is used in the attributes.

As before, at the conceptual level, we don’t show the attributes that form the foreign keys, as the associations themselves and their cardinalities implicitly indicate the existence of foreign keys. But in the relational diagram, we do show these attributes, where arrows indicate the primary key attributes of other entities to which they point. And, since the entity is not weak in identification, none of the foreign key attributes should be underlined.

Regarding the other constraints, we don’t allow any attribute to be NULL, as it doesn't make sense for a timestamp to be null, for example, when it’s precisely the valuable information about a rental that we want to store in the database. The other attributes also have constraints like non-negativity, since the duration or the hourly rate can’t be negative amounts.

This way, if someone tries to insert negative values for these attributes, the DBMS will automatically know that the inserted data isn’t valid or correct, since the actual numbers for duration and price can never be negative. This implies that the values for those attributes must be positive to be correct.

CarOwnership entity
Another entity related to Person in the diagram – that is, representing something else a Person can do – is CarOwnership. This aims to model that people can have cars, whether bought, rented, or leased. For this, we use the same conceptual structure as with Rental, where a person can have multiple cars and a car can belong to many people.

As before, this implicit N-M association between Car and Person must store information about the ownership, such as its type, start date, price, and so on. So we’ll use an intermediate entity with 1-* associations towards both entities, with the 1 side on them.

CREATE TYPE CarOwnershipType AS ENUM('buy', 'rental', 'lease');
CREATE TABLE CarOwnership (
    InsuranceID SERIAL PRIMARY KEY,
    BuyDate TIMESTAMP NOT NULL, /*Date when ownership starts*/
    BuyPrice DOUBLE PRECISION NOT NULL CHECK (BuyPrice >= 0), /*Ownership price, if rental or lease, this price represents a monthly amount*/
    WarrantyEndDate DATE NOT NULL CHECK (WarrantyEndDate >= DATE(BuyDate)),
    OwnershipType CarOwnershipType NOT NULL,
    PlateFK VARCHAR(32) NOT NULL,
    PersonFK INT NOT NULL,
    FOREIGN KEY (PlateFK) REFERENCES Car(Plate),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID)
);
The table implemented at the logical level is very similar to Rental, as we have a surrogate key that uniquely identifies the tuples, thus preventing the entity from being weak in identification. You can see this directly in the conceptual diagram. There, we have an attribute marked with {id} that we provide with semantics equivalent to that of a surrogate key. This means we don't need its identification to depend on any other entity.

In other words, at the conceptual level, InsuranceID is a unique identifier provided by an insurance company. To to generate it, they likely used a technique similar to SQL's SERIAL auto-increment, although it doesn't necessarily have to be that, as there are many others with very specific applications.

The value of InsuranceID might be provided to us when inserting tuples into our system, where this value would have to meet the primary key constraint and not repeat for any pair of possible tuples. But still, we decided to implement it with a SERIAL to make the generation of synthetic data for this database simpler.

Just keep in mind that, in a real situation, if we are provided with this value, we should avoid using SERIAL and save the identifier that each tuple has. Since InsuranceID is the primary key, no pair of tuples can have the same value in this attribute, but they can have the same start date, price, and so on.

In this table, to restrict the values that the attribute OwnershipType can take, instead of using a CHECK, we’ll create a new data type. We could have done this perfectly using a CREATE DOMAIN. But instead, we’ll use a TYPE ENUM to show another way of defining the set of values an attribute can take. It defines the possible values for the attribute, representing an ownership where a person buys, rents, or leases a car. Finally, that TYPE ENUM is assigned as the data type of the attribute.

We’ve implemented the most basic domain constraints and problem requirements here, which only involve the CarOwnership table itself. For example, we have those requiring the price to be positive or the warranty end date to be after the ownership start date.

On the other hand, we can see that the attribute BuyDate has been assigned a TIMESTAMP data type, which doesn't exactly match the attribute's name. In this example, such details aren’t as important, since the TIMESTAMP was declared this way to provide a time in addition to the date of purchase. But in a real project, you should be stricter with naming attributes according to their characteristics. This will help improve schema clarity and make database management easier.

Residence entity
A person can also reside in a city, so our database must be able to store information about a person's stay in a certain city. We’ll do this using the entity Residence, which functions similarly to the previous entities Rental and CarOwnership, but with some differences.

First, the attributes it stores are:

the start date of a person's stay in a city (which can’t be null because if the stay exists, it must have started on a date),

the end date of the stay (which can be NULL because the person may reside in the city for an indefinite amount of time), and

the address where they reside within the city.

When the EndDate attribute is NULL, it means the person is still residing in the city, as the end date of the stay is not defined. Also, if this date exists and is later than the current date, we can also know that the person still lives in the city until the specified date.

This has implications for identifying the Residence entity, as there is no set of attributes within the entity itself that uniquely identifies the tuples of Residence. Instead, it’s the start date, along with the references to the person and city, that together uniquely identify it. So since the identification of the entity depends on other entities, Residence is a weak entity in terms of identification.

These references work similarly to what we saw earlier in Rental, for example, where we had several 1-* associations with the many side in the Residence entity. This implies that for each association, the foreign key is located in the Residence entity, pointing to the entity on the other side of the association.

Since there are two such associations in total, there are two foreign keys, each formed by one attribute, because the primary keys of the entities they point to are also formed by a single attribute.

CREATE TABLE Residence (
    StartDate DATE NOT NULL,
    EndDate DATE CHECK (
        EndDate IS NULL
        OR EndDate >= StartDate
    ),
    Address VARCHAR(32) NOT NULL,
    PersonFK INT NOT NULL,
    CityFK INT NOT NULL,
    PRIMARY KEY (StartDate, PersonFK, CityFK),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID),
    FOREIGN KEY (CityFK) REFERENCES City(CityID)
);
If we look at the relational diagram, we’ll see that the table implementing this entity has its foreign keys underlined because they’re part of the primary key. This helps us identify that the corresponding conceptual entity is weak in identification, with some of its primary key attributes being foreign keys.

Also, if we wanted to reconstruct the conceptual entity from the relational diagram, it would be enough to look at the table's foreign keys, which other entities they reference, whether their attributes are underlined or not, and any possible constraints indicated in the relational diagram.

With this, if any of the foreign keys are underlined, the entity is necessarily weak in identification, and the «weak» role would be specifically placed on the association modeled by that foreign key. The many side of that association would be placed on the side of the entity from which the foreign key originates. And we wouldn’t include its foreign key attributes in the conceptual diagram entity.

In its DDL, we can see that the primary key is composed of StartDate along with the foreign key attributes, where each one represents a different foreign key pointing to a certain entity like Person or City – hence the addition of two FOREIGN KEY constraints. We’ve also added the NOT NULL constraint to both foreign keys due to the minimum multiplicity of the 1 side of the associations, which requires a Residence tuple to relate a person with a city. If we had 0..1 instead of 1..1 on those sides of the associations, then each foreign key of Residence might not reference any person or city, meaning it could be NULL.

Regarding the remaining constraints, no attribute can be null except EndDate. If it’s not NULL, then the date it stores must be after the date the residence began, as it wouldn't make sense for it to be earlier than the start date.

ShipAssignment entity
Another entity that is practically the same as the previous one is ShipAssignment, responsible for modeling the assignment of certain cruise ships to cruise lines. That is, a cruise can belong to or be assigned to a cruise line that operates it under its brand for a certain period, just like a person can reside in a city for a certain period.

Being a weak entity in identification, as we can see in its conceptual diagram, we could have represented it with an associative entity and an N-M association between CruiseShip and CruiseLine. But to be consistent with the notation we used in Residence, we won’t use an associative entity. Instead, we’ll have the entity interpose in the N-M association, resulting in two 1-* associations with the many side in ShipAssignment.

This implicitly indicates that there are two foreign keys pointing to CruiseShip and CruiseLine, respectively.

Also, note that just focusing on the many side (which is an easy rule to apply to determine where to place foreign keys just by looking at the conceptual diagram) isn’t by itself a good practice without further consideration. When you have a conceptual diagram, you should look at all the elements of the entity to make informed and reasoned decisions about its logical design.

CREATE TABLE ShipAssignment (
    StartDate DATE NOT NULL,
    EndDate DATE NOT NULL CHECK (EndDate >= StartDate),
    NameFK VARCHAR(32) NOT NULL,
    FoundationDateFK DATE NOT NULL,
    ShipFK INT NOT NULL,
    PRIMARY KEY (StartDate, NameFK, FoundationDateFK, ShipFK),
    FOREIGN KEY (NameFK, FoundationDateFK) REFERENCES CruiseLine(Name, FoundationDate),
    FOREIGN KEY (ShipFK) REFERENCES CruiseShip(ShipID)
);
Here, we assume the end date of the assignment is always defined, meaning cruises are assigned to cruise lines through "contracts" that always start and end on specific dates, and assignments don’t last indefinitely. This implies that EndDate can never be NULL. So in the DDL, we include the NOT NULL constraint and a CHECK to ensure that EndDate is after the start date, guaranteeing that only valid tuples are inserted into the database.

The foreign keys are formed solely by the attribute ShipFK, which refers to the CruiseShip entity. We use it to reference the cruise assigned to a certain cruise line. But the other foreign key, which is used to implement the other 1-* association, is composed of the attributes (NameFK, FoundationDateFK), which refer to the primary key of CruiseLine – and this, in turn, is composite and contains two attributes (Name, FoundationDate).

If we only look at the relational diagram, we’ll see that three attributes are part of foreign keys because there are arrows coming from them. Specifically, the arrow from one of them (ShipFK) will point to an attribute in a certain table. So we know that this attribute forms a foreign key by itself, while the other two have arrows pointing to attributes of another different entity (but both referencing the same one).

So together, they form another foreign key because the entity or table they reference is different from the one referenced by the other attribute ShipFK.

These attributes, in turn, serve to uniquely identify each tuple in ShipAssignment – because, with just the start and end dates, we can’t distinguish between any possible pair of tuples.

For example, if several ships are assigned to the same cruise line during the same time period, the start and end dates will match in both tuples, but they’ll represent different assignments even though the dates are the same. So the primary key of the table includes the attributes of the foreign keys, so that their values can distinguish any pair of tuples we might have in the table. Specifically, we include the foreign keys because a cruise can be or have been assigned to several cruise lines, just as a cruise line can have had multiple cruises assigned to it.

The values of both foreign keys are necessary to uniquely identify this "event" between a cruise and a cruise line, because according to the domain, the same cruise can’t be assigned multiple times to the same cruise line on the same date.

CREATE ASSERTION EveryCruiseLineHasAssignment CHECK (
    NOT EXISTS (
        SELECT *
        FROM CruiseLine cl
        WHERE NOT EXISTS (
                SELECT *
                FROM ShipAssignment sa
                WHERE sa.NameFK = cl.Name
                    AND sa.FoundationDateFK = cl.FoundationDate
            )
    ) 
);
Lastly, given the minimum multiplicity of 1..* on the ShipAssignment side, we need to implement a constraint to ensure that all cruise lines have at least one cruise assignment, which is always associated with a cruise.

To do this, we can use either an ASSERTION or a TRIGGER, as this is a constraint involving multiple tables. But for simplicity, we’ll assume that the data inserted always meets this constraint. This means we don’t need to include assertions and triggers in the DDL.

Now let’s discuss some other important entities in our system.

City entity
This entity is similar to Person and is used to store information about cities in the system. Specifically, for each city, it stores its name, the country where it’s located, population, area, and coordinates in latitude and longitude. Each physical city in our domain within the system will be represented by a tuple in the City table, which is how we implement this entity at the logical level.

Of all the associations that this entity has, none are of the 1-* type with the many side in City. Instead, they all have their 1 side in City. This means there will be exactly 4 foreign keys from other entities pointing to City, but the City table itself won’t have any foreign keys pointing to another entity.

This might not be straightforward to see at the conceptual level, as we need to look at the maximum cardinalities of the associations to know which ones result in foreign keys in the entity we’re implementing.

In contrast, in the relational diagram, this is more direct. References implemented with foreign keys are arrows, so we can directly know how many arrows refer to the primary key of a certain table or how many point to other tables, also clearly indicating from which attributes and tables they originate.

CREATE TABLE City (
    CityID SERIAL PRIMARY KEY,
    Name VARCHAR(32) NOT NULL,
    Country VARCHAR(32) NOT NULL,
    Population INT NOT NULL CHECK (Population >= 0),
    Area DOUBLE PRECISION NOT NULL CHECK (Area >= 0),
    Latitude DOUBLE PRECISION NOT NULL CHECK (
        Latitude BETWEEN -90 AND 90
    ),
    Longitude DOUBLE PRECISION NOT NULL CHECK (
        Longitude BETWEEN -180 AND 180
    )
);
Regarding the DDL, we implement the identifier CityID with a SERIAL surrogate key, as at the conceptual level we have defined that the attribute CityID is the primary key of City.

It's important to note that when modeling a domain or solving a problem for a client, we might be required to use identifiers specific to the domain we are modeling, which would mean CityID would be of the same type as the identifier to be stored. But for simplicity, let’s assume that we construct the city identifiers ourselves using a surrogate key.

In addition to the NOT NULL constraints that prevent all attributes from being NULL, since it doesn't make sense for a city to have no name or a defined population number, we impose a restriction on the range of values that Latitude and Longitude can take. This is to ensure the values are valid, even though we can't verify if they are correct, as this mainly depends on the data source.

To do this, we can use the BETWEEN operator, which performs the same check as Latitude >= -90 AND Latitude <= 90 but in a more readable way.

Port entity
In addition to cities, our domain also includes ports, which are represented by the entity Port. Like before, each port will be a tuple in the table, with its primary key composed of the port's name stored in the Name attribute and a foreign key that references City, modeling the city where the port is located.

We can infer the existence of this foreign key by looking at the entity's associations, where all of them are of the 1-* type, and only one has the many side in Port. This precisely models this relationship between Port and City. The others have their 1 side in Port, indicating that they point to Port, meaning they reference some tuple in the Port table.

At the same time, the foreign key of Port is also part of its primary key because a port can’t be identified by its name alone – we also need to know the city where it’s located.

For example, in this domain, we assume that there can be several ports with the same name, but not located in the same city. So if two ports are in the same city, according to the domain, we have the guarantee that their names can’t be the same. This allows us to define the primary key as the combination (Name, CityFK).

We’re making these assumptions here as an example, but in a real project they would need to be confirmed with domain experts and the client's requirements to ensure they are met. This would allow us to make design decisions such as establishing the keys of an entity. So once we know that Port has a foreign key that is part of its primary key, we know that the entity is weak in identification. In the relational diagram, we will have to underline not only Name but also the CityFK attribute.

CREATE TABLE Port (
    Name VARCHAR(32),
    TerminalCount INT NOT NULL CHECK (TerminalCount >= 0),
    MaxShipLength INT NOT NULL CHECK (MaxShipLength >= 0),
    Area DOUBLE PRECISION NOT NULL CHECK (Area >= 0),
    CityFK INT NOT NULL,
    PRIMARY KEY(Name, CityFK),
    FOREIGN KEY (CityFK) REFERENCES City(CityID)
);
The DDL is similar to the previous ones: we have the declaration of attributes and constraints like PRIMARY KEY, where the set of attributes (Name, CityFK) is defined as those that uniquely identify the tuples of Port. We also have the corresponding FOREIGN KEY that references the CityID attribute, the primary key of the City table.

A peculiarity of this CREATE TABLE statement is that we don’t add a NOT NULL constraint to the Name attribute because we don’t need to declare it explicitly in this case. That is, since Name is part of the primary key, and a primary key never allows NULL values in its attributes, we can skip declaring NOT NULL, as PRIMARY KEY does so implicitly to ensure the primary key integrity constraint.

This also applies to the foreign key attribute, which can’t be NULL due to the minimum cardinality (minimum cardinality 1 in 1..1) on the City entity side, which requires all ports to be associated with a city. But to more clearly reflect this minimum cardinality, we add NOT NULL explicitly to the CityFK attribute, even though it’s not strictly necessary.

Lastly, if we want to ensure that the logical design represented in the relational diagram is correct with respect to the conceptual diagram, we can try reconstructing the conceptual entity from the table in the relational diagram.

To do this, after creating the entity with its name and attributes (except for those that are foreign keys), we have to infer the associations implemented through these foreign keys. So for each of them, we introduce an association that relates Port to the entity the foreign key points to, where the many side is on Port and the 1 is on the other entity's side.

In addition to the maximum cardinalities 1 and *, we also have to define the minimums, which we can determine through the constraints indicated in the relational diagram.

For example, if one of the foreign keys can be NULL, then its minimum cardinality on the 1 side of the association will be 0, resulting in that side having a cardinality of 0..1.

On the other hand, if it can’t be NULL, the minimum cardinality is 1. On the other side of the association, we’ll default to a minimum cardinality of 0 unless there are constraints requiring cities to have at least one port, for example. This means the minimum cardinality would be 1, resulting in the Port side of the association having a cardinality of 1..*.

Finally, we can repeat this process with the foreign keys that point to the primary key of Port, leading to more associations with other entities.

For example, if we are reconstructing the conceptual entity City from the relational diagram, we will see that there is a foreign key from Port pointing to CityID of City. So City will have a 1-* association with Port, where the many side is on the Port side because the foreign key originates from Port.

In this way, when we have fully reconstructed the conceptual entity, we’ll determine if it’s weak in identification by checking if any of its foreign keys is underlined. This means it’s also part of the primary key. In that case, we’ll add the role «weak» to the associations that have arisen from these foreign keys, always on the side from which the foreign key originates.

CruiseLine entity
This entity is responsible for representing cruise lines in our system, which can have customers and cruises assigned. Conceptually, this entity is very similar to those we have already seen, as it has a primary key made up of two attributes of the entity itself, and no foreign keys pointing to other entities. But there are foreign keys in other entities that point to CruiseLine, which we can see from the 1-* type associations.

CREATE TABLE CruiseLine (
    Name VARCHAR(32) NOT NULL,
    FoundationDate DATE NOT NULL,
    ContactPhone BIGINT NOT NULL CHECK (ContactPhone > 0),
    Rating DOUBLE PRECISION NOT NULL CHECK (Rating >= 0),
    PRIMARY KEY (Name, FoundationDate)
);
Specifically, the primary key of this entity is made up of the company name and the foundation date. This combination of values might seem unique across the tuples we can store in the table, as it’s very unlikely that multiple cruise lines with the same name would be founded on the same date. But we shouldn’t make these assumptions ourselves – instead, we have to ensure that these conditions are met with the client, target user, or domain experts of our system.

Here, for simplicity, we directly assume that no cruise line has the same name as another founded on the same date, but you should always verify if this holds true in the domain.

So we set (Name, FoundationDate) as the primary key, which in turn imposes the implicit NOT NULL constraint on both attributes (meaning we don’t need to declare it explicitly). In the DDL, we can also see that the ContactPhone attribute is not of type INTEGER, but BIGINT. This is because phone numbers are usually long numbers representing large numeric quantities that would exceed the range representable by a more basic type like INTEGER. For text-type attributes, a fixed maximum length of 32 characters is used for all strings, which is enough to accommodate any cruise line name or similar information.

We could also represent the phone number with a string, allowing the storage of the country code in text format, but this can complicate processing since the number would need to be parsed from text.

Vehicle entity
In our domain, there can be certain types of vehicles, such as cars, cruise ships, bikes, or city buses. They all share a series of common characteristics like model, weight, color, or odometer reading to know how far they have traveled since they were manufactured.

These attributes are common to all vehicles in our domain, as they will always have a model name or weight, among other things, regardless of the type of vehicle they are. Becuase of this, we’ve decided to abstract these common characteristics in the conceptual design into a superclass entity called Vehicle. And from this, all entities representing specific types of vehicles must inherit.

In other words, at a conceptual level, we have an IS-A hierarchy where the parent entity is Vehicle, which contains all the characteristics that define all vehicles. From it, a series of entities inherit that represent specific types of vehicles (where each has more specific characteristics of the corresponding vehicle type).

In summary, we use an IS-A hierarchy because we need to model a situation where a series of "individuals" in our domain share a set of common features. Formally, an IS-A hierarchy can be defined as a specialization/generalization relationship between a superclass entity and some inheriting entities. The inheriting entities are composed of all the characteristics or attributes of the superclass plus some of their own attributes.

But, practically, what matters to us is that a hierarchy allows us to have a superclass (the Vehicle entity in this case) where we have attributes corresponding to these common characteristics, and then a series of entities that inherit from it and represent specific types of individuals (each having specific characteristics depending on their type).

With this, we gain clarity and maintainability in the diagram, as adding a new common characteristic to all vehicles only requires adding it to Vehicle – not to each and every inheriting entity. Similarly, if a new type of vehicle needs to be added to the system, we won’t need to include all the common attributes of vehicles in that entity.

How is this IS-A hierarchy implemented with tables?
At this point, we need to decide how to implement the hierarchy using tables in the logical model. Specifically, we have to determine the number of tables to use and the keys each will have concerning the implementation of the hierarchy itself.

To start, it's important to see that Vehicle has VehicleID as its primary key, which we assume is a surrogate key. With this, we know that if we had to implement any table for the inheriting entities, they should have a foreign key pointing to VehicleID, as it’s the primary key that can uniquely identify tuples of Vehicle.

We see that the hierarchy here is complete and disjoint. It’s complete because all the "individuals" in the hierarchy must always be represented by the inheriting entities. In other words, we will never find a vehicle that only has the attributes of Vehicle – instead, all vehicles in our domain are necessarily of one of the types defined in the inheriting entities (or so we assume). It’s disjoint because a vehicle can’t be of multiple types at once, meaning it can’t be both a car and a cruise ship, which makes sense.

All this means that each of them will be implemented with a specific table. Our system stores many types of vehicles and will likely need to expand with even more types of vehicles. To simplify this process of adding new types of vehicles and to avoid the appearance of too many NULL values in tables, we’ll implement a table for each inheriting entity of the hierarchy.

For the superclass, we’ll also implement a specific table, as each vehicle that exists in our system will be represented in one of the tables of the inheriting entities – but it’ll need to take values in the characteristics (attributes) of the superclass.

Here, we have several options. One option is not to implement a table for the superclass, duplicating all its attributes in each of the tables of the inheriting entities. This is easy to understand and initially seems practical, but it has significant drawbacks.

Another option is to implement a table for the superclass and include a foreign key in all the inheriting entities that point to the primary key of Vehicle.

We can easily dismiss the first option because duplicating attributes in all the tables for different types of vehicles leads to a lot of redundancy at the metadata level or schema, meaning duplicated attributes in multiple tables without a clear need for duplication.

Beyond the redundancy issue, duplicating the same attributes in multiple tables makes certain schema modifications more complicated. For example, adding an additional common feature in Vehicle would require adding an attribute in each table. Or we could change how common attributes like color are represented, such as switching color names from uppercase to lowercase (or any change in their representation). We’d need to make these changes across all the vehicle type tables.

With the other option, we implement a specific table for the superclass, avoiding these problems by centralizing the storage of common features in a single table. This makes it easier to perform the operations mentioned before, or even additional ones like counting how many vehicles are in our system.

We can easily do this by counting the tuples in the Vehicle table, instead of adding up the tuple counts from each of the tables for different types of vehicles. We can resolve this query this way because all vehicles will have a tuple in Vehicle that stores the common features, as well as one in their specific vehicle type table that stores the rest of the features defining it as a car, cruise, bike, and so on.

In this tuple, there’s a foreign key that references the tuple in the superclass table, thus associating the information from both tuples so it can query it and know all the information about a vehicle – both its common features to all vehicles and the specific ones of its type.

CREATE TYPE ColorType AS ENUM ( 'red', 'green', 'blue', 'yellow', 'black', 'white' ); 
CREATE TABLE Vehicle (
    VehicleID SERIAL PRIMARY KEY,
    Model VARCHAR(32) NOT NULL,
    Weight DOUBLE PRECISION NOT NULL CHECK (Weight >= 0),
    Color ColorType NOT NULL,
    Odometer DOUBLE PRECISION NOT NULL CHECK (Odometer >= 0)
);
Finally, we decide to implement a table for all entities in the hierarchy, using foreign keys in the tables of the inheriting entities to reference tuples in Vehicle that store the common features of the vehicles.

In its DDL, we can see that the primary key is implemented with an attribute of type SERIAL because it’s a surrogate key. For the Color attribute, we create a TYPE ENUM with the possible colors in our system. This is a good practice because if we need a color attribute in more areas of the system, we’ll have its domain (or data type) defined in ColorType. And this allows us to reuse it and ensure that all color attributes can take values from exactly the same data set.

But if we try to reconstruct the IS-A hierarchy from the entity-relationship diagram just by looking at the implementation represented in the relational diagram, we’ll realize that it’s somewhat more complex than what we saw before.

This is because there’s not a single way to translate an IS-A hierarchy into a relational diagram. Depending on the semantics of the features and entities, plus the system requirements, it may be better to use more or fewer tables to implement it. But in cases like this where we have a table for each entity in the hierarchy, we can clearly see that there’s a Vehicle table with a primary key VehicleID (which is referenced by multiple tables, each having exactly the same foreign key referencing Vehicle).

If we only look at this, we might think that Vehicle is an entity that has 1-* associations with other entities – and this is entirely possible when looking only at the relational diagram.

But to derive the conceptual design from the logical one and infer the existence of an IS-A hierarchy, we have to focus on the semantics of the tables and attributes. That's where we'll see that Vehicle contains attributes common to all types of vehicles that have foreign keys pointing to Vehicle. This gives us clues that Vehicle could be the superclass of a hierarchy, and the rest of the tables with foreign keys pointing to Vehicle could be inheriting entities.

But inferring the existence of an IS-A hierarchy in the conceptual design simply by observing the logical implementation is not always unequivocal. This is because, for example, here we could perfectly consider that Vehicle is an entity associated with the other inheriting entities through 1-* type associations. This would also be correct from a conceptual and logical point of view.

Still, even though conceptually we can transform the hierarchy into a series of 1-* associations between Vehicle and the other entities, this is only true to the implementation if we implement one table per entity. Otherwise, we wouldn’t be correctly reflecting in the conceptual design what is actually implemented in the logical one.

In summary, when we see an IS-A hierarchy, it doesn't necessarily mean there are foreign keys between the inheriting entities and the superclass, as not always as many tables as entities are used to implement the hierarchy. So to reconstruct a hierarchy at the conceptual level from the logical one, the most reliable thing to focus on is the constraints, notes, or indications left in the relational diagram explaining why certain tables were implemented – that is, where they come from.

Implementing a hierarchy at the logical level usually involves a series of design decisions that must be properly justified, which we can then use to infer the existence of the hierarchy at the conceptual level.

This exercise of trying to reconstruct the conceptual level is important to approach clearly, as understanding this reverse process is key to comprehending the elements of the different design levels and how they translate from one to another.

CruiseShip entity
To illustrate how we’ve implemented the IS-A hierarchy of Vehicle, let's look at the different inheriting entities that make it up.

Part of the entity-relationship diagram where the IS-A hierarchy of vehicles is represented according to their type. Image by author. 

First, we have CruiseShip, which models the existence of cruise ship-type vehicles in our system, where each cruise ship is a tuple in the table. Regarding the specific features of the cruise ship that make it a cruise ship-type vehicle, we have its length, passenger capacity, or even the speed at which it travels. It also has features that all vehicles must have in the Vehicle table, such as model, color, and so on, specifically in tuples that store the values of each cruise's features.

To relate this information from both tables, there is a foreign key in CruiseShip that points to Vehicle, meaning it references the tuple in Vehicle where these feature values are stored, for each cruise ship (tuple of CruiseShip).

This way, we ensure that the attributes repeated in all vehicle types are centralized in one table where they can be easily modified or consulted, much better than having them all duplicated in the different tables of vehicle types.

CREATE TYPE ClassType AS ENUM('first', 'second', 'third', 'economy');
CREATE TABLE CruiseShip (
    ShipID SERIAL PRIMARY KEY,
    Speed DOUBLE PRECISION NOT NULL CHECK (Speed >= 0),
    Length DOUBLE PRECISION NOT NULL CHECK (Length >= 0),
    PassengerCapacity INT NOT NULL CHECK (PassengerCapacity >= 0),
    Class ClassType NOT NULL,
    VehicleID INT UNIQUE NOT NULL,
    FOREIGN KEY (VehicleID) REFERENCES Vehicle(VehicleID)
);
In the DDL, we see that the attributes and their types are declared, where ShipID is defined as a surrogate key using the SERIAL data type. This allows us to uniquely identify each cruise ship. But since every cruise ship is also a vehicle, we could also identify it by making its primary key {VehicleID}, because this attribute, even though it’s a foreign key, will never be NULL since a cruise ship needs to have the features that classify it as a vehicle.

So the foreign key must reference a valid tuple in Vehicle where the values for the features common to all vehicles are stored. Consequently, VehicleID is an alternative key declared with the UNIQUE constraint, although we aren’t required to add this constraint since the surrogate key ShipID is sufficient to identify it.

The important thing about this attribute is to correctly define the NOT NULL and FOREIGN KEY constraints, ensuring it correctly references the primary key VehicleID of the Vehicle table.

In the conceptual design, we see that this entity has multiple 1-* associations, which indicate that there are three foreign keys from other entities pointing to CruiseShip. But if we only have the conceptual design, we can’t say anything about the possible foreign key generated by the IS-A hierarchy. That is, if we only have the conceptual diagram, we can’t "guess" how many tables have been used to implement the hierarchy – we only know that after creating the logical design. At most, we could consider all possible options for implementing the hierarchy and, for each one, analyze whether there is a foreign key coming from CruiseShip.

But if in addition to the entity-relationship diagram we know that there is a foreign key originating from CruiseShip and pointing to another entity, then the entity it points to must necessarily be Vehicle. This is because 1-* type associations are elements that we know will generate foreign keys. But certain types of associations like 1-1 or 0..1-0..1 can lead to ambiguities, as we have seen before when trying to infer the existence of a hierarchy at the conceptual level.

So by discarding entities related through 1-* associations, the only option left would be Vehicle. With all this, we can also know that the implementation of the hierarchy at the logical level has been done by creating a table for the superclass and for the CruiseShip entity – but we couldn’t be sure whether the other entities have also been implemented with a table or not, as that heavily depends on the semantics.

Bike entity
Continuing with the different types of vehicles, we also have bicycles represented in the entity Bike, which inherits from Vehicle. Here, it’s clearer that the attributes of the inheriting entities are more specific than those of the superclass, as only bikes have features like FrameHeight or Foldable.

If we only look at the conceptual diagram, we can’t be certain if Bike has a foreign key pointing to Vehicle. This is precisely because, as mentioned before, without knowing the specific implementation at the logical level, we can’t guarantee that there is a foreign key in Bike. But considering the semantics of the hierarchy, we could propose the different options available for implementing it and determine in each case whether such a foreign key exists.

CREATE TABLE Bike (
    BikeID SERIAL PRIMARY KEY,
    Electric BOOLEAN NOT NULL,
    Foldable BOOLEAN NOT NULL,
    HasLights BOOLEAN NOT NULL,
    FrameHeight DOUBLE PRECISION NOT NULL CHECK (FrameHeight >= 0),
    VehicleID INT UNIQUE NOT NULL,
    FOREIGN KEY (VehicleID) REFERENCES Vehicle(VehicleID)
);
Since we decided to use a table to implement each table in the hierarchy, in this case, there is indeed such a foreign key, just as in CruiseShip. And we can see it declared in the same way as the FOREIGN KEY constraint.

Also, the primary key of Bike is not the foreign key that uniquely identifies the vehicles. Instead, it’s the BikeID identifier. Here we’re assuming that our domain requires each type of vehicle to have its own identifier. That is, in addition to the VehicleID identifier that serves for any type of vehicle, each of these types must have its own type-specific identifier. This means that cruise ships, bikes, cars, and buses will each have a way to identify themselves (even though all of them can be distinguished from each other by the VehicleID identifier they possess indirectly through their foreign key referencing Vehicle. This is why the foreign key attribute is declared as UNIQUE.).

And since this foreign key is not part of the primary key, the entity is not weak in identification. Even if it were, it wouldn’t be marked in any way at the conceptual level. This is because at that level, we have a hierarchy of entities that can be implemented at the logical level in many ways, and not all of them will have entities weak in identification.

Entity-relationship diagram with inheritance where Bike and Car are subclasses of Vehicle. Image by author. 

To understand this, we can consider a simpler example of a hierarchy with only two inheriting entities (as you can see in the diagram above). If we only have the conceptual design, we still won't know which tables we'll use to implement the hierarchy – although we know we have several options, such as:

implementing or not implementing a table for the superclass

implementing a table to represent all inheriting entities, or just one table for each entity

or even more complex implementations like using a single table for the superclass and some of the inheriting entities.

Each of these options has its peculiarities. If we don't implement a table for the superclass, then there will necessarily be no foreign keys pointing to it. Or if we decide to create a table to represent the superclass and some inheriting entity together, then that inheriting entity won’t have any foreign key pointing to the superclass.

Regarding weakness in identification, depending on whether we are required to have each type of vehicle with its own identifier, we could have a global identifier in the superclass, or as in our diagram, multiple identifiers, one for each type of vehicle in addition to the Vehicle superclass identifier that identifies any vehicle. So we see that weakness in identification does not always exist, as it mainly depends on the domain and the requirements of the problem.

For example, if we see identifiers in each of the inheriting entities, and we know that those identifiers can serve as primary keys, then this suggests that the inheriting entities may have been implemented with a table each, where their foreign key initially does not include other foreign key attributes. But the conceptual diagram can’t guarantee this, as the existence or absence of a foreign key that may or may not be part of the primary key when implementing a hierarchy is a design decision specific to the logical level.

Another aspect we can infer from the conceptual diagram is the 1-* associations where the 1 is in one of the entities of the hierarchy. Necessarily, if any foreign key points to the superclass (the 1 side of the association is in the superclass), then a table must be implemented for it.

On the other hand, if it points to one of the inheriting entities, it’s not a sufficient condition to infer that there is a table for that inheriting entity. This is because there may be a table representing the superclass along with that inheriting entity, with the foreign key perfectly pointing to the identifier of that table.

So in the IS-A hierarchies of the conceptual model, the "weak" role is never used to indicate possible identification weakness that the tables implementing the entities might have. There are many ways to implement the hierarchy with tables, and the chosen method is not 100% determined by the conceptual diagram.

But it’s very important to be clear that the entities in the hierarchy can have associations with other entities that make them weak in identification. In that case, even though they are part of an IS-A hierarchy, the "weak" role would be used to indicate that the entity is weak in identification (but not due to the hierarchy – rather because of an association with another entity).

Car entity
Similar to the previous entities, we have Car, which represents the existence of cars in the system. Its primary key is Plate, which we assume is unique for each car. As we can see in the DDL, the data type of Plate is VARCHAR. This makes it perfectly possible for the attribute to be part of a primary key, as they don’t necessarily have to be integers or numeric to be part of a key. Any data type whose values are unique across the tuples stored in the table can serve.

CREATE TYPE CarFuelType AS ENUM ( 'gas', 'diesel', 'electric', 'hybrid', 'hydrogen' ); 
CREATE TABLE Car (
    Plate VARCHAR(32) PRIMARY KEY,
    FuelType CarFuelType NOT NULL,
    DoorCount INT NOT NULL CHECK (DoorCount >= 0),
    TrunkCapacity INT NOT NULL CHECK (TrunkCapacity >= 0),
    HorsePower INT NOT NULL CHECK (HorsePower >= 0),
    Doors INT NOT NULL CHECK (Doors > 0),
    AirConditioning BOOLEAN NOT NULL,
    VehicleID INT UNIQUE NOT NULL,
    FOREIGN KEY (VehicleID) REFERENCES Vehicle(VehicleID)
);
Regarding the foreign keys related to this entity, we have the same one as before, which serves to reference a tuple of Vehicle that stores information about the car model, color, and so on. But looking at the conceptual diagram, we can see that there are two other foreign keys in other entities pointing to Car, since the 1 side of the corresponding 1-* associations is in Car.

Even though we can’t see this in the DDL of the Car table, these foreign keys are in other entities referencing Car. But we can see them in the relational diagram as arrows pointing to the attributes of the primary key of Car.

Lastly, we also create an ENUM TYPE to restrict the domain of the FuelType attribute. We could implement this perfectly with a constraint, but to reuse this data type in other entities that might need it, we should define a DOMAIN or an ENUM TYPE (as in this case, that can be assigned as a data type to the attribute).

Also, defining a set of values this way is especially useful when the attribute holds text, as in other numeric attributes it may be easier to restrict their possible values with conditions like (HorsePower >= 0).

CityBus entity
To finish with the vehicle hierarchy, we have CityBus, which represents city buses in our domain. In this entity, we also have Plate as the primary key, which stores the bus's license plate and serves to uniquely identify it (meaning it differentiates it from any other city bus).

But the license plate does not directly differentiate it from other types of vehicles like cars or bikes, as the semantics of each attribute are different for each type of vehicle, as mentioned before.

For example, although cars and buses both have a license plate, if we try to differentiate cars from buses using their Plate attributes, we will see that cars may have a different license plate structure than buses, as determined by the domain and project requirements.

So to distinguish and uniquely identify them, we need to use the VehicleID identifier, since Plate is specific to the vehicle types Car and CityBus.

In addition to representing the existence of buses, this entity has 1-* type associations with Person and City that model the person driving each bus and the city where it operates. So in the conceptual diagram, we can see that the association with Person has the role “drives”. This indicates that a person can drive an arbitrary number of buses, while a bus is driven by one and only one person.

This association results in CityBus having a foreign key pointing to Person, allowing us to know, given a bus, the person who drives it by accessing the Person tuple referenced by the foreign key attribute.

Similarly, CityBus also has an association with City that represents the city to which each bus belongs and operates. Conceptually, we can see this as each bus having to operate in only one city, and each city having an arbitrary number of buses operating in it, including none (since the minimum cardinality on the CityBus side is 0).

Logically, this is implemented with a foreign key in CityBus pointing to City. So if we need to know the city where a certain bus operates, we simply check the value of its foreign key, which will uniquely identify a tuple in City, indicating the city we are looking for.

CREATE TABLE CityBus (
    Plate VARCHAR(32) PRIMARY KEY,
    RouteNumber INT NOT NULL CHECK (RouteNumber >= 0),
    Seats INT NOT NULL CHECK (Seats > 0),
    FreeWifi BOOLEAN NOT NULL,
    VehicleID INT UNIQUE NOT NULL,
    DriverFK INT NOT NULL,
    CityFK INT NOT NULL,
    FOREIGN KEY (VehicleID) REFERENCES Vehicle(VehicleID),
    FOREIGN KEY (DriverFK) REFERENCES Person(PersonID),
    FOREIGN KEY (CityFK) REFERENCES City(CityID)
);
In addition to the previous foreign keys, there is another one in the BusTrip entity that references CityBus, which we can see with the 1-* type association it has with that entity. This last one isn’t directly reflected in the DDL of CityBus, but it’s in the relational diagram where we have an arrow pointing to the primary key Plate. And, as usual, we do not add the NOT NULL constraint to the Plate attribute, since we are imposing the PRIMARY KEY constraint, which implicitly includes NOT NULL in all the attributes that comprise it.

The foreign keys for Person and City also can’t be NULL due to the minimum cardinalities of the associations, where having 1..1 implies that a city bus must have a driver and a city to operate in, hence NOT NULL is explicitly added in the DDL.

CarRegistration entity
In our domain, cars can belong to a person through a record in the Carownership table. They can also be registered as fit to drive through CarRegistration, which associates cars with driver's licenses to model their legal registration. That is, a car can exist at any time, but to be able to drive, it must be registered and associated with a driver's license. This is why CarRegistration is dedicated to associating cars with driver's licenses.

The entity is very similar to some we have seen before, like Residence (while the entities it relates to here are different, as well as the reason for its existence). Implicitly, a car can be registered and associated with many driver's licenses, while the same driver's license can have an arbitrary number of cars associated with it. We can determine this by observing the cardinalities and navigability of the associations in the conceptual diagram.

For example, if we have a car, then by conducting an exhaustive search in the tuples of CarRegistration, we can find out how many records it’s in or has participated in. Also, for each of those records, we automatically know the driver's license it has been associated with – so from one car, we can learn about many driver's licenses.

Conversely, the same applies: if we have a certain license, we can indirectly find out by looking in the CarRegistration table how many records associate cars with that license. And for each of those records, we would obtain the associated car.

We’ve now analyzed navigability at the logical level. Previously, we saw the concept of navigability at the conceptual level, where associations could only be traversed in a certain direction depending on their cardinality. But in the logical model, we have access to all the tuples of all the tables in the database schema.

So, even though the Car-CarRegistration association is not navigable towards CarRegistration at the conceptual level, it is at the logical level. That is, if we have a car, we can find out which tuples in CarRegistration refer to that car, using the foreign keys that implement the association. With that information, we can then navigate to DrivingLicense once we know which tuples in CarRegistration pointed to the car.

This type of navigation is considered more typical of the logical level. With it, we can obtain information from other entities in a broader way than with the concept of navigation we saw at the conceptual level.

Here, on the entity-relationship diagram, we can see that there is an implicit N-M association between Car and DrivingLicense, which we just navigated through.

To do this, we had to go through the 1-* type associations, which are divided so that there can be an "intermediate" entity that stores information related to the N-M association, and to enable its implementation at the logical level. But we need to keep in mind the cardinalities of the 1-* associations that make up the implicit N-M association, where on the CarRegistration side we have optionality because the minimum cardinality is 0. This means that a car may not be registered, so there would be no tuple in CarRegistration referring to a certain car, thus preventing navigation to DrivingLicense.

This is completely valid because if a car is not registered, it won’t be associated with any driving license, and so we won’t be able to access any information from DrivingLicense.

Despite this, since there is a possibility that it’s registered, we consider these associations at the logical level to be navigable (all of this is equivalent to analyzing it in the opposite direction, from DrivingLicense to Car through CarRegistration).

For this process to be carried out, CarRegistration needs to have two foreign keys: one pointing to the primary key of Car to uniquely identify the car being registered, and another pointing to DrivingLicense to identify the driver's license associated with the car.

CREATE TABLE CarRegistration (
    RegistrationID SERIAL PRIMARY KEY,
    RegistrationDate DATE NOT NULL,
    ExpirationDate DATE NOT NULL CHECK (ExpirationDate > RegistrationDate),
    PlateFK VARCHAR(32) NOT NULL,
    LicenseFK INT NOT NULL,
    FOREIGN KEY (PlateFK) REFERENCES Car(Plate),
    FOREIGN KEY (LicenseFK) REFERENCES DrivingLicense(LicenseID)
);
When implementing the CarRegistration table, we see in its DDL that the primary key is declared as SERIAL because it’s a surrogate key. Also, the foreign keys all have the NOT NULL constraint due to the minimum multiplicity of 1 for the corresponding associations, which requires every tuple in CarRegistration to reference exactly one car and one driving license.

Regarding the information stored in the car registration, we mainly have the registration date or expiration date. Neither can be NULL, since we assume that in our domain, cars are always registered for a certain period that can later be extended through other registrations.

Here, we could have defined the CarRegistration entity as weak in identification, including both foreign keys in a primary key like {RegistrationDate, PlateFK, LicenseFK}. But for simplicity, a surrogate key is preferred, which simplifies database operations. In fact, the only advantage of not using the surrogate key would be saving the space occupied by the values of that additional column (and we could remove it if need be). But doing so would complicate the identification of CarRegistration tuples, as well as make certain queries less efficient and less readable.

And if we delve into the physical level, we would realize that having a primary key composed of more attributes would cause the DBMS to use more space to manage it. This would counteract the savings from removing the surrogate key – so the surrogate key remains the preferred option.

In summary, at the conceptual level, we’ve learned that navigation from Car to DrivingLicense is not entirely possible, as there is no foreign key in Car pointing to CarRegistration. But at the logical level, we can get information from CarRegistration because we can examine all the tuples of CarRegistration, allowing us to know which of them has their corresponding foreign key referencing the car we started from.

That is, conceptually, 1-* type associations are only navigable from the many side to the one side, but at the logical level, they are considered bidirectional. Also, the 1-* associations surrounding CarRegistration are both of the 1-* type and implicitly give rise to an N-M type, which is another reason why we can actually navigate from Car to DrivingLicense through CarRegistration.

DrivingLicenseRequest entity
In our domain, people can request a driver's license from a public entity, which in this case doesn't matter to us – we only care that it’s responsible for accepting or rejecting these requests. If a request is accepted, it should become the driver's license of the person who requested it, while if it is rejected, it will remain in the database as a failed request.

To model this in our database, we have many options:

creating a DrivingLicenseRequest entity with a boolean attribute Accepted to represent whether the request status is accepted or not, or

creating an IS-A hierarchy as seen in the conceptual diagram, where we have a superclass DrivingLicenseRequest dedicated to recording all requests that exist or have existed. In turn, we have inheriting entities that are created once the request has been resolved, with one entity representing accepted requests and the other modeling those that are rejected.

On one hand, using a single entity with attributes that determine its status is not the best option, because besides knowing if it has been rejected or not, the request can be in process. This would mean that it’s neither accepted nor rejected yet.

This causes multiple problems that complicate implementation, such as needing to make the Accepted attribute NULL until the request has been resolved, or even using this NULL value to represent the request's status. This "mixes" the semantics of the Accepted attribute with the representation of the request's status. This is not necessarily a serious problem, just a lack of clarity in representing the status and outcome of a request.

This option would also generate NULL values in the specific attributes of rejected or accepted requests, since each of them requires specific attributes that the other type does not have (such as the number of points for an accepted driver's license). So with this option, besides distinguishing the type of request, you would need to manage the NULL values in all attributes that don’t correspond with the type represented in the Accepted attribute. And this greatly complicates the semantics and operations when managing the data, as well as wasting unnecessary space storing these NULLs.

On the other hand, using an IS-A hierarchy to conceptually model driver's license requests can bring other disadvantages, such as greater complexity in the schema from the high number of tables that can be generated. You can also have more complexity when adding constraints to ensure that a request is not accepted and rejected at the same time. Or you can even have data fragmentation across multiple tables, where part of the information is stored in a superclass and the rest in an entity representing the specific type of request, whether accepted or rejected.

Still, using an IS-A hierarchy solves all the problems we saw with the previous option, providing a simpler and more consistent semantics with which we can operate on the database more easily. It also keeps us from having to worry about managing NULL values in certain attributes or the consistency between attributes that determine the request's status, as there are none of those here.

Thus, in the conceptual model, we have an IS-A hierarchy representing these requests, where a superclass represents requests that have just been created and are in process. In inheriting entities, these same requests are represented once they have been resolved. If they are rejected, they become a specific type RejectedDrivingLicense, and if accepted, another type called simply DrivingLicense.

In other words, at the conceptual level, we can view each request as an "individual" that can be found in the set of entity occurrences of the superclass, indicating that the request is in process. When it’s rejected or accepted, that individual then belongs to the set of the corresponding inheriting entity.

How can we model the driving license requests hierarchy at the logical level?
As we have seen before, an IS-A hierarchy doesn’t have a direct and unique translation at the logical level, as its semantics and domain requirements determine which possibilities are better or worse in aspects like query efficiency, ease of management, and so on.

So to translate this entity hierarchy formed by the superclass entity DrivingLicenseRequest and the inheriting entities RejectedDrivingLicense and DrivingLicense into tables in a DBMS, we need to analyze its characteristics to determine what implementation best suits the domain we’re modeling. We also need to analyze the other entities and the associations that connect with them, such as the association between Person and DrivingLicense, which models the relationship between a person and their driving license.

The first thing we need to check is whether the hierarchy is complete or not. In this case, there can be requests in process that haven’t been accepted or rejected, and so aren’t represented in any of the inheriting entities.

Since there are requests that don’t necessarily need to be represented by any inheriting entity, we see that the hierarchy is not complete (partial). Given that it’s not complete, the only way for our database to correctly store those requests that are in process is to create a specific table for the superclass DrivingLicenseRequest. Without it, it would be more complicated to know when a request has been resolved or not.

Later, knowing that all requests are stored in DrivingLicenseRequest, our system must be able to store information that determines whether it has been resolved or not, as well as the result of its resolution. For this, when a request is resolved and accepted, an occurrence of the entity DrivingLicense is created. But if it’s rejected, an occurrence of the other inheriting entity is created.

So in no case will the request be represented by occurrences of multiple inheriting entities at the same time, so the hierarchy is disjoint. This ensures that the previous decision to implement a table for the superclass is the correct option.

To translate the inheriting entities to the logical level, we need to decide whether to implement a table for each one, a single table for both, or even a table with all the entities in the hierarchy.

The most important thing to consider in this decision is the number of attributes each entity has and the tuples expected to exist in the database if we implement a table for each entity. In other words, we need to consider how many occurrences of each entity are expected to exist in the domain.

Initially, we can assume that there are always more rejected licenses than accepted ones, as it’s very likely to be rejected at least once before being accepted. With this, we could decide to implement a table for the DrivingLicenseRequest and RejectedDrivingLicense entities together (since there are more rejected than accepted) and another for DrivingLicense that has a foreign key pointing to that table. But this would generate NULL values in the attributes from RejectedDrivingLicense when representing accepted driving licenses.

Since implementing the entire hierarchy with a single table also leads to too many NULL values in the attributes when representing accepted or rejected licenses, the best solution in this case is again to implement a table for each entity in the hierarchy.

The main reason for choosing this option is the number of NULL values generated when representing accepted or rejected licenses. In general, if the inheriting entities had only one attribute, then it would be clear that it could be implemented more simply with a single table for the entire hierarchy, or two.

But when there is more than one attribute, the queries become especially complicated because we need to check if several attributes are NULL at the same time (and manage the database and the possible extension of the hierarchy to more types of requests).

CREATE TABLE DrivingLicenseRequest (
    LicenseID SERIAL PRIMARY KEY,
    RequestDate DATE NOT NULL,
    Fee DOUBLE PRECISION NOT NULL CHECK (Fee >= 0),
    PersonFK INT NOT NULL,
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID)
);
Once we decide to use a table for each entity in the hierarchy, we need to reflect this decision in both the relational diagram and the SQL DDL. This is mainly because it’s not complete, disjoint, and has multiple attributes in the inheriting entities that would lead to too many NULL values.

So in the relational diagram, we create the corresponding tables, where DrivingLicense and RejectedDrivingLicense add foreign keys pointing to DrivingLicenseRequest to identify the request that has been rejected or accepted.

In other words, all requests are stored in the superclass table. Then when they are accepted or rejected, a tuple is added to the corresponding table so that its foreign key references the DrivingLicenseRequest tuple representing the request itself. This way, the superclass table is dedicated to storing requests, while the other tables focus on representing which requests have been rejected or accepted.

Regarding the foreign keys pointing to or present in any of the tables in the hierarchy, we can see that to know which person a certain request belongs to, there is a foreign key in DrivingLicenseRequest pointing to Person. So for every request, that foreign key indicates the person associated with that request.

On the other hand, given the associations we see in the conceptual diagram, there are two other foreign keys from other entities pointing to DrivingLicense. We need to consider all of this because it can affect the decision we made earlier about how to implement the hierarchy. If there are foreign keys pointing to the superclass, for example, we would necessarily have to implement a table for it.

Finally, we identify the requests using a surrogate key in DrivingLicenseRequest, which uniquely identifies all requests, regardless of their status. We can also see this in the inheriting entities, which don’t have any type of identification on their own, but are assumed to be identified by the primary key of DrivingLicenseRequest.

In other words, even though there is no clear identifier in the inheriting entities, it’s important to remember that the attributes of the superclass are inherited. So when we’re implementing the hierarchy at the logical level, no matter how we do it, we will always do it in such a way that each accepted or rejected request can be identified by the primary key of DrivingLicenseRequest. This is the table that stores the resolved request.

RejectedDrivingLicense entity
Continuing with the previous hierarchy, given its implementation, we have the table RejectedDrivingLicense, which represents its corresponding entity. Its tuples will store information regarding the rejection of requests that have been denied, such as the date or reason for rejection.

Also, to know which request each tuple's information corresponds to, there is a foreign key pointing to DrivingLicenseRequest, specifically referencing the tuple in that table that stores the rest of the request information (including the primary key that identifies it).

To avoid having to include a surrogate key in this table or define a primary key from the entity's attributes, we’ll choose the primary key to be the foreign key itself. This in turn references the primary key of the superclass table that uniquely identifies all requests, regardless of their status.

This means that the RejectedDrivingLicense table is weak in identification, as it requires the primary key of the owning entity DrivingLicenseRequest to identify it. But as we’ve seen before, this shouldn’t be reflected in the conceptual diagram because this way of implementing the hierarchy is not always unique. So depending on how we do it, the table may cease to be weak in identification.

In summary, the variability that exists when implementing an IS-A hierarchy with tables means that concepts like identification weakness aren’t indicated in the conceptual diagram, as they are only generated by certain implementations.

Generally, if we only look at the diagram, all we can do is consider all the options available for implementing the hierarchy, analyze each one, and even decide which one to implement in the end. But this is a decision we make and doesn’t imply what we have represented in the hierarchy of the diagram.

In other words, from the diagram, it’s impossible to infer which specific logical implementation has been used, although in very specific cases, it can be easier and more straightforward to "guess."

Entity-relationship diagram with incomplete and disjoint inheritance where AcceptedRequest is a subclass of Request. Image by author. 

For example, say we have a hierarchy with only one superclass Request that is pointed to by a foreign key and an inheriting entity AcceptedRequest that’s very similar to the one in our domain. We can see at the conceptual level that the hierarchy is incomplete, as there may be requests that have not yet been accepted. It’s also disjoint, which in this case makes analyzing this aspect of the hierarchy irrelevant given the number of inheriting entities it has.

So, since it’s incomplete, we’ll need a table for the superclass. Also, to avoid the appearance of NULL values if the hierarchy is implemented with a single table, we’ll use a table for the inheriting entity.

But if the hierarchy were complete, we would only have one clear way to implement the hierarchy: with a single table. This is because there would never be NULLs in the attributes of AcceptedRequest, despite the option of using a table for each entity and complicating the database logic by inserting a tuple in each table for each request.

With this example, we see that in very specific cases, it’s possible to infer the clearest way to implement a hierarchy at the logical level, even though there will always be some variability that prevents us from "guessing" the exact implementation chosen for the DBMS. Finally, it’s also important to consider the identification of the tuples with which we model the hierarchy, where multiple options also arise.

On one hand, if the domain or requirements dictate that certain entities need to have their own identifiers, then we’ll have to define them as primary keys of the corresponding entities. In our domain, all requests must be uniquely identified by an attribute in DrivingLicenseRequest, so we add a surrogate key to that entity.

If the requirements tell us that some of the attributes of an entity serve as an identifier, then we’ll use them as the primary key – but here for simplicity, we assume there is no domain-specific identifier, and we are the ones adding the surrogate key as an identifier to store the data in our system.

On the other hand, if we don't have information on how the entities should be identified, then we have the freedom to do so however we want, mainly depending on the implementation chosen in the end.

But regardless of the source of this identification, in general, it all comes down to whether or not each inheriting entity can be identified by its own attributes. This determines if the table it converts to at the logical level is weak in identification or not – because if we ultimately decide to define a primary key for an inheriting entity, then we will necessarily implement it with a concrete table.

As you can see, the identification of each entity can give us clues about how the hierarchy will be implemented, but it’s not something unequivocal that always guarantees a single way to implement it.

Sometimes, it’s our task to define how we identify them, and that will depend on how many tables we choose for the implementation and how we associate them with each other.

CREATE TABLE RejectedDrivingLicense (
    LicenseID SERIAL PRIMARY KEY,
    RejectionDate DATE NOT NULL,
    ReapplicationDate DATE NOT NULL CHECK (ReapplicationDate >= RejectionDate),
    Reason VARCHAR(32) NOT NULL,
    FOREIGN KEY (LicenseID) REFERENCES DrivingLicenseRequest(LicenseID)
);
In the DDL corresponding to this entity, we see that a specific table is created for it with the respective attributes shown in the conceptual diagram. Also, we include one that serves as a foreign key to reference the tuple of DrivingLicenseRequest that represents the rejected application (and also serves as the primary key of this table).

We could include a surrogate key here as well, but since we already have the LicenseID value from the superclass table, we don’t need to do so (and the domain doesn’t require us to identify rejected applications in a special way).

So we add the PRIMARY KEY and FOREIGN KEY constraints to that attribute at the same time, so it can’t contain NULL values because of the implicit NOT NULL restriction added by PRIMARY KEY. It must also reference the primary key LicenseID attribute of DrivingLicenseRequest.

To reflect this in the relational diagram, we can just underline the foreign key attribute, indicating that the table is weak in identification and that attribute can’t take NULL values. But to clearly indicate that other attributes that aren’t part of the key (like RejectionDate) can’t be NULL, we need to use other elements like margin notes or any other technique that clearly reflects this condition.

CREATE ASSERTION RejectionDateConstraint CHECK (
    NOT EXISTS (
        SELECT *
        FROM RejectedDrivingLicense R
            JOIN DrivingLicenseRequest D USING (LicenseID)
        WHERE R.RejectionDate < D.RequestDate
    )
);
CREATE ASSERTION ApprovalDateConstraint CHECK (
    NOT EXISTS (
        SELECT *
        FROM DrivingLicense D
            JOIN DrivingLicenseRequest R USING (LicenseID)
        WHERE D.ApprovalDate < R.RequestDate
    )
);
Finally, although we define constraints on the table – such as that the reapplication date can’t be earlier than the rejection date – there are also other constraints (like the rejection date must be after the application date).

These types of constraints involving information from multiple tables need to be implemented with assertions or triggers. The simplest option is to use assertions as shown above, although we haven’t yet implemented the ASSERTION statements in PostgreSQL, so attempting to define them will result in an error from the DBMS. It’ll simply ignore these definitions.

So we’ll choose not to implement these types of constraints at this point, assuming that the inserted data already meets them for simplicity.

CREATE ASSERTION NoSimultaneousApprovalRejection CHECK (
    NOT EXISTS (
        SELECT *
        FROM DrivingLicense d
            JOIN RejectedDrivingLicense r USING (LicenseID)
    )
);
Also, accepted and rejected applications can’t exist at the same time, so with this assertion, we could prevent this inconsistency. Basically, we define here that there can’t be any tuple in either DrivingLicense or RejectedDrivingLicense with the same LicenseID. This means that no application (LicenseID) can appear simultaneously in both tables, as the domain requires people to submit a new application when the one they have submitted is rejected.

DrivingLicense entity
To conclude with this hierarchy, when a driver's license application is accepted, a tuple gets created in the DrivingLicense table with which we have implemented its respective entity. Thus, the main goal of this entity is to model a person's driver's license, because once it’s accepted, it can be used to register cars, and is indirectly associated with the person who holds the license.

To do this, first, the DrivingLicense has a foreign key pointing to DrivingLicenseRequest, just like the previous inherited entity in the hierarchy. In turn, the request, regardless of its status, always refers to a person through its foreign key, to model whose license it is. And for cars to be registered in association with a person's driver's license, the CarRegistration entity has a foreign key pointing to DrivingLicense, so each registration necessarily refers to a specific license.

We can see all of this in the entity-relationship diagram through the 1-* type associations, as well as in their minimum cardinalities. But we can’t directly infer the existence of the foreign key in DrivingLicense, specific to the hierarchy implementation, because we can implement the hierarchy in many ways.

To understand this last point, imagine being told that there is a foreign key in DrivingLicense pointing to another entity. With this information, we can directly know that this foreign key points to the superclass of the hierarchy and exists because of the implementation where there is at least a specific table for DrivingLicense.

This is because the rest of the associations of the DrivingLicense entity are of the 1-* type, with the 1 on the DrivingLicense side – so these associations result in foreign keys pointing to DrivingLicense, not the other way around. In summary, with just the conceptual diagram, you can’t know exactly how a hierarchy has been implemented, but with some additional information, you can.

CREATE TABLE DrivingLicense (
    LicenseID SERIAL PRIMARY KEY,
    ApprovalDate DATE NOT NULL CHECK (ApprovalDate <= CURRENT_DATE),
    Points INT NOT NULL CHECK (
        Points BETWEEN 0 AND 15
    ),
    FOREIGN KEY (LicenseID) REFERENCES DrivingLicenseRequest(LicenseID)
);
The DDL of this entity is very similar to the previous one, where we have a primary key composed of the LicenseID attribute, which is also a foreign key that identifies the request that has been accepted. This refers to the tuple in the superclass table where the request is stored and uniquely identified, along with the entity's own attributes.

The table constraints in this case declare that the approval date must be earlier than the current date, as it’s impossible for a request to have been accepted on a date that has not yet occurred.

For this, we use CURRENT_DATE to get the current date in SQL and compare it with another date like the one stored in the attribute. There’s also the Points attribute, which determines the remaining points on the person's driver's license. According to the domain, this value is an integer between 0 and 15, so we restrict the possible values it can take with a CHECK, as well as with the INTEGER data type itself, preventing it from taking decimal values.

Given the simplicity of the attribute's domain, using a CHECK is the easiest option, although we could have defined a DOMAIN or TYPE ENUM and assigned it as the data type to the attribute. This would be useful if we had more attributes with the same domain in the rest of the schema.

BusTrip entity
People in our domain can use CityBus buses as a means of transportation. To this end, we have an entity called BusTrip that models specific routes buses take across the city. Each time a bus travels from one point to another, it’s considered a trip recorded in this entity through a tuple. This tuple stores information such as the starting and ending addresses of the trip, the date it takes place, and the time it took.

To uniquely identify the tuples in this table, the primary key uses the attributes TripDate, the starting and ending addresses, and the foreign key attribute that identifies the specific bus that made the trip. We have to include the foreign key in the primary key because there could be several BusTrips with the same date and starting and ending addresses, all conducted by different buses.

So to uniquely distinguish all of them, we need to include the information of the bus making the trip, which means the value of the foreign key pointing to CityBus.

Regarding the semantics of this entity, we can see that no bus can make the same trip multiple times on the same date, as this would result in duplicate tuples, violating the primary key constraint. We assume that this is the case because of the characteristics of the domain.

In the design process, sometimes we have to model situations that may not be entirely intuitive, such as a bus not making the same trip more than once a day.

Since the TripDate attribute is of type DATE, it can only store dates with a resolution up to days. This means that we can’t represent the exact moment the trip occurs in our database (in the same way we could using the TIMESTAMP data type, which allows representing moments in time with date and time).

So, given the granularity of the DATE data type, we comply with the restriction that a bus can’t make the same trip multiple times a day (beause in that case, several tuples with exactly the same date would be stored, since DATE can only represent up to days).

This is an example of a restriction that is implicitly modeled by the data type of the attribute itself. If it were TIMESTAMP, we could have multiple trips by the same bus on the same day but at different times.

CREATE TABLE BusTrip (
    TripDate DATE NOT NULL,
    StartAddress VARCHAR(32) NOT NULL,
    EndAddress VARCHAR(32) NOT NULL,
    Duration INT NOT NULL CHECK (Duration >= 0),
    PlateFK VARCHAR(32) NOT NULL,
    PRIMARY KEY (TripDate, StartAddress, EndAddress, PlateFK),
    FOREIGN KEY (PlateFK) REFERENCES CityBus(Plate)
);
When constructing the relational diagram, we must also underline the foreign key attribute that points to CityBus, since it’s part of the primary key (it’s the weak entity in identification). More specifically, we can infer this from the entity-relationship diagram by looking at where the «weak» role is located, which indicates the owner entity of BusTrip, meaning the one it depends on for identification.

In the DDL, this is reflected in the attributes that make up the primary key, where we find the three from the table itself and PlateFK, which is the foreign key responsible for referencing the bus that makes the trip. We won’t impose any additional restrictions on the StartAddress and EndAddress attributes, even though just any text can’t be stored in them – only texts that represent valid addresses in a city (specifically where the bus operates).

For simplicity, we’ll assume that if an address is not valid, it’s the responsibility of another part of the system to check this, such as software in the application layer that validates addresses before inserting tuples into the database.

On the other hand, we will add the non-negativity restriction on the duration, as it doesn't make sense for it to be negative. We could name these restrictions to make database administration easier, but since we aren’t going to work on them here, we won’t do so.

BusTicket entity
For a person to travel on a bus route, they must have a ticket that allows them to board a bus represented in the CityBus table. So in our domain, we’ll model the existence of tickets with the BusTicket entity. Its only attribute is used to store the timestamp when it was issued.

It’s important to use the TIMESTAMP data type here and not DATE because a person can buy multiple tickets on the same day for different routes, which is why we need to clarify which ticket was generated first.

When we see how this is represented in the conceptual diagram, you might notice the XOR restriction that appears between the associations connecting BusTicket with Person and BusPass. This restriction represents that all existing tickets are either directly associated with a person who owns the ticket or are associated with a BusPass that’s owned by a person and allows multiple trips with a pass.

This is how we’d semantically explain the restriction we want to model – but conceptually, when we have a restriction represented by a dashed line and a logical condition like XOR, it means that either the BusTicket-Person association exists, or the BusTicket-BusPass association exists. It’s not possible for neither to exist or for both to exist at the same time.

Because these associations exist, the foreign key in BusTicket pointing to the respective entity is not NULL. That is, both associations are of type 1-*, so they are clearly implemented with foreign keys in BusTicket. But the minimum cardinalities on both sides are 0, indicating that the associations as a whole may not exist. In other words, it means that the values of the foreign key attributes can be NULL.

At this point, if we didn't have the XOR restriction, the foreign keys could both be NULL at the same time, indicating that a ticket is not associated with any person or pass, making it impossible to identify the passenger taking the trip.

On the other hand, if both foreign keys had values in their attributes, we would be modeling that the person has used the pass to travel by bus through the non-nullity of the BusTicket->BusPass foreign key, while at the same time modeling that the same person has not used the pass but has obtained a ticket directly through the non-nullity of BusTicket→Person.

That is, if the foreign key BusTicket->BusPass is not NULL, then we are modeling the situation where a person uses their pass to travel by bus, while the other foreign key, when not NULL, represents that the person is not using a pass to travel but is doing so directly with a ticket.

So both situations can’t occur at the same time thanks to the domain restrictions that dictate that a person either travels with a ticket or with a pass – but not both at once, and not neither. This is because a ticket is necessary to travel. This is why we use the XOR condition to represent that either one association exists or the other, but not both at the same time. It also prohibits neither from existing.

PersonFK	PassFK	Valid	Meaning
No NULL	NULL	✔️	Ticket purchased directly by the person.
NULL	No NULL	✔️	Ticket charged to the person's BusPass.
NULL	NULL	❌	There is no information on which person is traveling (orphan ticket).
No NULL	No NULL	❌	Indicates both direct purchase and use of a pass at the same time (inconsistent).
It’s also important to emphasize that for the foreign keys to be NULL, the minimum cardinality on the side of Person and BusPass in the associations must be 0.

To model that a person travels using a pass, we might consider associating the BusPass entity directly with BusTrip instead of with BusTicket. But doing this would result in an N-M relationship between BusPass and BusTrip, since a pass can lead to an indefinite number of trips, while multiple people can travel using their pass on a single trip.

To avoid having to add another intermediate entity to implement the N-M association, let’s associate BusTicket with BusPass, so that we can see each trip made using a pass by checking the foreign key values of the ticket.

CREATE TABLE BusTicket (
    IssueTime TIMESTAMP,
    TripDateFK DATE,
    StartAddressFK VARCHAR(32),
    EndAddressFK VARCHAR(32),
    PlateFK VARCHAR(32),
    PersonFK INT,
    PassFK INT,
    PRIMARY KEY(
        IssueTime,
        TripDateFK,
        StartAddressFK,
        EndAddressFK,
        PlateFK
    ),
    FOREIGN KEY (
        TripDateFK,
        StartAddressFK,
        EndAddressFK,
        PlateFK
    ) REFERENCES BusTrip(TripDate, StartAddress, EndAddress, PlateFK),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID),
    FOREIGN KEY (PassFK) REFERENCES BusPass(PassID),
    CONSTRAINT XORConstraint CHECK (
        (
            PersonFK IS NULL
            AND PassFK IS NOT NULL
        )
        OR (
            PersonFK IS NOT NULL
            AND PassFK IS NULL
        )
    )
);
To uniquely identify each ticket, we have to use both the IssueTime attribute of the table and the foreign key pointing to BusTrip, which determines which trip will be made with that ticket. So we have a weak entity in identification again – and it’s peculiar in that the foreign key in this case is composed of several attributes, since the primary key of BusTrip (which is the owning entity on which it depends for identification) is itself composed of multiple attributes. Specifically, this primary key has 4 attributes – so in BusTicket, as the foreign key must reference the primary key of BusTrip, it will be composed of exactly 4 attributes (meaning as many as the primary key it points to).

To declare this foreign key, we use the same FOREIGN KEY constraint as always, the only difference being that here we use several attributes instead of just one.

The most important thing about this constraint when we have multiple attributes is to declare them in order. For example, if we want TripDateFK to point to the TripDate attribute of the BusTrip primary key, then we must put those two attributes in the same order in the constraint tuple. Here, for example, they are in the first position, but we could place them both in the second position after StartAddressFK and StartAddress, or in the third (and so on), as long as they correspond.

Since this is the only foreign key that can’t be NULL in the table, we need to ensure that all its attributes have the NOT NULL constraint. But since they’re part of the primary key, we don’t need to explicitly declare the constraint.

On the other hand, for the other foreign keys that model associations with Person and BusPass, we shouldn’t add this constraint because these foreign keys will need to take a NULL value in certain situations. So none of the attributes require us to declare constraints.

Finally, the TIMESTAMP data type isn’t the only one that can store date and time in the IssueTime attribute – we also have alternatives like DATETIME or TIMESTAMP WITH TIME ZONE. These have specific uses, such as storing the time zone in addition to the time itself. For simplicity, in this example, we’ll use TIMESTAMP for all attributes that need to store date and time.

BusPass entity
We can already infer the semantics of this entity from what we’ve just seen. Specifically, if a person plans to take multiple bus trips and doesn’t want to buy individual tickets for each of those trips, they can purchase a pass (represented by the BusPass entity) which allows them to take multiple trips without worrying about tickets.

If we look at the conceptual diagram, we’ll see that it has several associations of type 1-*, where one of them is affected by the XOR constraint. So given the minimum cardinalities of this association, it may not exist, as we explained earlier. So we know that there won't always be a foreign key pointing to BusPass, since the association is optional due to its minimum cardinalities.

But on the other hand, we have the other association with Person that results in a foreign key in BusPass that always has to exist, because all passes must have an associated person (meaning every pass must have an owner).

CREATE TYPE ModalityType AS ENUM( 'single', 'round_trip', 'daily', 'weekly', 'monthly', 'annual' ); 
CREATE TABLE BusPass (
    PassID SERIAL PRIMARY KEY,
    IssueDate DATE NOT NULL,
    ExpirationDate DATE NOT NULL CHECK (ExpirationDate > IssueDate),
    Modality ModalityType NOT NULL,
    RemainingTrips INT NOT NULL CHECK (RemainingTrips >= 0),
    PersonFK INT NOT NULL,
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID)
);
For the identification of this entity, we include a surrogate key to simplify the process. We could have selected another set of attributes as the primary key, although this would result in the entity being weak in identification, as well as a primary key with more attributes than it has using a surrogate key. So the simpler solution is generally preferred.

We should include a CHECK to ensure that the expiration date of the pass is after the issue date to prevent inserting tuples with inconsistent dates. Also, none of the attributes can be NULL. The foreign key also can’t be NULL because of the minimum cardinality that prevents it from being NULL. And finally, other attributes like modality can’t be NULL either. For these, we implement a custom ENUM TYPE where we define the different pass modalities that determine how a person can use that pass.

Lastly, we can indicate the constraint that we modeled at the conceptual level with an XOR in the same way in the relational diagram using a dashed line between the foreign keys involved. We can also indicate it with a textual note. But in the DDL, the simplest way to code it is with a CHECK in BusTicket, which is where the foreign keys involved in the integrity condition originate.

Voyage entity
Continuing with the ways people in our domain travel by cruise, we have the entity Voyage. This models the trips taken by the cruises. Specifically, the entity stores information about the trip, such as the departure and arrival dates, as well as the ports where the trip begins and ends.

We can also see that it has an attribute called Distance, which might initially seem irrelevant – but Distance records the total distance traveled by the cruise during the trip. And this doesn’t necessarily have to match the shortest distance between the departure and arrival ports.

The decision to use this meaning for this attribute came from our domain and its constraints. That is, if we are required to record the total distance the cruise travels, in addition to the distance between both ports, the simplest option would be to add an attribute in this entity that records that magnitude.

In other words, if we didn't need to know the distance traveled by the cruise itself, we could be satisfied with knowing the distance between the departure and arrival ports (which we can determine from the port information). But we’ll use the attribute Distance here, which records the actual distance traveled by the cruise during the trip (since we need this information).

If we look at the conceptual model, we’ll see that this entity has two identical associations of the same type 1-* with the entity Port, all with the aim of conceptually modeling that a voyage is associated with two ports, one for departure and one for arrival, where both can be the same. Regarding this last point, if they could not be the same, we would need to indicate that restriction with a note, as there are no standard elements in an entity-relationship diagram or in the relational model to represent such a situation.

On the other hand, we could also consider modeling the trip so that is has departure and arrival ports through a single Voyage-Port association with a cardinality of 2 on the Port side. But if we did this, conceptually, we wouldn't distinguish which port was for departure and which was for arrival. Rather, we would be modeling that the cruise passes through two ports on that trip – but we wouldn't know for sure which was the arrival or departure port (at least at the conceptual level) since at the logical level there would necessarily have to be two foreign keys pointing to Port.

So to easily distinguish between the arrival and departure ports for a trip and to clarify the semantics of the association between Voyage and Port, we’ll use multiple associations, each with a role that explicitly indicates the relationship the port has with the trip.

In addition to these associations, the Voyage entity needs to reference CruiseShip to know which cruise has made that trip. That's why, in the conceptual diagram, there is a 1-* association with CruiseShip, where one cruise ship can make many trips, but a trip is only made by one cruise ship.

To identify this entity, we’ll take advantage of the fact that both the start and end dates of the trip are always defined to include them in the primary key. This means both dates can’t be NULL as they define the trip's duration.

But, to truly uniquely identify the Voyage tuples, we have to distinguish them using the departure and arrival ports of the trip, as well as the cruise ship that performs it. That's why we include all foreign keys in the primary key. If we didn’t do this, there could be several tuples of different trips made by different cruise ships or passing through different ports that could still have the same value in the departure and arrival dates. So we need to include information about the cruise ship making the trip, as well as the ports involved.

By defining the primary key this way, we are making the entity weak in identification, where its owning entities are CruiseShip and Port, even though part of its primary key is composed of attributes from the entity itself.

CREATE TABLE Voyage (
    DepartureDate DATE,
    ArrivalDate DATE CHECK (ArrivalDate >= DepartureDate),
    Distance DOUBLE PRECISION NOT NULL CHECK (Distance >= 0),
    DepartureNameFK VARCHAR(32) NOT NULL,
    DepartureCityFK INT NOT NULL,
    ArrivalNameFK VARCHAR(32) NOT NULL,
    ArrivalCityFK INT NOT NULL,
    ShipFK INT NOT NULL,
    PRIMARY KEY (
        DepartureDate,
        ArrivalDate,
        DepartureNameFK,
        DepartureCityFK,
        ArrivalNameFK,
        ArrivalCityFK,
        ShipFK
    ),
    FOREIGN KEY (ShipFK) REFERENCES CruiseShip(ShipID),
    FOREIGN KEY (DepartureNameFK, DepartureCityFK) REFERENCES Port(Name, CityFK),
    FOREIGN KEY (ArrivalNameFK, ArrivalCityFK) REFERENCES Port(Name, CityFK)
);
To implement this entity at the logical level, in its DDL, you can see that we first define the attributes of the entity itself, as well as those of the foreign key for the departure port, which are (DepartureNameFK, DepartureCityFK).

Note that the foreign keys pointing to Port must have two attributes since the primary key of Port has two attributes. So we’ll need a total of 4 attributes to model the foreign keys that reference the departure and arrival ports of the trip, both referencing the Name and CityFK attributes of the Port table (which make up its primary key as we saw earlier). Also, we need another attribute, ShipFK, to reference CruiseShip and thus determine which cruise ship made the trip.

With all this, the primary key of Voyage is defined as the set of attributes (DepartureDate, ArrivalDate, DepartureNameFK, DepartureCityFK, ArrivalNameFK, ArrivalCityFK, ShipFK).

If we had to infer the attributes that make up the primary key using only the conceptual diagram, we would need to look at the entities that the foreign keys reference. These are represented with the 1-* associations.

For example, in CruiseShip, we would see that its primary key has only one attribute, so necessarily in Voyage, the corresponding primary key that references it must have one attribute, ShipFK. Meanwhile, the other two foreign keys that reference Port need to have two attributes each, since we can see that Port is identified by its name and the city where it’s located. So its primary key has two attributes (Name, CityFK) that we will need to reference from Voyage.

In the relational diagram, this is easier to interpret. We’ll see that one attribute references an attribute of the CruiseShip table, so we know it’s a foreign key that leads to a 1-* association in the conceptual model.

Also, there are two other attributes that together reference two attributes of Port – and together, they also form a foreign key that creates a 1-* association in the conceptual diagram, where the many side is in the entity from which the foreign key originates ( that is, in Voyage).

With this last foreign key, we can represent the departure port of the trip. There’s another pair of attributes (ArrivalNameFK, ArrivalCityFK) that follow the same pattern to represent the arrival port of the trip. From them, we can also infer that at the conceptual level, there’s another association with the same characteristics.

And, since all these foreign keys are underlined, this implies they are part of the primary key of Voyage. From that we can infer that Voyage is weak in identification.

Lastly, if we look at the data types of the foreign keys, we’ll see that they match exactly with the types of the attributes they reference. This is especially important because a foreign key, by definition, is an attribute that holds the value of another attribute it references, so both must be of the same type for this to be possible.

Since foreign keys are made up of multiple attributes, in this case, we also need to consider the relative order of the attributes that form the foreign key with the order of the attributes they reference. This is unlike what happens in the PRIMARY KEY constraint, where the order in which the primary key attributes are declared doesn’t matter. In that case, in PRIMARY KEY, we are declaring a set of attributes, where what matters is that they appear in the constraint (not that they follow a specific order).

CruiseBooking entity
For a person to travel on a cruise, they must make a reservation for a specific voyage. So in our domain, we have the entity CruiseBooking, which is responsible for storing the reservations people make to travel on a cruise.

The data stored for each reservation includes the booking date, cabin number, price, and payment method. To know which person has booked which voyage, the entity has 1-* associations with Person and Voyage, which logically translate into two foreign keys pointing to the respective entities.

To uniquely identify each booking, we could choose the easy option of including a surrogate key attribute to serve as the primary key. But to illustrate the complexity of not doing this, we’ll use only attributes from the table itself to identify its tuples. So the primary key of this entity is composed of the attributes BookingDate, CabinNumber, the foreign key to Person, and the other foreign key to Voyage.

We do this because we assume that multiple people can book the same cabin for the same voyage, all on the same date. For example, this can happen if several people from the same family decide to book a certain voyage. Each of those people will have a record or tuple in the CruiseBooking table with the same attributes in BookingDate, CabinNumber, and the foreign key of Voyage, but a different value in the foreign key of Person. This allows the tuples to be uniquely distinguished.

The foreign key to Person has a single attribute since the primary key of Person has only one attribute. But the other foreign key that refers to the voyage being booked has exactly 7 attributes (as the Voyage entity requires 7 attributes to be uniquely identified).

With this, we realize that the primary key of CruiseBooking will have a total of 10 attributes, making it a much more complex solution than simply using a surrogate key. So you can see why it’s very convenient to use surrogate keys whenever possible for this type of entity – especially when the foreign keys that will be part of the primary key have too many attributes, as in this case.

CREATE TYPE PaymentMethodType AS ENUM ('card', 'paypal', 'bank', 'cash', 'mobile');
CREATE TABLE CruiseBooking (
    BookingDate DATE NOT NULL,
    CabinNumber INT NOT NULL CHECK (CabinNumber > 0),
    Price DOUBLE PRECISION NOT NULL CHECK (Price >= 0),
    PaymentMethod PaymentMethodType NOT NULL,
    PersonFK INT NOT NULL,
    DepartureDateFK DATE NOT NULL,
    ArrivalDateFK DATE NOT NULL,
    DepartureNameFK VARCHAR(32) NOT NULL,
    DepartureCityFK INT NOT NULL,
    ArrivalNameFK VARCHAR(32) NOT NULL,
    ArrivalCityFK INT NOT NULL,
    ShipFK INT NOT NULL,
    PRIMARY KEY (
        BookingDate,
        CabinNumber,
        PersonFK,
        DepartureDateFK,
        ArrivalDateFK,
        DepartureNameFK,
        DepartureCityFK,
        ArrivalNameFK,
        ArrivalCityFK,
        ShipFK
    ),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID),
    FOREIGN KEY (
        DepartureDateFK,
        ArrivalDateFK,
        DepartureNameFK,
        DepartureCityFK,
        ArrivalNameFK,
        ArrivalCityFK,
        ShipFK
    ) REFERENCES Voyage(
        DepartureDate,
        ArrivalDate,
        DepartureNameFK,
        DepartureCityFK,
        ArrivalNameFK,
        ArrivalCityFK,
        ShipFK
    )
);
If we look at the DDL, it seems much more complex than the previous ones. But actually, the elements we used are the same. We define the primary key with PRIMARY KEY, and the attributes of the foreign keys with the same data types as the attributes they reference. We also use the NOT NULL constraint to correctly implement what's indicated in the minimum cardinalities of the associations.

We declare each foreign key with FOREIGN KEY, which is longer in this case due to the number of attributes that make up each one. The only important thing to keep in mind here is that one of the FOREIGN KEYs is exclusively dedicated to declaring the foreign key to Person (meaning the association between CruiseBooking and Person) while the other models the association with Voyage.

We do this without mixing attributes of both foreign keys in the same FOREIGN KEY – as this would be an error since we wouldn't be modeling the conceptual diagram correctly. Each foreign key is independent of the others, so each FOREIGN KEY includes only the attributes that make up each corresponding foreign key.

To simplify the domain of the PaymentMethod attribute, we can define a TYPE ENUM, since the payment method is an attribute that will likely be used in other parts of the domain. Even if it's not needed now, it's possible that in a future expansion of the domain, we might need to include it in the schema. This is why it's important to declare it to make database management easier in a potential expansion.

Pool entity
In our domain, there are also pools, which are represented in the IS-A hierarchy with the entity Pool as the superclass. This allows people to interact with pools in different ways, as we will see below. So we consider that our domain includes different types of pools, such as cruise pools found on cruise ships modeled with CruiseShip, city pools found in cities, or Olympic pools also found in cities.

Since they all share common attributes, we do the same as in the Vehicle hierarchy, using a superclass that includes these common attributes like the pool's name, its address, minimum and maximum depths, or the current state of the pool.

We also include a 1-* association between Pool and City to represent that all pools are located in a city – except for those of type CruiseShip, which are on a cruise ship and not in a city. In that specific case, the semantics of the association are different, as we’ll see later. From this, we can define different types of pools with distinct characteristics, where all of them inherit all the attributes of their superclass, including the association with City.

As we can see, CityPool and OlympicPool have no issue with this, but CruisePool models pools on cruise ships, so its association with City does not have the same semantics as the others. In other words, the pool is not located in a city but on a cruise ship – so we assume that the associated city is its place of manufacture.

As you can guess, this is not the only way to model this domain, nor is it the best, since the "locatedAt" semantics indicated in the conceptual diagram's association between City and Pool does not capture the meaning of that relationship when the pool is of type CruisePool. But once we clarify this, the model is correct in the sense that all essential elements are represented correctly, even if not in the best possible way.

How can we translate the pool hierarchy entities at the conceptual level to tables?
Once we’ve clarified the semantics of the hierarchy, we can follow the same process as before to implement it at the logical level.

Part of the entity-relationship diagram where the IS-A hierarchy of pools is represented according to their type. Image by author.

First, we note that the hierarchy is not complete, as we assume that in our domain there are many types of pools, of which we only model 3 with specific entities, while the rest are pools modeled with occurrences of the Pool entity.

In other words, if a pool is one of the types of the inheriting entities, it will be represented as an occurrence of that entity, while if it’s of a different type, it will be represented by an occurrence of the superclass. So, in the hierarchy, pools aren’t required to belong to the inheriting entities, making it incomplete.

On the other hand, the types of pools are all disjoint, meaning a pool can’t be both Olympic and cruise at the same time, or city and Olympic at the same time. So the hierarchy is disjoint because there won’t be any pool that is of multiple types at once.

Just like in the DrivingLicenseRequest hierarchy, pools here are also uniquely identified with a surrogate key in the PoolID attribute, while the rest of the entities in the hierarchy initially do not have any type of identification.

This might lead us to think that the best way to implement the hierarchy is, once again, with a table for each entity. But this doesn't necessarily have to be the case because a single table can be used to implement multiple entities at once, using the table's identifier to distinguish between the entities. This is because we assume that the domain does not impose any restrictions, unlike in the Vehicle hierarchy where each type of vehicle had to have its own identifier.

Regarding the decision to implement a table for the superclass, whenever we have an incomplete hierarchy, we’ll need a specific table for the superclass – specifically to store information about pools that don’t belong to any type present in the inheriting entities. This means we need to include a Pool table.

Later, to decide whether to use that table to implement all entities in the hierarchy, only some of them, or to include a table for each inheriting entity, we need to look at the number of attributes the inheriting entities have. In this case, we see they have too many attributes, especially CruisePool and CityPool, so the simplest option is to implement a table for each entity in the hierarchy.

Another option we would have is to use the Pool table to also represent OlympicPool (which has the fewest attributes) and model the rest of the entities with specific tables. But this has disadvantages, such as the division in how we represent each type of pool.

For example, while we represent OlympicPool with some attributes in Pool that may or may not be NULL depending on whether the pool is of that type, the other types of pools would be represented differently. This can be confusing when querying the database.

We also need to consider that some foreign keys point to OlympicPool, so those foreign keys would only be valid for tuples in Pool whose corresponding attributes SpectatorMaxCapacity and CompetitionLanes aren’t NULL, greatly complicating database management, creating more constraints, and possibly complicating certain queries.

Although, no matter how complicated this option is, it would be possible to implement it, and it would be just as valid as implementing a table for each entity. That is, the complexity of an implementation can make it unfeasible but not incorrect – as long as the corresponding constraints are defined to maintain data integrity.

So even though in this case the simplest option is to use a table per entity, that doesn't mean there aren't other correct ways to implement the hierarchy. This means that from the entity-relationship diagram, we can’t infer the exact way it’s finally implemented, although it can be useful for making that decision.

CREATE TYPE PoolStatusType AS ENUM ('open', 'closed', 'maintenance', 'renovation');
CREATE TABLE Pool (
    PoolID SERIAL PRIMARY KEY,
    Name VARCHAR(32) NOT NULL,
    Address VARCHAR(32) NOT NULL,
    MinDepth INT NOT NULL CHECK (MinDepth >= 0),
    MaxDepth INT NOT NULL CHECK (MaxDepth >= MinDepth),
    Status PoolStatusType NOT NULL,
    CityFK INT NOT NULL,
    FOREIGN KEY (CityFK) REFERENCES City(CityID)
);
After deciding how to translate the hierarchy to the logical level, we add the table to the relational diagram and code it in the SQL DDL. As you can see, it’s very similar to the Vehicle table, with a surrogate key as an identifier, the entity attributes that characterize all pools, and a foreign key that references the City table. This determines the city where the pool is located (or manufactured in the case of a pool of the type CruisePool).

Regarding the status of the pool, we can see that it’s modeled here with a Status attribute. We define an ENUM TYPE for it to limit its domain. This design decision is justified because in this hierarchy we’re representing the types of pools in the inheriting entities, not their statuses. So to represent the statuses of the pools, we’ll need to use a different mechanism than generalization/specialization, such as a simple Status attribute.

There are other ways to model this, but they’d be more complex. This doesn’t make them wrong, but we won’t discuss or show them here.

To represent this attribute's data type in the entity-relationship diagram, we have chosen to define a «Enum» entity in UML with the possible values the attribute can take. Entities with the «Enum» type serve the same purpose as using a TYPE ENUM in SQL. This defines a set of values that can then be used as a data type for an attribute, thus restricting its domain.

But in general, this doesn’t have to be fully specified at the conceptual level. We could’ve simply used string as the data type and omitted this «Enum» entity, limiting its domain later at the logical level. Or rather, when the logical model is implemented in the DBMS.

Still, if we want our design to be as clear and self-descriptive as possible at all levels, we should indicate the possible values that attributes can take at all levels, as restricting the domain implicitly imposes an integrity constraint. We can do this through «Enum» entities, side notes, or by using other applicable standard UML elements.

CruisePool entity
Just as we did in the Vehicle hierarchy, here each type of pool is represented with a dedicated table. This way, when registering a new CruisePool type pool in our system, a tuple will be created in this table where the data characterizing cruise pools is stored. But the data that characterizes it as a pool won’t be stored there, as those can only be stored in the Pool table.

So to logically model the inheritance of all Pool attributes to the specific type of pool, we’ll use a foreign key to point to the Pool tuple that contains the rest of the pool information. Specifically, we’ll choose PoolID as the foreign key, as it’s the identifier of the pools in our system. We declare it it as the same SERIAL type to reference that same attribute in the Pool table, where it’s the primary key.

As you can guess, the Pool table not only stores information about pools that aren’t specifically modeled in our system, but it also contains information about pools of each of these types. So, if we want to get information about all the pools in our system, regardless of their type, we just need to query the Pool table.

This is possible because we have a table for the superclass, whereas in other hierarchies we might not implement it, which would require us to query multiple tables to get information about all the pools in the system. This is not necessarily a problem, but it’s worth considering when implementing the hierarchy or even modeling certain aspects of our domain with hierarchies.

For example, if we have a Pool and want to know its type, we must check the rest of the tables in the hierarchy to see if there is any tuple referencing that pool. This results in a very computationally expensive operation because it has to go through all the stored data. If our system needs to prioritize efficiency in such a query, it’d be helpful to modify the hierarchy implementation to make sure that this query runs as quickly as possible.

For instance, adding a redundant attribute in Pool to indicate the type, even though it introduces redundancy and unnecessary additional space, can greatly optimize the latency of certain queries. Just make sure you make these decisions according to project requirements, such as the latency that queries must have, the space the database should occupy, and so on.

CREATE TABLE CruisePool (
    PoolID SERIAL PRIMARY KEY,
    DeckNumber INT NOT NULL CHECK (DeckNumber >= 0),
    MaxCapacity INT NOT NULL CHECK (MaxCapacity >= 0),
    WaterTemperature DOUBLE PRECISION NOT NULL,
    SlideCount INT NOT NULL CHECK (SlideCount >= 0),
    ShipFK INT NOT NULL,
    FOREIGN KEY (PoolID) REFERENCES Pool(PoolID),
    FOREIGN KEY (ShipFK) REFERENCES CruiseShip(ShipID)
);
In addition to the foreign key pointing to Pool (which also serves as the primary key, making this table weak in identification with Pool as its owner entity), we have another foreign key referencing CruiseShip to determine the cruise on which the pool is located. And, since all cruise pools must be on a cruise ship to be of that type, the foreign key pointing to CruiseShip can’t be NULL. It must always reference a valid cruise. This is why we include the NOT NULL constraint, which we don’t do for PoolID because we are declaring it as the primary key.

CityPool entity
Another type of pool we can find is a municipal pool, represented by the entity CityPool and implemented with its specific table. Its DDL is very similar to the previous one, with the unique feature that in this case, we have a foreign key pointing to CityPool, which can be directly inferred from the 1-* type association connecting CityPool with Entry in the conceptual diagram.

CREATE TABLE CityPool (
    PoolID SERIAL PRIMARY KEY,
    MaxCapacity INT NOT NULL CHECK (MaxCapacity >= 0),
    AnnualBudget DOUBLE PRECISION NOT NULL CHECK (AnnualBudget >= 0),
    AccessibilityFeatures VARCHAR(32) NOT NULL,
    FreeWifi BOOLEAN NOT NULL,
    FOREIGN KEY (PoolID) REFERENCES Pool(PoolID)
);
In the relational diagram, it's important that the foreign key PoolID is underlined, indicating that this attribute, despite being a foreign key, is used to uniquely identify the tuples in CityPool. This means that when referencing the primary key of PoolID, the foreign key that refers to it contains exactly the value that identifies the pool in Pool.

So if a query simply needs the identifier of a pool of a specific type, we don’t need to access the Pool table, as the foreign key attribute of the CityPool, CruisePool, or OlympicPool table, for example, is enough to know it.

There are even times when we can access data from other tables that are indirectly associated through more levels of association, as in CruiseBooking, where we can access the identifier of a CruiseShip through the value of its foreign key, which doesn't point directly to CruiseShip, but to Voyage.

OlympicPool entity
Regarding the last type of pool in our schema, there's OlympicPool, which represents Olympic pools. The implementation of this entity as a table is the same as the previous ones, with the difference that in the entity-relationship diagram, we can see that there are two foreign keys pointing to OlympicPool. Otherwise, the only differences are in the attributes that characterize the type of pool.

CREATE TABLE OlympicPool (
    PoolID SERIAL PRIMARY KEY,
    SpectatorMaxCapacity INT NOT NULL CHECK (SpectatorMaxCapacity >= 0),
    CompetitionLanes INT NOT NULL CHECK (CompetitionLanes > 0),
    FOREIGN KEY (PoolID) REFERENCES Pool(PoolID)
);
Entry entity
Continuing with what a person can do in a pool in our system, we have the entity Entry. This is responsible for storing tickets that a person can use to enter a municipal pool, meaning one that is represented by the CityPool entity only.

To ensure that a person can only access a municipal pool with these tickets, the entity has a 1-* association with CityPool, and not directly with Pool, as that would give access to any pool regardless of type. Also, to know which person the ticket belongs to, it also has a 1-* association with Person, where a person can have an arbitrary number of tickets, but a ticket can only belong to one person.

On the other hand, we also have a 1-* association where the 1 side is in Entry, modeling that the tickets can have associated penalties, which we’ll see later. So, with all this, we can know that at a logical level, Entry will have 2 foreign keys pointing to other entities, as well as a foreign key from another entity pointing to Entry.

To uniquely identify the tickets, the most important attribute is EntryTimestamp. This records the exact time the ticket was purchased. But several people can buy tickets at the same time to enter the same pool, leading to multiple tuples with the same EntryTimestamp value, so the primary key must have more attributes to uniquely identify all the tickets.

Specifically, the primary key needs the foreign key attributes PersonFK and PoolFK to differentiate entries by the person who bought them and the pool they enter, as well as the exact time of purchase. So if we consider the possible situations and combinations of values that can occur for the primary key of Entry, we’ll see that a person can’t buy multiple entries at the exact same moment to enter the same pool.

This makes sense when the domain states that each ticket is associated with a single person and that a person can’t buy a ticket for someone else. In other words, if a person buys a ticket, they must use it themselves. They can’t buy multiple tickets for several people to enter. This doesn't have to be the case in all domains – we're just assuming here that people can't buy tickets for others.

In other domains, this might need to be modeled differently depending on the requirements. So, we’ll need to make sure that our model meets these types of requirements imposed by the domain, especially when defining primary keys or UNIQUE constraints.

So by requiring the attributes PersonFK and PoolFK to be present in the primary key, the Entry entity becomes weak in identification with two owning entities, Person and CityPool, respectively. In the DDL, we have explicitly added the NOT NULL constraint to all attributes for clarity, although it wouldn't be necessary for those present in the primary key.

CREATE TABLE Entry (
    EntryTimestamp TIMESTAMP NOT NULL,
    Price DOUBLE PRECISION NOT NULL CHECK (Price >= 0),
    PaymentMethod PaymentMethodType NOT NULL,
    AppliedDiscount DOUBLE PRECISION NOT NULL CHECK (AppliedDiscount >= 0),
    Duration INT NOT NULL CHECK (Duration >= 0),
    PersonFK INT NOT NULL,
    PoolFK INT NOT NULL,
    PRIMARY KEY (EntryTimestamp, PersonFK, PoolFK),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID),
    FOREIGN KEY (PoolFK) REFERENCES CityPool(PoolID)
);
On the other hand, the EntryTimestamp attribute of type TIMESTAMP is named differently from the IssueTime attribute of the BusTicket entity, for example.

This isn't very important, but in a real design process, we might be required to use style guides that determine how we should name each attribute depending on its semantics, type, or constraints, as well as when and how we should declare certain constraints. In this specific case, we didn't follow any style guide – we simply named the attributes as descriptively as possible according to the circumstances. Still, following a style guide offers advantages in system maintainability and ease of administration, among others.

Team entity
To hold competitions in Olympic pools, our system needs to be able to model sports teams made up of people who participate in these competitions. So we have the entity Team, which represents sports teams that have a reference Olympic pool, are made up of people, and participate in competitions in Olympic pools.

This is modeled by using the attributes of Team to store the characteristics of the sports teams, such as the name, creation date, uniform color, and so on. We can also use associations with other entities to determine which Olympic pool is the team's official pool, which people belong to the team, who coaches the team, and which competitions they participate in.

First, to model the Olympic pool considered the team's official pool, we’ll use a 1-* association with OlympicPool, which becomes a foreign key due to its cardinality. As you can see in the conceptual diagram, the role of the association specifies the semantics, since without it, you can’t directly infer what is being modeled with that association.

The same applies to the 1-* association with Person, which we use to determine who coaches the team, so we need to specify its semantics to avoid confusion about what that association actually models. Although, given their cardinalities, it’s clear that a team can only have one person as a coach, not an arbitrary number of people, so we can rule out that the association models the people who belong to the team.

On the other hand, besides the foreign keys that Team has, there are others that point to Team and are responsible for modeling the people who make up the team, such as the one from Membership, or the team's participation in competitions, like the one we see from Participation.

CREATE TYPE SportType AS ENUM ('waterpolo', 'swimming', 'diving');
CREATE TABLE Team (
    Name VARCHAR(32),
    CreationDate DATE NOT NULL,
    ClothColor ColorType NOT NULL,
    Sport SportType NOT NULL,
    Budget INT NOT NULL CHECK (Budget >= 0),
    ContactEmail VARCHAR(32) NOT NULL,
    CoachFK INT NOT NULL,
    HomePoolFK INT NOT NULL,
    PRIMARY KEY (Name, CoachFK),
    FOREIGN KEY (CoachFK) REFERENCES Person(PersonID),
    FOREIGN KEY (HomePoolFK) REFERENCES OlympicPool(PoolID)
);
To uniquely identify each team, the attribute that can serve us best from the table itself is Name. But in this domain, we assume that multiple teams can have the same name, so the primary key can’t be formed solely by that attribute.

So from all the other attributes we have, we finally include the foreign key CoachFK in the primary key, meaning we also use the information of the person who coaches the team to uniquely identify it. This works because we assume that there can’t be multiple teams with the same name coached by the same person.

At first glance, this might seem entirely possible, but consider that some domain requirements might impose this condition, which we can leverage to define (Name, CoachFK) as the primary key. In any case, before making such a decision, make sure that the set of attributes meets the primary key restriction, either due to domain requirements or the semantics of the attributes themselves.

We can declare foreign keys with FOREIGN KEY referencing the primary key of Person and OlympicPool. We impose the NOT NULL restriction on them since all teams must have a coach and an official Olympic pool. Here, we have also assumed the necessity of these elements, but in other cases it might not be mandatory to have an official pool or a coach – it all depends on the domain.

If having a coach were not mandatory, we couldn’t include the foreign key attribute CoachFK in the primary key, as it could be NULL and would violate the primary key restriction. So for an entity to be weak in identification and another to be its owner, the association between them must be mandatory, meaning its minimum cardinality on the owner's side can’t be 0.

Finally, we define a TYPE ENUM here for the type of sport the team plays, which is stored in the Sport attribute. But we don’t need to redefine it for the Color attribute, as we had the ENUM ColorType defined earlier, which is the best example of how a data type is reused across attributes with the same domain in different entities.

Membership entity
We’ll continue with the semantics of the previous entity. To model the possibility of people being part of a team, the simplest approach would be to include an N-M association between Person and Team. This would be in addition to the 1-* association that already exists to model the person who coaches the team. This way, a person can belong to an arbitrary number of teams, while a team can be composed of an arbitrary number of people.

But since this association requires an intermediate entity to be implemented at the logical level, and we also need to store information about a person's membership in a team, we’ll introduce the Membership entity. This entity divides the N-M association into several 1-* associations, indirectly connecting Person with Team. In this way, each person belonging to a team will have a tuple in this table representing their membership. It’ll store information such as the start or end date of membership or the fee they must contribute to the team to be part of it.

At the conceptual level, we can see that this entity has many similarities with others like Residence. For example, we define the primary key of this entity with the attribute JoinDate and the foreign keys that determine the Person who belongs to a certain team. This is because the attributes that appear exclusively in the entity can’t uniquely identify each membership. That is, there can be multiple people who started belonging to different teams on the same date, causing multiple tuples in Membership with the same value in their primary key.

So even though the foreign key attributes don’t explicitly appear at the conceptual level, it’s clear that a Membership tuple must be identified not only by the start date but also by the person and team it relates to. This will avoid situations where multiple tuples with the same person and date are considered equal, or with the same team and start date. So we know it’s a weak entity in identification that’s dependent on Person and Team.

Since it depends on both entities it’s related to for identification, we could have represented it as an associative entity connected with the possible N-M association between Person and Team. But to make the diagram as clear and close to the logical level as possible, we should instead use an "intermediate" entity like the one represented here with 1-* associations.

CREATE TYPE PaymentFrequencyType AS ENUM('monthly', 'anual', 'weekly', 'quarterly');
CREATE TABLE Membership (
    JoinDate DATE NOT NULL,
    LeaveDate DATE CHECK (
        LeaveDate IS NULL
        OR LeaveDate >= JoinDate
    ),
    FeeAmount INT NOT NULL CHECK (FeeAmount >= 0),
    PaymentFrequency PaymentFrequencyType NOT NULL,
    AutoRenewal BOOLEAN NOT NULL,
    PersonFK INT NOT NULL,
    TeamNameFK VARCHAR(32) NOT NULL,
    CoachFK INT NOT NULL,
    PRIMARY KEY (JoinDate, PersonFK, TeamNameFK, CoachFK),
    FOREIGN KEY (PersonFK) REFERENCES Person(PersonID),
    FOREIGN KEY (TeamNameFK, CoachFK) REFERENCES Team(Name, CoachFK)
);
To implement the foreign keys, we’ll create the corresponding attributes: PersonFK, which is the foreign key pointing to Person, and (TeamNameFK, CoachFK), where both constitute the other foreign key referencing the team to which the person belongs. Both keys are not null because a Membership tuple must associate a person with a team.

Once we’ve declared the attributes and FOREIGN KEY constraints, we can define the primary key as the set of attributes consisting of JoinDate, the foreign key attribute PersonFK, and the other two attributes (TeamNameFK, CoachFK) of the foreign key referencing the team. We can declare them in any order in the PRIMARY KEY constraint, as long as they all appear.

Finally, according to the domain, we assume that people don’t know exactly when they will stop being members of a team, so LeaveDate doesn’t always have to be defined. This means it can be NULL until the person leaves the team or plans to leave on a specific date. So we have to define a CHECK constraint on that attribute to make sure that it’s either NULL or the date is after JoinDate, as a person can’t leave a team before the start date of membership.

Participation entity
Similarly, a sports team can also participate in sports competitions registered in SwimmingCompetition. So we have an entity called Participation that indirectly links Team with SwimmingCompetition through 1-* associations. This is just like we saw earlier with Membership, but with a different meaning. Specifically, what mainly changes is the information stored about the team's participation in a competition, such as the date they register to participate, their ranking position after the competition, or the time it took to complete the competition.

To uniquely identify the tuples of Participation, the simplest way is to use a custom database identifier as a surrogate key, just as we have done before with certain entities. But if the domain requirements don’t allow us to include surrogate keys or any additional database-specific identifier, we’ll need to choose a set of attributes that enable identification.

So if we assume that no team participates more than once in the same competition (as this wouldn't make sense), we can declare a primary key formed by the foreign keys referencing Team and SwimmingCompetition. This way, we ensure that different tuples of Participation don’t associate the same team with the same competition, as that situation can’t occur.

As you can see, identifying this entity completely depends on other entities like Team and SwimmingCompetition, meaning there is no attribute at the conceptual level of the entity that forms part of the primary key.

This isn’t necessarily a bad thing, but rather a consequence of the domain requirements preventing us from using a surrogate key. In fact, this dependency in identification can have certain advantages, such as avoiding additional columns, which might be a domain-imposed requirement (to use the fewest columns possible).

CREATE TABLE Participation (
    RegistrationDate DATE NOT NULL,
    Rank INT NOT NULL CHECK (Rank > 0),
    RecordedTime DOUBLE PRECISION NOT NULL CHECK (RecordedTime >= 0),
    NameFK VARCHAR(32),
    StartDateFK DATE,
    EndDateFK DATE,
    TeamNameFK VARCHAR(32),
    CoachFK INT,
    PRIMARY KEY (
        NameFK,
        StartDateFK,
        EndDateFK,
        TeamNameFK,
        CoachFK
    ),
    FOREIGN KEY (TeamNameFK, CoachFK) REFERENCES Team(Name, CoachFK),
    FOREIGN KEY (NameFK, StartDateFK, EndDateFK) REFERENCES SwimmingCompetition(Name, StartDate, EndDate)
);
At the logical level, the foreign key referencing Team has two attributes, which make up the primary key of Team. The foreign key pointing to SwimmingCompetition has three for the same reason. So we’ll use two FOREIGN KEY constraints: one to declare the foreign key pointing to Team and the other for the one pointing to SwimmingCompetition, respectively.

Note that the FOREIGN KEY constraint only allows one REFERENCES clause. So if we have multiple foreign keys pointing to various entities, we have to use a separate FOREIGN KEY constraint for each foreign key. If we try to declare them all with a single constraint, we would have to indicate the multiple entities/tables being referenced, which means we would need to use multiple REFERENCES statements.

After declaring the foreign keys and adding their respective NOT NULL constraint, since it’s mandatory for a participation to relate a team with a competition, we declare the foreign key as the set of attributes that form both foreign keys together. So in our system, there can be Participation tuples with different Rank or RegistrationDate values without any problem – but there can’t be multiple tuples with the same value in their primary key (meaning they can’t relate the same team with the same competition multiple times).

Finally, if we try to reconstruct the conceptual entity from the relational diagram, the first thing we should notice is that all the foreign keys are underlined, and therefore form the primary key. As they are foreign keys, these attributes won’t appear in the conceptual entity of Participation.

To determine how many foreign keys we actually have, and know how many 1-* associations to introduce and with which entities to connect them, we can see that a subset of attributes like (TeamNameFK, CoachFK) refers to the same entity – so there will be a 1-* relationship with that entity, with the many side in Participation. Doing the same with the attributes (NameFK, StartDateFK, EndDateFK), we see that they all refer to attributes of the same entity. So they form a foreign key that results in a 1-* association like the previous one, but connecting with another entity.

To infer the minimum cardinalities, we should look at the constraints indicated in the relational diagram: which foreign keys can or can’t be NULL, or how many participations each competition must have (as well as the participations each team must have).

In this case, we haven’t indicated any constraints in the relational diagram for simplicity. But, for example, if we were told that a foreign key can’t be NULL in the relational model, this conceptually translates to its respective 1-* association having a minimum cardinality of 1 on the 1 side. Similarly, if there are special constraints that require each team to have 2 participations, for example, then we know that in its corresponding association, the minimum cardinality on the Participation side would be 2.

This reverse process is what we initially followed to implement the entity at the logical level, where the attributes that make up each foreign key are inferred, and those selected to declare the primary key.

SwimmingCompetition entity
To model the sports competitions that can take place in an Olympic pool, in the conceptual diagram we have the entity called SwimmingCompetition that’s responsible for storing information about the competitions held in all the Olympic pools registered in the system. In these, any number of sports teams can participate.

The information that SwimmingCompetition stores mainly depends on the domain and requirements. In this case, we assume that we only need to store the name of the competition, start and end dates that will always be determined, a RecordTime attribute to store any record times achieved during the course of that competition, and the monetary amount of the prize for that competition.

With these attributes, the simplest way to uniquely identify each tuple in the SwimmingCompetition table is to define the set of attributes (Name, StartDate, EndDate) as the primary key.

For example, there can be competitions in the database with exactly the same name, but they can never have the same start and end dates simultaneously (because that would mean they were the same competition). Ultimately, by declaring this primary key, we’re assuming that there are no different competitions with the same name and start and end dates – so if this condition aligns with the domain requirements, it would be correct.

Consequently, there can be different competitions in the database with different combinations of values for the primary key attributes – but they might have the same record time, or the same prize in PrizeAmount, since there are no restrictions preventing it.

CREATE TABLE SwimmingCompetition (
    Name VARCHAR(32),
    StartDate DATE NOT NULL,
    EndDate DATE NOT NULL CHECK (EndDate >= StartDate),
    RecordTime DOUBLE PRECISION CHECK (RecordTime >= 0),
    PrizeAmount INT NOT NULL CHECK (PrizeAmount >= 0),
    PRIMARY KEY (Name, StartDate, EndDate)
);
In the conceptual diagram, we see that the entity has several associations that lead to the existence of a foreign key pointing to OlympicPool, since the competition must necessarily take place in an Olympic pool. So this foreign key references the specific pool where the competition is held, making its existence mandatory. In other words, the foreign key can’t be NULL because, in the conceptual model, we set the minimum cardinality to 1 to ensure that every competition is associated with a pool where it takes place.

Another peculiarity of this entity is that the attribute RecordTime may not always be defined. For example, when we register a competition in the database and need to provide a value for this attribute, such value might not exist because it’s the first time the competition is being held. So, the simplest way to model it would be to set that attribute to the maximum or minimum possible, depending on how we consider which times are better than others.

Additionally, since in our domain we also model the possibility of athletes participating in a competition being sanctioned, there is a chance that in a certain competition held for the first time, all participants could be sanctioned. This means that none of them would contribute to initializing the value of the RecordTime attribute. This is why it needs to be allowed to be NULL.

But this is a decision we must make primarily considering the domain and its requirements, as we may want to initialize the attribute with a default or special value if all athletes are penalized and the counter can’t be initialized, for example.

Sanction entity
Given everything a person can do in our domain in relation to other entities, they might break a rule that results in a sanction. So in our schema, we can introduce an IS-A hierarchy where the superclass is the entity Sanction, and its inherited entities are the different types of sanctions we define, all depending on their scope of application.

Specifically, deciding to use a hierarchy to model sanctions is driven by the specific information that needs to be stored for each type of sanction. For this reason, if we tried to use a single Sanction entity to represent all these types, its semantics would be very complicated (as some attributes would only be useful if the sanction were of a certain type – and the same goes for many others). We would also need to use a specific attribute to represent the sanction type, since otherwise knowing the exact type might depend on which attributes were NULL, and this would complicate queries.

So with this hierarchy, we can have a set of common attributes for all sanctions in Sanction, such as the monetary amount of the fine, the description, the date of the sanction, or the status, while in the inherited entities, we have specific attributes that characterize each type of sanction.

How is the IS-A hierarchy implemented with tables?
Just as we have done with other hierarchies, we need to analyze it to know how to implement it at the logical level. We need to keep in mind that the conceptual design doesn’t unequivocally determine the implementation that we’ll ultimately carry out, especially when working with IS-A hierarchy. Rather, it’s a decision we should make based not only on the conceptual design itself, but also on the domain and data requirements.

To do this, we first check whether the hierarchy is complete or not. In this case, all existing sanctions will be of a specific type represented by the inherited entities. This means that all individuals in the hierarchy will belong to one of the sets generated by these entities, which implies that the hierarchy is complete.

On the other hand, the types of sanctions are all disjoint, meaning a sanction can only be of one type, not several at once. This means that the hierarchy is disjoint because no individual will be represented by multiple inherited entities at the same time.

To identify each sanction, we’ll use a SanctionID attribute in the superclass Sanction, which we’ll implement using a surrogate key. This avoids the inherited entities needing to use their own identifiers, as we assume that the domain requirements don’t require us to identify each type of sanction differently.

So given the number of attributes each inherited entity has, it’s clear that we’ll need a table to implement each inherited entity. Otherwise, too many NULL values would be generated in the corresponding attributes, complicating both database management and queries, and potentially leading to unnecessary constraints aimed at ensuring schema integrity.

On the other hand, we have several options for implementing the superclass at the logical level. One option is to duplicate all attributes in each of the tables of the specific sanction types.

This has advantages, such as identifying each table using the SanctionID attribute inherited from the superclass, but it leads to schema management problems. If we later want to delete, modify, or add an attribute of Sanction, we would have to perform that operation on all the tables of the different sanction types, increasing the likelihood of errors in the process. Also, if we want to query all the sanctions in our database, with this option, we would have to go through all the tuples of all the tables of each sanction type, which could be inefficient because of accessing multiple tables.

To minimize errors in the database management process, we need to simplify the operations involved as much as possible. To do this, we can implement a specific table for the superclass of the hierarchy in the same way as we did with the Vehicle hierarchy (and for similar reasons).

Each of the tables for the inherited entities will have a foreign key referencing the superclass table, where we’ll store the information of attributes common to all sanctions. This makes it easier to modify these attributes. It also simplifies other operations, such as adding a new type of sanction. For this, only a new table for that type needs to be created, and we just need to make sure that its foreign key references Sanction.

If we look at the inherited entities, we’ll see that each one has a 1-* association with other entities where the many side is always in the entities of the hierarchy. This means that using a single table to implement the entire hierarchy isn’t a good idea, as it would combine all those foreign keys into one table. This would lead to much more complicated integrity constraints.

In other words, if a sanction is of a specific type, the attributes and foreign keys of the remaining types must be NULL, so ensuring this for all types involves overly elaborate and complex constraints.

CREATE TYPE SanctionStatusType AS ENUM ('created', 'active', 'expired');
CREATE TABLE Sanction (
    SanctionID SERIAL PRIMARY KEY,
    Amount DOUBLE PRECISION NOT NULL CHECK (Amount >= 0),
    Description VARCHAR(32),
    IssueDate DATE NOT NULL,
    ExpirationDate DATE CHECK (
        ExpirationDate IS NULL
        OR ExpirationDate >= IssueDate
    ),
    Status SanctionStatusType NOT NULL
);
In the DDL of the Sanction table, we can see that its primary key {SanctionID} consists of a single attribute of type SERIAL. This is a surrogate key that will be used to identify all sanctions in the database, regardless of their type.

This table also stores the status of the sanction in an attribute, since the type of sanction is represented with inherited entities from the superclass at the conceptual level. So the status must be modeled as an attribute to avoid mixing the semantics of what we represent with each tool of the entity-relationship diagram.

In other words, we could include new inherited entities that model the states of the sanctions, but we have to consider that each type of sanction could be in any of those states – leading to an unnecessarily complicated multi-level hierarchy.

Because of this, we should separate the semantics of what we represent with inherited entities from the semantics of the sanction's status, modeling it with an attribute in the superclass, as any type of sanction can be in any state.

In this Status attribute, we define a TYPE ENUM to restrict the possible states a sanction can have. But for the Description, if we want to save a description of the sanction written in natural language, we shouldn’t add restrictions unless it’s required by the project specifications.

A description in natural language can be very diverse, so the simplest approach is not to limit the possible values the attribute can take, not even with a NOT NULL constraint. This can indicate that a sanction has no description, although this isn’t necessarily correct.

In general, decisions to allow NULL values also depend on the domain and requirements. For example, sanctions may or may not have an expiration date, which is why the CHECK constraint defined on ExpirationDate specifies that this attribute can either be NULL or must hold a date later than the issuance date of the sanction.

DrivingSanction entity
Let’s now talk about the different types of sanctions in our system. First, we have DrivingSanction, which are sanctions associated with driver's licenses. So in the conceptual diagram, it has a 1-* association with the DrivingLicense entity, resulting in a foreign key in DrivingSanction that references the driver's license with the sanction. This refers to the license of the person who committed a traffic violation, leading to the existence of the fine.

The specific attributes of this type of sanction are store information about why the fine was issued, such as the speed the vehicle was going, as well as the effect the sanction has on the license (like deducting a certain number of points or suspending it for a certain period).

In its DDL, we can see that all attributes have been declared as NOT NULL, which at first might seem unnecessary in the case of RecordedSpeed, since not all sanctions are caused by speed. But this illustrates that even if an attribute isn’t necessary, it shouldn’t be NULL to be considered unnecessary.

For example, if a sanction is not related to speed, instead of using a NULL value in the RecordedSpeed attribute, we can use a special value like 0, as long as it respects the integrity constraints and system domain requirements. This allows us to distinguish whether the sanction is related to a possible speeding violation. So we make the decision to allow NULL or not is initially when modeling the entity at the logical level. This works as long as we aren’t forced to use a specific semantics like setting the attribute to 0 when it’s not necessary.

If we consider whether other attributes can be NULL or not, we can see that PermanentSuspension always has the option to take the value false (as the suspension might not be permanent). Similarly, if the suspension is permanent, the SuspensionDays attribute can always be set to 0, or to a different special value. We could also simply ignore its value and check first if the suspension is permanent before accessing the SuspensionDays attribute, among other options.

CREATE TABLE DrivingSanction (
    SanctionID SERIAL PRIMARY KEY,
    RecordedSpeed DOUBLE PRECISION NOT NULL CHECK (RecordedSpeed >= 0),
    PointsDeducted INT NOT NULL CHECK (PointsDeducted >= 0),
    SuspensionDays INT NOT NULL CHECK (SuspensionDays >= 0),
    PermanentSuspension BOOLEAN NOT NULL,
    LicenseFK INT NOT NULL,
    FOREIGN KEY (SanctionID) REFERENCES Sanction(SanctionID),
    FOREIGN KEY (LicenseFK) REFERENCES DrivingLicense(LicenseID)
);
On the other hand, the NOT NULL is indeed necessary for both foreign keys in the table, as SanctionID is the foreign key that references the tuple in Sanction that holds the rest of the sanction information. Its primary key attribute SanctionID serves as the primary key of the DrivingSanction table itself, and it’s the only way to uniquely identify the sanctions. Also, the foreign key that references the driving license that received the sanction can’t be NULL either, because if the sanction is of type DrivingSanction, it must necessarily be associated with a license.

SportSanction entity
Another type of sanction is represented in the entity SportSanction. This models those sanctions that occur in sports competitions, specifically those caused by a sports team while participating in a competition. Like the previous entity, it has attributes that characterize this type of sanction, such as the number of competitions the team is suspended or the name of the referee who issued the sanction.

In addition to this information, each sanction of this type needs to know which specific team received the sanction, as well as the competition they were participating in when they were sanctioned. So to model this, we have multiple options. We could use two 1-* associations to connect SportSanction with Team and with SwimmingCompetition, so that from the sanction you can identify the corresponding team and competition. But this is redundant and unnecessary, as it would lead to two foreign keys that can actually be reduced to one.

Remember that in our schema, we have an entity called Participation that relates teams to the competitions they participate in. So instead of two 1-* associations in SportSanction, we can use just one that connects it with Participation, since from Participation we can determine the team and competition.

CREATE TABLE SportSanction (
    SanctionID SERIAL PRIMARY KEY,
    SuspendedCompetitions INT NOT NULL CHECK (SuspendedCompetitions >= 0),
    RefereeName VARCHAR(32) NOT NULL,
    NameFK VARCHAR(32) NOT NULL,
    StartDateFK DATE NOT NULL,
    EndDateFK DATE NOT NULL,
    TeamNameFK VARCHAR(32) NOT NULL,
    CoachFK INT,
    FOREIGN KEY (SanctionID) REFERENCES Sanction(SanctionID),
    FOREIGN KEY (
        NameFK,
        StartDateFK,
        EndDateFK,
        TeamNameFK,
        CoachFK
    ) REFERENCES Participation(
        NameFK,
        StartDateFK,
        EndDateFK,
        TeamNameFK,
        CoachFK
    )
);
To implement this in SQL, we can create a table very similar to DrivingSanction, where its primary key is the attribute SanctionID, also declared as a foreign key referencing the Sanction table of the superclass. We can declare attributes in the same way as we have been doing so far, both for the attributes of the entity itself and for the foreign keys. The foreign keys must have the same data types as the attributes they reference.

In this case, to declare the foreign key that points to Participation, we need as many attributes as its respective primary key has, which is a total of 5. To simplify this process, the ideal approach is to look directly at the PRIMARY KEY constraint of the table we want to reference. Then for each of those attributes, we can declare it in our table with a characteristic name and the corresponding data type. We finally add it to the FOREIGN KEY constraint so that it references the attribute that originated it, as we have already seen.

For example, if the primary key of Participation is (NameFK, StartDateFK, EndDateFK, TeamNameFK, CoachFK), then we declare an attribute NameFK for the foreign key of SportSanction that points to the NameFK attribute of that primary key, another StartDateFK that points to the StartDateFK attribute of the primary key of Participation, and so on.

PoolSanction entity
To conclude the hierarchy of sanctions, we have PoolSanction. These, as you can guess, are sanctions imposed on people who have entered a CityPool and violated the pool rules. In this case, we store start and end dates as attributes, indicating the period during which the person can’t enter the pool. We can also include an amount as compensation if necessary, or a number of community service hours that the person must complete.

To determine from the sanction which person and pool are affected by the sanction, we can use a 1-* association with Entry. This results in a foreign key in PoolSanction that points to Entry because the many side is placed in PoolSanction. This way, we can identify the entry the person used when they received the sanction.

Besides the person, the entry also provides information about the pool they will no longer be able to enter freely. The sanction determines when they can re-enter or the action they must take due to being sanctioned.

CREATE TABLE PoolSanction (
    SanctionID SERIAL PRIMARY KEY,
    BanStartDate DATE,
    BanEndDate DATE,
    CompensationRequired INT NOT NULL CHECK (CompensationRequired >= 0),
    CommunityServiceHours INT NOT NULL CHECK (CommunityServiceHours >= 0),
    EntryFK TIMESTAMP NOT NULL,
    PersonFK INT NOT NULL,
    PoolFK INT NOT NULL,
    FOREIGN KEY (SanctionID) REFERENCES Sanction(SanctionID),
    FOREIGN KEY (EntryFK, PersonFK, PoolFK) REFERENCES Entry(EntryTimestamp, PersonFK, PoolFK),
    CHECK (
        (
            BanEndDate IS NULL
            AND BanStartDate IS NULL
        )
        OR BanEndDate >= BanStartDate
    )
);
In its DDL, we can see that the table identification is the same as the previous ones, with a primary key composed of the foreign key pointing to Sanction, as well as another foreign key pointing to Entry (which consists of three attributes). In this case, the sanction can impose several conditions on the sanctioned user: it can either prohibit them from entering for a period of time, require them to pay compensation, or serve a certain number of community service hours.

So if we assume that the domain and requirements don’t force us to store NULL values in any attribute and that we can make any decision about how data is stored in the system, we’ll decide to allow BanStartDate and BanEndDate to be NULL for sanctions that don’t prohibit the sanctioned user from entering the pool. Thus, in the CHECK constraint defined at the end, we see that as an integrity condition for all tuples in the table, both attributes must be either null or the end date must be after the start date of the prohibition. This ensures that only valid data is stored in the table.

Lastly, we can see that some attributes of the foreign key pointing to Entry are named exactly the same as the attributes they reference, like personFK or PoolFK. This is neither a problem nor an error, although in a larger project where each table has more attributes, we should follow a proper style guide when naming attributes, especially those reserved for foreign keys. This way, we can more clearly understand their purpose without having to spend time analyzing the schema in detail.

How to Create the Database
Now that you understand the domain semantics and have completed the conceptual and logical design phases, we can implement the logical model on the DBMS.

The easiest way to do this is by creating a script with a .sql extension that contains all the necessary DDL code to populate the database – that is, the statements we just reviewed where we create tables, data types, and constraints.

But since we aren’t working with a real project database here, we don't need to worry about the data that might be in tables that already exist in the database, especially those with the same name as any of the tables we’ll going to create. So for simplicity, before creating them, we’ll execute some DROP statements to remove tables with names matching any of the tables we are going to create. This will make sure that they contain no tuples.

Following this process, we’ll arrive at a DDL script like this (it’s quite long, so I’ve left it in the gist).

When we run the script, keep in mind that the statements will execute one by one from top to bottom. So we first use the DROP statements to remove any tables in the database that have the same name as any of those we’ll create.

This process is equivalent to deleting our entire database – that is, our logical model that was once created – so we first need to remove the tables that aren’t referenced by any foreign keys to maintain integrity while deleting the remaining tables.

Then, under the same condition, all corresponding tables that aren’t referenced by any foreign keys are successively deleted until no tables remain to be deleted.

There should now be no tables in our database whose names conflict with the tables in our logical model, so we write the CREATE TABLE statements we saw earlier for each table in the logical model.

We also need to do this in a specific order, specifically the reverse of the deletion process. Here, we first need to create tables that don’t have any foreign keys pointing to another entity. If we create a table at the beginning that needs to reference another table that hasn't been created yet, the DBMS will generate an integrity error. So as you can see in the script, we place the statements in an order such that whenever a table with foreign keys pointing to other tables is created, those tables have already been created beforehand.

To figure out how we need to order both the DROP and CREATE TABLE statements, there are algorithms like topological sorting that we can apply to the relational diagram. This way, we treat the database schema as a directed graph made up of nodes (tables) and directed edges (foreign keys). With this algorithm, for example, we can progressively remove minimal or maximal nodes from the graph, creating or deleting the table they represent. But, this is not the only method available.

Regarding data types and constraints defined in assertions or triggers, the order of creation is easier to infer. This is because the ENUM or DOMAIN types must always be created before being used in a table's attribute declaration. So the simplest approach is to create them at the very beginning, or just before we use them for the first time (what we’ve done here).

On the other hand, it's best to define assertions or triggers at the end. We also want to give them names descriptive enough of the constraints they model, as their definitions may involve multiple tables that we need to create before defining the constraint. Also, since these elements don’t contain data (tuples), we don’t need to delete them at the start of the script unless we are going to modify the schema itself. In that case, some constraints might become obsolete, meaning they access attributes or tables that no longer exist.

In summary, with this SQL script, we create the tables, data types, and constraints that make up our database schema, ensuring that none of them contain tuples immediately after being created.

But to run the script, we need to create a database in the DBMS. Let’s use the CREATE DATABASE statement to create a new database with a specific name:

 CREATE DATABASE ExampleDataBase OWNER postgres;
If we run this command on the DBMS terminal, we will create a completely empty database named "exampledatabase". Note that PostgreSQL is not case-sensitive for element names or SQL statements. So even if we write an element's name in uppercase, when we later check the name value stored by the DBMS for the database, we’ll see it in lowercase.

We can also assign an owner user, who will have all the privileges over that element. By default, we can make the owner user postgres, but we can change it later with a statement like the following:

ALTER DATABASE exampledatabase
OWNER TO user3; /*user3 is a sample user*/
Once we’ve created the database, we can connect to it using the DBMS command \c exampledatabase. Finally, we can execute the .sql script with the command \i /path_to_script/script.sql. The DBMS should then notify us that the DROP statements have had no effect since there is no table with the corresponding name to delete (the database is empty). But, after creating the tables, if we run the script again, the DROP statements will delete them because they are created, preventing the DBMS from giving us these notifications.

Similarly, if any statements encounter errors that prevent their execution, or in special situations like the one we just mentioned, the DBMS will notify us – but it won’t stop the execution of the script. It will simply move on to execute the next declared statements (at the syntactic level, it executes the next statement we have separated with the corresponding ;).

Chapter 11: Example Queries
Once we have done all this, we’ll have the database created and populated with tables. But these tables are empty, meaning they don't contain any tuples. So if we want to run queries on them that return any results, we need to execute INSERT statements to add tuples to all the tables.

In this case, since the database is an example, we don't have real data to use for populating the tables, and there's no simple and automatic way to fill them with synthetic data. The best option is to use the Python library faker and create a script to generate this synthetic data (I’ve explained this in this Jupyter Notebook).

There is also always the option to look for real data sources to populate our database. But when doing this, those data sources might provide information in table schemas that don't exactly match those of our database tables, requiring us to integrate and then insert the information through a process like an ETL. These ETL processes of integration and insertion are often applied in Data Warehouses, which can also be a database like ours.

Running Basic Queries
So, assuming we already have the database populated with tables and tuples within them, we can run different queries on them. After all – the main operation that other services from other software layers use from the database is querying. This lets them obtain data that they can then transform, use to calculate certain metrics, or simply display to the end user.

For example, right after inserting the data, the first query we can run to ensure that the insertion process worked is the following:

SELECT 'person' AS tableName, COUNT(*) AS numberOfTuples
FROM Person;
As you can see, we use the FROM clause to get all the information stored in the Person table (which we could have written entirely in lowercase). Then, we use the aggregation function COUNT(*) to count the total number of tuples in the table, naming the column where this number is stored numberOfTuples.

But, if we also want to display the table name in the same tuple as the previous count, we can add another column in the SELECT statement where all its values are 'person'. This way, when the query is executed, it will return a table with two columns, one tableName and another numberOfTuples. Since the aggregation function only returns one value, the resulting table will have only one tuple, where the tableName column will have the value 'person' and the other column will show the number of tuples in the Person table.

If we want to count the tuples of all the tables in the database, we have the option to create a larger query that gathers all the results of the sub-queries that count the tuples of each table. For this, we can use UNION ALL, which combines the tuples from all resulting tables into a single table. This works as long as all resulting tables have exactly the same schema, with the same column names and data types, as in this case.

SELECT 'person' AS tableName, COUNT(*) AS numberOfTuples
FROM Person
UNION ALL
SELECT 'city' AS tableName, COUNT(*) AS numberOfTuples
FROM city;
Lastly, when we say "obtain information" about an element of the domain or database schema in this context, we mean getting its data stored in the attributes of the table that represents it.

For example, information about a person could be the Name or Email attribute of the Person table, among others. We won’t detail that info here, as in most cases, it’s easy to modify which attributes are selected to return as a query result. But in a real environment, it’s convenient and important to pay attention to the attributes the query should return, the names/aliases they should have, and the order in which they should be returned. The functionality of other software layers often depends on this step being performed correctly.

Tuple Filtering
The query we just looked at is useful for managing the database. Knowing how much information is stored in each table helps us make sure that certain normalization or schema transformation operations have run correctly (and even that the information itself is correct).

Let’s now look at some other queries that allow us to execute services provided to the end user. We use these to operate on the domain according to its semantics, so they can be very diverse. Here, we’ll distinguish between different types of queries based on their approach and the SQL tools used in their construction.

First, we have a series of queries for tuple filtering. These queries apply a filter on a table to keep only certain tuples that meet specific conditions. Note that the table containing the tuples we want to filter can be generated in any way, whether through a JOIN, a set operation, or whatever we want to do. But if you need to perform a grouping with GROUP BY, the resulting table must be filtered using a HAVING clause, which differs from the usual WHERE clause used to filter tuples.

SELECT *
FROM person P
WHERE P.name LIKE 'Carol%';
The above is a simple example that retrieves the tuples from the Person table for people whose names starts with “Carol“. As you can see, the only statement we need to filter tuples is WHERE. In it, all the conditions required for filtering are defined, regardless of their number or nature, as some will be performed using subquery results.

In this specific case, the query has the condition that a person's name must start with exactly the string that appears in the LIKE operator. Since it’s case-sensitive, the string has to match exactly what we want to search or filter. Then all tuples that meet this condition will be returned in the resulting table. We’ll get all of its attributes because of the SELECT * notation we used.

To illustrate that it doesn't matter whether you use uppercase or lowercase when naming schema elements in SQL statements, we can see in the below query that both the table City and its attributes are in lowercase (except for one that’s written exactly as it was declared, with the first letter capitalized). If we run this query, it will work the same as if we use C. to reference the attributes, since using only one table means there’s no ambiguity when referring to the table's columns.

SELECT *
FROM city C
WHERE (population > 20000 AND C.Latitude >=0) OR C.longitude <= 0;
Ultimately, with these conditions, we get all the tuples from City that have a population greater than 20,000 and a positive latitude, or those that simply have a negative longitude.

Let’s look at a similar example: here, we get all cruise bookings with a price below 500, an even cabin number, and a payment method of cash. In this case, we can see how we can apply different types of operators to build the condition.

SELECT *
FROM cruiseBooking CB 
WHERE CB.price < 500 AND MOD(CB.cabinnumber, 2)=0 AND CB.paymentmethod='cash';
On one hand, if we want to declare that all the conditions we impose must be met, we’ll use the logical operator AND. This performs a logical conjunction of those conditions so that the selected tuple is added to the resulting table only when all of them are met at the same time.

In other words, we can see the WHERE clause as a logical function that runs once for each tuple present in the table we want to filter. So if the result of that logical function is TRUE, then the tuple meets the conditions. Otherwise, it’s discarded and not included in the result table of the query.

So now we know that all the conditions we can define in a WHERE clause must be composed of a sequence of simpler logical conditions like “CB.price < 500“ joined by logical operators. Also, in each of these simpler conditions, we can find more logical operators, as they’re conditions that we can see as logical functions, which can themselves be composed of a sequence of even simpler conditions joined by logical operators. This allows for recursion, enabling us to use parentheses like in (C1 AND (C2 OR C3)) to adjust the priority and precedence of these operators at different levels of recursion in our condition (just like in other programming languages).

On the other hand, we can also encounter conditions where arithmetic or comparison operators are used, such as in this case when checking if the string containing the payment method is exactly the value ‘cash‘.

While in other languages we might write CB.paymentmethod='cash', in SQL we write the comparison operator with a single character =. If we want to negate it, we can do this either by using the logical operator NOT (affecting the entire equality condition) or by using CB.paymentmethod<>'cash' which represents the condition where it checks that the payment method is not ‘cash‘, meaning it’s different from that value.

In addition to these operators, we also have a series of mathematical functions available. For example, to check if a number is even or odd, in most general-purpose programming languages we have the modulo operator % which calculates the remainder of dividing the number by 2 – so if it’s 0, the number is even.

But in SQL, these operations aren’t implemented by default with arithmetic operators, but rather with functions. Specifically, to calculate the modulo, we use MOD(Dividend, Divisor), although there are many other similar functions.

We can use some of the operators mentioned earlier to perform calculations using entire columns. This results in other columns containing the results of those operations.

SELECT *, (CURRENT_DATE - CB.bookingDate) AS DateDifference1, ABS(CB.ArrivalDateFK-CB.DepartureDateFK)
FROM cruiseBooking CB;
For example, in this query, we want to calculate several date differences, one being the number of days between the booking date and the current date, and another being the number of days between the departure and arrival dates of the cruise trip.

To do this for each tuple in the CruiseBooking table, the simplest way is to add several columns that take the results of these calculations as their values. Specifically, we create these columns in the SELECT statement. This selects the corresponding attributes from the resulting table of the query and displays them to the user. Only those attributes are visible to the user, even though we got them from a table with more attributes.

But, besides selecting attributes, we can also define new columns that didn't exist in the table we’re selecting from. For example, in this query, using the notation *, we select all the attributes present in the table from the FROM statement, which in this case is CruiseBooking.

In addition to those, we concatenate more attributes with a comma, like DateDifference1 or the difference between the departure and arrival dates of the corresponding trip. If we look at the result of the query after adding these additional attributes, we’ll see a new column in the resulting table called DateDifference1, which will take as values the difference between the current date gotten with CURRENT_DATE and the booking date, which is CB.bookingDate.

So we see that in the SELECT statement, we can perform operations with the values of the tuples to generate new columns with intermediate calculations, or simply calculations required by the query, as in this case.

Specifically, the operation performed on each tuple to generate the value of the new column is defined in the SELECT statement itself. In this case, with CURRENT_DATE - CB.bookingDate, we define that the value of each tuple equals the current date minus the booking date. By default in SQL this returns the difference in days between the two dates.

Then to get the difference between the departure and arrival dates of the cruise trip, we use the values of the DepartureDateFK and ArrivalDateFK attributes from the foreign key pointing to Voyage. This avoids having to query data from other tables that contain them.

If we simply subtract them, depending on the order, we could get negative results, since one date is earlier than the other. So if we just want the absolute difference, we can wrap the operation with the ABS() function. And if we don't assign a specific name to that additional column, SQL by default assigns it the name “abs“. But we’ll want to change it sooner or later to avoid ambiguity problems if we use the ABS() function again to create another new column.

In the previous query, we saw that all the information we needed was present in the CruiseBooking table from the FROM clause – but this is not always the case.

For example, in the below query, we want to do a few things: first, we want to get all the bookings made by people whose names start with a letter that is later or equal to ‘L’. They should also meet a series of conditions like the ones we saw before. Finally, we want to calculate the difference in days between the current date and the booking date as we saw before.

SELECT *, (CURRENT_DATE - CB.bookingDate) AS DateDifferenceColumn 
FROM cruiseBooking CB INNER JOIN Person P ON CB.PersonFK = P.PersonID
WHERE CB.price < 2000
    AND MOD(CB.cabinnumber, 2) = 0
    AND CB.paymentmethod = 'cash'
    AND CB.bookingDate BETWEEN '2025-01-01' AND CURRENT_DATE
    AND P.Name > 'L';
For this, if we only use the CruiseBooking table in the FROM clause, we won't be able to access the name of the person who made the booking, as that’s an attribute of the Person table. We can get information from that table using the foreign key PersonFK from CruiseBooking. So to use the Name attribute from the Person in our query, we need to somehow "concatenate" or join the columns of the Person table with the information from the CruiseBooking table that we had before.

In SQL, the JOIN operation allows us to do this. We just need to choose a type and conditions that let us obtain only the tuples with the information we want.

Among all the types of JOINs, the least likely to be used in production or in complex queries is the implicit join. When we use an implicit join, we are performing a Cartesian product between all the tuples involved in that JOIN. So if we want to keep only certain tuples from that Cartesian product, we have to use a WHERE clause to impose certain conditions on the attributes.

Implicit joins are harder to read and maintain. In large or complex queries, we need to separate the join itself from the conditions on the Cartesian product. That means the logic is split between the FROM list and the WHERE clause, so you have more places to check when you modify or refactor the query.

Also, in implicit JOINs, we can’t perform operations equivalent to an OUTER JOIN because there’s no way to fill certain attributes with NULL if they’re not referenced in the other table of the JOIN (among other disadvantages). So the type of JOIN we choose will depend on the condition we need to impose on the tuples of the Cartesian product.

Just keep in mind that there are certain cases where it might be convenient to use implicit joins, such as in queries involving very few tables (at most 2 to keep the code as simple as possible) with simple restrictions, or when maintaining legacy code, meaning old or inherited code that uses implicit joins.

In this case, when performing the Cartesian product, we’ll get a series of tuples that combine all those from CruiseBooking and Person. This will result in tuples with information about these two tables where the person's information does not correspond with the person referenced by the foreign key of the CruiseBooking tuple.

For that reason, we don't need those tuples from the Cartesian product – or in other words, we want to get all those where the foreign key PersonFK of CruiseBooking points to the person whose information is indeed in that same tuple of the Cartesian product.

Formally, we express this condition as CB.PersonFK = P.PersonID. In this case, we need to assign alias names to the tables to differentiate their attributes and resolve possible ambiguity issues. So the most suitable type of JOIN for this query is an INNER JOIN, as it allows us to declare this equality condition exactly as we have written it here in an ON clause, as seen above.

In this way, by using a specific type of JOIN that’s not implicit, we can isolate all the filtering conditions of the tuples in the WHERE clause (dedicating the FROM to obtaining the data). Through the JOIN, we can concatenate the attributes of other tables to the resulting table of the query, and apply a specific filter to the tuples of the Cartesian product of that operation with an equality condition.

Regarding the WHERE conditions in this query, we’ve added one that makes sure the booking date is between '2025-01-01' and the current date we get with CURRENT_DATE. We could’ve used the arithmetic operators <= and <= for this, but SQL offers us a more convenient alternative using BETWEEN, where we define that the date of the bookingDate attribute must be between '2025-01-01' and CURRENT_DATE, both included.

The BETWEEN operator would also work to check if a string is between a pair of strings, all compared alphabetically. In this query, the only condition we impose on the lexicographical order of a string is P.Name > 'L'. This ensures that the name of the person who made the booking starts with a letter greater than or equal to L. (If their name is composed of text that starts with L followed by more letters, that text will automatically be considered strictly greater than the text 'L'.)

If we wanted to keep only the people whose names start strictly with a letter greater than L, we would have to use the condition P.Name > 'M'.

What if we need to get a list of all the people in the database, their information, and also the details of all the cruise bookings they have made? We’d need a list where all the registered people in the database appear.

SELECT *
FROM cruiseBooking CB RIGHT JOIN Person P ON CB.PersonFK = P.PersonID
ORDER BY P.PersonID;
For example, if someone has made 2 bookings, there will be 2 rows with their information plus the details of the two bookings they made. Meanwhile, people who have never made a booking will appear in the list with a row containing their information and a series of NULL values in the columns where the booking information would be.

This query isn’t common in real cases, but this structure can be useful for solving other types of queries. So to build this list, the first operator we might think of is an OUTER JOIN. In this type of join, we specify the side of the table whose rows should always appear in the final list, filling in with nulls in the other table when necessary.

To understand this, in this example, we see that a person doesn’t have to have any associated booking – so for each person, there doesn't necessarily have to be a booking in their name. So there may be some people who don’t have any bookings associated with them. So when we’re trying to do an INNER JOIN with the CruiseBooking table, they won't appear in the resulting table from the query.

That's why, instead of an INNER JOIN where we impose a strict condition that all tuples from the operation must meet, we use an OUTER JOIN. So, if we want all people to appear in the list even if they haven't made any bookings, we need to specify the side of the OUTER JOIN where we placed the Person table in the JOIN operation.

In this case, the Person table is on the right side, meaning its attributes are concatenated to the right of those in the CruiseBooking table. So in the OUTER JOIN, we must specify the RIGHT side so that all tuples from the table on the right side appear in the list, and for those people who don't have any associated bookings, their corresponding tuple will be filled with NULL values in the respective attributes that hold booking information.

If we had placed the Person table on the left side, then to achieve the same result as the previous query but with the columns of both tables reordered, we just need to change RIGHT to LEFT in the JOIN operation. This way, all tuples from the table on the left (meaning Person) must appear in the resulting table. The right side gets filled in with NULL values in this case, since that's where the attributes of the CruiseBooking table are.

SELECT *
FROM Person P LEFT JOIN cruiseBooking CB ON CB.PersonFK = P.PersonID
ORDER BY P.PersonID;
On the other hand, in both queries, you can see that we used ON to define the equality condition on the tuples of the Cartesian product produced by JOIN. We have to do this because if we use USING instead of ON, both attributes on which we want to impose the equality condition must be named exactly the same – so we can’t use USING here.

Aside from the JOIN operation from which the data is extracted, we often need to return the result sorted by an attribute. It may also simply be useful to have the result sorted so we can make checks more quickly, as in this case.

SELECT P.Birth
FROM Person P LEFT JOIN cruiseBooking CB ON CB.PersonFK = P.PersonID
ORDER BY P.PersonID;
To do this, at the end of the query, we can add an ORDER BY statement, which sorts the tuples of the resulting table according to the PersonID attribute of the person. This attribute doesn’t need to appear in the SELECT, as we might need other attributes that aren’t the ones defining the order, as shown above.

To finish with this type of JOIN, besides defining one side as RIGHT or LEFT, in an OUTER JOIN we might also need all the tuples from both sides' tables to appear. In the query below, for example, we need to get a list of all driving license applications, so that all of them appear, one in each tuple, with all the information regarding their rejection or acceptance.

SELECT *
FROM DrivingLicense D FULL OUTER JOIN RejectedDrivingLicense R USING (LicenseID);
To resolve this query, first, keep in mind that the schema constraints prevent us from having a driving license application both accepted and rejected at the same time. So for each application registered in the database, there will be either a tuple in RejectedDrivingLicense or in DrivingLicense, depending on whether it has been rejected or not. So when obtaining the query list, if the resulting table contains all the attributes from both tables, there will always be NULLs in some of them (either in RejectedDrivingLicense or in DrivingLicense).

To make sure that all applications appear, we can perform a FULL OUTER JOIN, where the OUTER specification is optional as we have seen on other occasions. This forces the tuples from both tables to appear in the final result, filling with NULL on the corresponding side for each tuple.

For example, if a license is accepted and we try to find it in the RejectedDrivingLicense table, it clearly won't be there. So, if we did an INNER JOIN, we wouldn't get a tuple for that application, which happens similarly with rejected applications and the DrivingLicense table. So with a FULL OUTER JOIN, we ensure that all applications appear, filling with NULL in RejectedDrivingLicense when the application is accepted and in the other table when it’s rejected. In this case, its also possible to use USING in the JOIN, since the equality condition is based on attributes in different tables that have exactly the same name.

Another JOIN we might encounter in real queries is the NATURAL JOIN, which is very similar to the INNER JOIN but with simpler syntax.

SELECT PersonFK, RequestDate, Fee, ApprovalDate, Points
FROM DrivingLicenseRequest NATURAL JOIN DrivingLicense;
For example, you can see in the example above a query that can help us verify that the schema's integrity constraints are met. In it, we get a list of all driving license requests that have been approved.

To do this, we perform a NATURAL JOIN between the DrivingLicense table and its superclass DrivingLicenseRequest. Since the only attributes with equivalent names are LicenseID, SQL automatically imposes the condition that the tuple with information from both tables has the same values in the LicenseID attributes of both tables, removing both attributes from the resulting table of the query.

This automatically imposed condition, as well as removing the attributes, is what characterizes the NATURAL JOIN. It’s often be preferable to an INNER JOIN because of these characteristics. By eliminating identical attributes, we eep the information that actually represents the people in those tuples. We can then use it to calculate various metrics or even as the result of a subquery in a more general query.

In this specific case, since all accepted requests have to be recorded in the DrivingLicenseRequest table, this query should return all tuples from DrivingLicense. But if any aren’t recorded in DrivingLicenseRequest, the foreign key won’t reference any valid tuple in DrivingLicenseRequest, revealing a database integrity issue.

Fortunately, we never have to manually check this situation with these queries, as the DBMS automatically verifies that all integrity constraints are met with each database modification, especially those related to keys.

In real queries, multiple JOIN operations are usually used in the same FROM statement because we need to gather data from multiple tables (or even from within the same table).

SELECT *
FROM Person P
    INNER JOIN Residence R1 ON (P.PersonID = R1.PersonFK)
    INNER JOIN Residence R2 ON (
        P.PersonID = R2.PersonFK
        AND R1.CityFK <> R2.CityFK
    )
ORDER BY P.personID;
For example, say we want to find people who have lived in several different cities at some point in their lives, regardless of when they did so. Since our schema lets people live in multiple cities at once, we’ll have to use several JOIN operations to gather data from Person and Residence and join them.

But given the condition we impose on the people, to know if someone has lived in more than one city, we need to check the Residence table and see if there are multiple Residence tuples for the same person with different cities.

Specifically, the query we want to make should get all those people who have lived in at least two different cities. If we only impose the condition that a person appears in at least two tuples of the Residence table, we’d get people who have had at least two residences – not those who have lived in different cities in those residences.

Therefore, the final condition ends up being that the person appears in at least two tuples of Residence where the associated city they have lived in is different in both tuples. Also, by checking this condition, we aren’t ensuring that the person only has those two tuples – we just need to know if they appear in at least two tuples with the previous characteristics (as a person may have had many residences).

To implement this query, we might first think of using set operations and subqueries – but there is a way to solve it using only JOIN operations.

When we do a JOIN between two tables, we are really doing the Cartesian product, from which we only keep some tuples that meet certain conditions. For example, when doing a JOIN between Person and Residence, the foreign key PersonFK in Residence must refer to the person from that same tuple in the Cartesian product. This means it must match the PersonID attribute from the Person table. With this, we can see that we obtain all the residences each person has or has had.

Then, from all of them, if we want to check that there are at least two with different foreign key CityFK values (meaning that there are two residences in different cities), we can do another JOIN of the intermediate table resulting from the previous JOIN with the Residence table.

This way, in addition to saying that its foreign key PersonFK has to refer to the corresponding person from each tuple resulting from the JOIN, we’re also declaring that the city it refers to must be different from the city referenced by the previous Residence table used in the previous JOIN.

To understand this in a more programmatic way, when doing a JOIN between Residence and itself, we’re getting tuples that represent pairs of residences. So we’re obtaining a series of tuples that together represent the Cartesian product between the tuples of the Residence table with themselves.

In other words, we end up with a series of tuples where, in each one, we can find information from exactly 2 tuples of the Residence table, for each possible pair of Residence tuples (including cases where both tuples are the same). If we add the restriction that these pairs must refer to a certain person, then they will be all the possible pairs of residences that a person has had.

Then if we also add the condition that for each pair of residences the cities they refer to must be different, we‘ll end up with tuples where the person who has had those residences have lived in at least two different cities. This doesn’t ensure that it’s exactly two, as they may have lived in many more (which we can see in the resulting tuples from these JOIN operations).

When implementing this in SQL, we see that in both ON clauses, we declare the condition that the Residence tuples must refer to the same person of the tuple we want to construct – with that person and a pair of their residences. Also, in the second JOIN, we declare the condition that the cities of the pair of residences must be different using the operator <>. Finally, we order the result according to the values of the PersonID attribute.

SELECT DISTINCT P.Name
FROM Person P
    INNER JOIN Residence R1 ON (P.PersonID = R1.PersonFK)
    INNER JOIN Residence R2 ON (
        P.PersonID = R2.PersonFK
        AND R1.CityFK <> R2.CityFK
    )
ORDER BY P.personID; /*Error*/
As you can see from the query result, there are people who have had many residences, resulting in many pairs of residences that meet the imposed conditions. This creates multiple tuples in the resulting table where the same person's information appears.

So, if we only want to get the person's name, we can replace * with P.Name in the SELECT statement to select only that attribute. To avoid duplicate values, we can use DISTINCT. Without DISTINCT, the same person's name may appear multiple times, depending on the number of residence pairs they have had in different cities. This also happens because SQL by default models tables with multisets, allowing such duplicates.

If we care about removing duplicates, we should use DISTINCT – but this decision can affect other statements like ORDER BY. In this example, we’re ordering by the values of the PersonID attribute, which we don't need in the resulting table where only the Name attribute appears.

Since PersonID doesn’t appear in the SELECT after using DISTINCT, the DBMS will give us an error. We have several options to fix it.

On one hand, we can remove DISTINCT, which will result in duplicate person data but that’s ordered by their PersonID (even though it won't be shown in the result).

On the other hand, we can keep DISTINCT and remove ORDER BY, because if the attribute we are ordering by does not appear in the SELECT after using DISTINCT, we will get an error that will prevent us from executing the query.

Another alternative we have is to show all the information about the person, not just the name. This way, we can order the result by the PersonID attribute and remove duplicate people. Instead of writing the entire list of attributes from the Person table in the SELECT, we can use the notation P.* to refer to all the attributes of the table with alias P.

SELECT DISTINCT P.*
FROM Person P
    INNER JOIN Residence R1 ON (P.PersonID = R1.PersonFK)
    INNER JOIN Residence R2 ON (
        P.PersonID = R2.PersonFK
        AND R1.CityFK <> R2.CityFK
    )
ORDER BY P.personID;
Finally, in SQL, it's common to encounter queries where we need to work with dates. For example, in our schema, we might have a query to get all the people who were born in May.

SELECT *, EXTRACT(MONTH FROM Birth) AS BirthMonth
FROM Person
WHERE EXTRACT(MONTH FROM Birth) = 5;
We can solve this by imposing a single condition on the birth date, with the peculiarity that we can't treat the data type exactly as if it were entirely numeric or text. Instead, we need to extract characteristics from the date to operate with.

In this case, the clearest characteristic to obtain is the month. By using the EXTRACT() function and the MONTH characteristic, we extract the month number from the Birth attribute's date to check if it’s May or not.

Note that the function generally returns numbers for day, month, year, and so on, not strings. So we treat the month as if it were a number from 1 to 12.

We can convert between number and string using other SQL tools, all in the appropriate format according to the time zone and geographic area. Then, if we want that date characteristic to appear as an additional attribute in the resulting table, we simply treat the EXTRACT() function as if it were any SQL function that returns a value when given certain values from a tuple.

But even if we assign it an alias, we can’t use that alias in the WHERE clause to declare the condition that it equals to be 5. Instead, we must write the entire calculation in the WHERE clause. Although this may seem inefficient in terms of readability, without using additional Common Table Eexpression (CTE) techniques like those we will see later, we have no choice but to duplicate the attribute calculation in the WHERE clause if we want to impose a condition on it.

SELECT *, EXTRACT(WEEK FROM Birth)
FROM Person;
In addition to the day, month, and year, the EXTRACT() function allows us to obtain all kinds of characteristics from a date, like the week number with WEEK as shown above, or the current quarter number with QUARTER.

Subqueries
There are some SQL queries that require subqueries. A subquery is simply a query inside another query. It helps you solve a smaller problem so the main query can solve a bigger one.

Let’s dive in a little deeper. When you run a query in SQL, you get a result table (a multiset, since rows can repeat). A subquery lets the outer query use that result – for example, to check membership or existence.

SELECT *
FROM Person P
WHERE P.PersonID IN (SELECT PersonFK FROM Residence);
This returns every person whose identifier appears in Residence.PersonFK – that is, everyone who has (or had) a recorded residence. The subquery produces the set of referenced person IDs, while the outer query keeps rows where p.PersonID is in that set.

Note that this is a non-correlated subquery (it doesn’t reference the outer query), which many databases may materialize once or rewrite as a semi-join before applying the IN filter. In practice, this is usually comparable to an equivalent EXISTS or JOIN-based formulation. We’ll just choose the form that’s clearest and add appropriate indexes (for example, Residence(PersonFK), Person(PersonID)) for speed.

If the subquery can return NULL, IN uses three-valued logic. With a foreign key on Residence.PersonFK, NULL values are typically disallowed, so this isn’t an issue.

On the other hand, we can solve the query using JOIN operations as shown below:

SELECT DISTINCT P.*
FROM Person P INNER JOIN Residence R ON R.PersonFK = P.PersonID;
Here, we combine the data from Person and Residence using the equality condition that requires the foreign key of Residence to reference the person in the same tuple of the Cartesian product. This way, we only get those tuples that have the information of a residence and the person associated with it.

Then, to keep only the data of the people, we use P.* as before – but here we need to use DISTINCT, since a person may have multiple residences. Specifying DISTINCT prevents this from duplicating the data of the same person.

The JOIN operation is often considered inefficient because it’s a Cartesian product that must construct all tuples of that product and then filter them using the conditions we declare. But we can make it faster with the right hardware, like GPUs.

Still here, we need to remove duplicates with DISTINCT, which involves additional processing of the query result. We also need another filter or process that eliminates duplicate tuples, so it seems less efficient at first glance.

But depending on how the DBMS implements these operations at a physical level, it can be more or less efficient than using subqueries (as the hardware also makes a difference).

Here’s another construction based on subqueries that we can use to solve the previous query. As you can see, we build a correlated subquery where we use the PersonID attribute from the "higher-level" query to get all the residences (tuples) from the Residence table that belong to the person indicated by the PersonID identifier. In other words, since the WHERE clause is executed for each tuple of Person, we can construct a subquery where, given a certain person with that identifier, we can get all the residences registered in their name. That would be those whose foreign key PersonFK refers to the PersonID identifier of the person.

SELECT *
FROM Person P
WHERE EXISTS (
        SELECT *
        FROM Residence R
        WHERE R.PersonFK = P.PersonID
    );
With this correlated subquery, SQL must build its result for each person in the Person table, as the result depends on the specific person being processed. So to only keep those people who have a residence, we use the EXISTS operator to verify that the resulting multiset of the subquery contains at least one tuple (indicating that the person has a residence).

SQL has to go through the Residence table for each person in the Person table, although it only goes through Residence until it finds the first tuple whose foreign key points to the corresponding person. This avoids unnecessary checks of the rest of the tuples in Residence because EXISTS only requires at least one tuple in the subquery.

Still, in the worst-case scenario, it would have to go through the entire table for each person if no person has or has had residences.

Another way we can use membership or existence operators is on a list of values. This is declared very similarly to a tuple and a subquery but is not necessarily a tuple or a subquery.

SELECT *
FROM Pool P
WHERE Status IN ('closed', 'renovation')
    AND mindepth IN (
        SELECT mindepth
        FROM Pool
        WHERE mindepth > 4
    );
For example, above we have a query that returns all pools whose status is ‘closed’ or ‘renovation’ and whose minimum depth is greater than 4.

To check the first condition, we could easily use the logical OR operator and declare two simpler conditions to check whether the Status value is either ‘closed’ or ‘renovation’. But we can do this more simply using the IN operator. So by using the notation ('closed', 'renovation'), we declare a list with those two values, checking with IN if the value contained in the Status attribute is in the list or not. This has the same effect as using the OR operator, but with clearer syntax and similar efficiency.

This check we do with IN is like a membership check on the result of a subquery, as the syntax is very similar. But don’t confuse the list declaration with a subquery, since ('closed', 'renovation') doesn’t represent a multiset with tuples, but rather a list of values. We can also view it as if it were a column on which we perform a check.

On the other hand, the simplest way to check if the pool's minimum depth is greater than 4 is with the condition mindepth > 4 directly. But to show an equivalent way of checking with subqueries, you can see above that the subquery for the condition retrieves all mindepth values from the Pool table that are strictly greater than 4. Then it uses IN to check if the mindepth value from the outer query's Pool table is in the subquery's result.

So instead of writing mindepth > 4 directly, the subquery first selects all mindepth values greater than 4, and the outer query uses IN to keep a pool row only if its mindepth is in that set. In practice, although this can also be a solution to the query, we should keep the code as simple as possible. We generally avoid these techniques.

Also, we don’t need alias P. to refer to the mindepth of the outer query – as it’s the only one called that way in this query. But if we had to use it in the subquery, we’d need to use the alias P. to distinguish it from the mindepth attribute of the Pool table in the subquery. (This also doesn’t need an alias because it’s a simple subquery without another subquery inside it. This is possible to do, and sometimes even necessary.)

Here’s another equivalent way to solve the query using subqueries:

SELECT *
FROM Pool P
WHERE Status IN ('closed', 'renovation')
    AND P.mindepth > ALL (
        SELECT mindepth
        FROM Pool
        WHERE mindepth <= 4
    );
The main difference is that here, the subquery gets all the mindepth values that are <=4, which is the opposite condition of what we want the tuples to meet. So in the outer query, we have the result of this subquery, which includes all the mindepth values we’re not interested in.

To check if a tuple meets the condition of having a minimum depth >4 using these values, we use the \> ALL operator to verify if the mindepth of the tuple we are checking is strictly greater than all the values present in the subquery.

This equivalent way of solving the query is more elaborate than the simplest and most efficient solution, which is to use the mindepth>4 condition directly. This is simply an example to demonstrate that there's often more than one way to get the same result for any state of the database. This is the definition of equivalent queries.

Also, in many situations, it’s useful to use operators like ANY, IN, ALL, EXISTS, and so on in combination with other arithmetic operators on a subquery to define conditions that certain tuples must meet, as shown in these examples.

So far, we’ve seen queries that use subqueries in their implementation, but those subqueries essentially behave as if they were queries themselves. This means we can execute them directly on the DBMS as if they were regular queries. So nothing prevents a subquery from being made up of subqueries at a "lower" level, meaning subqueries that are at a nesting level below the other subquery, which in turn is at a lower nesting level than the query it’s in.

Basically, SQL allows us to chain as many subqueries as we want within a query or subquery. This helps us solve problems like the query below, which retrieves a list with information on all the people who don’t have a valid driver's license:

SELECT *
FROM Person P
WHERE NOT EXISTS (
        SELECT *
        FROM DrivingLicense D
        WHERE D.LicenseID IN (
                SELECT LicenseID
                FROM DrivingLicenseRequest R
                WHERE R.PersonFK = P.PersonID
            )
    );
We could approach this query so that it'd require JOIN operations to solve it. But in this case, it’s structured in a "nested" manner at the subquery level so that it requires the use of subqueries.

So to get this list, we first go through all the people in the Person table. For each one, we check that there is no driver's license whose associated request was created by that person. We can implement this condition by applying the NOT EXISTS operator to a subquery that returns all valid driver's licenses associated with a person. We get these by filtering DrivingLicense to licenses whose matching DrivingLicenseRequest row has PersonFK = P.PersonID – that is, licenses requested by the current person.

Regarding this last point, as you can see in the code, the simplest way to implement it with subqueries is to check that the LicenseID of the valid driver's license exists in the set of LicenseID values from the requests in the DrivingLicenseRequest table whose foreign key points to the person being iterated over in Person. That makes this subquery correlated with the outer query we are making, as it includes the attribute P.PersonID.

In short, we’ve implemented this query by nesting subqueries, where SQL allows us to reach an arbitrary level of nesting according to the needs of the query. But we could’ve done it in other ways like using JOIN operations, which in certain situations are easier to understand than the approach we just followed.

Just remember that nesting queries is not always the best way to solve a problem, especially when multiple levels of nesting are created (whether correlated or not with each other). We’re just showing what’s possible here. It’s only worthwhile when it improves the efficiency or clarity of the query sufficiently compared to other alternatives.

Let’s talk about where or in which statement subqueries can be nested. In the below code, you can see how the subquery is nested in the FROM clause.

SELECT P.Name
FROM Person P
    INNER JOIN (
        SELECT DISTINCT PersonFK
        FROM Rental
    ) R ON R.PersonFK = P.PersonID;
Since it returns a table with tuples, we’ll often use that query result in a FROM clause to get the information from the tuples and return it to the user through a SELECT. Or we could even combine it with another table using a JOIN operation, as in this case. Specifically, this query will get information about all the people who have rented a bike at some point at least one.

So the approach we follow to resolve the query is to perform a JOIN between the Person table (that contains all the people in the system) and a table that has the identifiers of the people pointed to by the foreign key {PersonFK} of any tuple in Rental. This means anyone whose identifier is referenced by any tuple in Rental, implying that they’ve rented a bike at least once.

We can construct this list of person identifiers using a subquery that extracts all the PersonFK values from the Rental table while removing duplicates. A person may have made an arbitrary number of rentals throughout their history, but we’re interested in whether they have made at least one. So, we simply need to know if they appear in the list of PersonFK values.

Then, using an INNER JOIN, we combine the information of PersonFK returned by the subquery with the tuples from the Person table. This gives us all the information of the people identified by PersonFK, which in turn points to PersonID. But since we want, for example, the names of the people and not just their identifiers, both the JOIN and the subquery are essential, because if we only needed the identifier, it would be enough to return what the subquery provides.

In addition to nesting subqueries in the FROM clause, we can also do it in the SELECT clause, where the main goal is to calculate a metric or get more information for each tuple in the query. That is, if in the SELECT we get attributes P.PersonID and P.Name from each of the tuples returned to the user, we might want to get more information beyond these two attributes that needs to be calculated with a query. In this case, this query will be nested as a subquery in the SELECT, and it’s result will be the value added to the additional attribute representing the subquery in the SELECT.

SELECT P.PersonID,
    P.Name,
    (
        SELECT COUNT(*)
        FROM Residence R
        WHERE R.PersonFK = P.PersonID
    ) AS NumResidences
FROM Person P;
In these cases where the subquery is nested in the SELECT statement, the subquery must meet a basic requirement: it has to return at most one tuple and one column. This is because the result of the subquery will be added in a new additional column (and only one) in our SELECT. Then we’ll calculate its result and add it in each tuple of the outer query – so the subquery can’t return more than one tuple.

For example, in this query, we want to list all the people in the database along with a column that contains the number of residences they have had. To solve this, the simplest approach is to go through all the tuples of Person and, for each one, count how many tuples of Residence have their foreign key PersonFK referencing that person.

Going through the tuples of Person is simple: we just use a combination of SELECT and FROM. But in order to count how many tuples of Residence meet this condition for each person, we need a correlated subquery – specifically with the person being processed. We can uniquely identify this with P.PersonID.

We need to do this because to count tuples in Residence, we have to compare the values of their foreign key PersonFK with the identifier P.PersonID. To get the value of this count, we can use a subquery: the aggregation function COUNT(*) lets us count all the tuples present in Residence. It does this after filtering them with the condition that their foreign key PersonFK references the person being processed in the Person table.

It’s important to note that the subquery will only return one value generated by COUNT(), and only one column generated by this function. This meets the requirement that every subquery used in the SELECT statement must fulfill.

Finally, it’s worth mentioning that this value generated in the subquery populates an additional column which we’ve added by including the subquery itself in the SELECT for each tuple of our query. In other words, each tuple will need a value for this new column, which they’ll get by executing the correlated subquery on that specific tuple.

SELECT and FROM aren’t the only statements where subqueries are allowed. We can also use them in a WHERE, HAVING, or even ORDER BY clause. More importantly, a query can have an arbitrary number of subqueries (nested or not) depending on its needs.

SELECT P.PoolID,
    P.Name AS PoolName,
    C.Name AS CityName,
    P.Status,
    (
        SELECT PoolID
        FROM CityPool C
        WHERE C.PoolID = P.PoolID
    ) AS CityPoolID
FROM (
        SELECT *
        FROM Pool
        WHERE Status = 'maintenance'
    ) AS P
    JOIN City C ON P.CityFK = C.CityID;
For example, in a query like the one above, we can see that there is not only a subquery in the SELECT but also one in the FROM.

In this specific case, the query gets information about all the pools currently under maintenance, including details about the city the pools are located in (such as its name). There’s also an additional column indicating the pool's identifier if it’s of the CityPool type, leaving it blank if it’s not.

So to resolve this query, we first need to get information about the pools under maintenance. This simply involves going through the tuples in the Pool table and selecting those whose Status value is ‘maintenance’.

Then, to gather information about the city where each pool is located (along with the Pool tuples we just obtained), we can use a JOIN that operates on the previous tuples and the City table. This is why we’re extracting all Pool tuples using a subquery.

So although the type of JOIN is not explicitly specified, by using the ON clause SQL automatically interprets it as an INNER JOIN (it would also be interpreted as INNER type if we had used the USING clause). But this practice is not recommended, as in most situations where the JOIN type is omitted, the readability of the code is compromised, especially when there are many JOINs in the same query.

Here, in the ON clause, the JOIN condition states that in the same tuple of the Cartesian product, the foreign key CityFK – which represents the city where the pool is located – must have the same value as the CityID identifier of the city in the tuple.

Then, to attach the extra column with the pool identifier from CityPool for those tuples that represent pools of that type, respectively, we’ll use a subquery. This subquery searches the CityPool table for a tuple whose PoolID matches the PoolID from Pool. This checks if the pool from Pool is actually of the CityPool type or not.

In this way, the subquery will return the identifier value if it’s of the CityPool type – otherwise, it will return nothing, meaning it will return a table without tuples (or in other words, an empty set or multiset, rather).

This is allowed in SQL, but it can sometimes cause errors, so it's generally not a good practice to use subqueries in the SELECT that aren’t guaranteed to return at least some tuple.

So for those pools that aren’t of the CityPool type, the subquery will return nothing. This means that the value of the extra column in the SELECT will be NULL as we can see when executing the query.

Since it doesn’t return any tuple with any value, we’ll insert an unknown value. The way to represent this in SQL is with the special value NULL. Also, this extra column by default has no name, so we can assign it a recognizable alias using the AS clause as shown in the query.

On the other hand, if we want to avoid having NULL values in the additional column, we can have this column contain boolean values where TRUE indicates that the pool is of the CityPool type and FALSE that it’s not.

Starting from the same query as before, the only change we need to make to achieve this is to add an IS NOT NULL check. For each tuple, it checks whether the value inserted in the additional CityPoolType column is NULL or not. Thus, if its type is indeed CityPool, the value in the additional column provided by the original subquery won’t be NULL. This meets the IS NOT NULL condition and returns TRUE. Conversely, if it’s not of that type, IS NOT NULL won’t be met, and the additional column in this case will be filled with FALSE.

SELECT P.PoolID,
    P.Name AS PoolName,
    C.Name AS CityName,
    P.Status,
    (
        SELECT PoolID
        FROM CityPool C
        WHERE C.PoolID = P.PoolID
    ) IS NOT NULL AS CityPoolType
FROM (
        SELECT *
        FROM Pool
        WHERE Status = 'maintenance'
    ) AS P
    INNER JOIN City C ON P.CityFK = C.CityID;
Here, we need to be careful about where we place the IS NOT NULL condition. On one hand, we might think of comparing the PoolID attribute of the CityPool table itself in the SELECT clause of the subquery. If we do this, we’ll be comparing a value that may or may not exist with NULL, so the final result of the subquery will be FALSE if the pool is of the CityPool type.

But if it’s of another type, there won't even be a value for that PoolID attribute in the CityPool table, so the comparison with NULL won’t be executed. This will result in the final query output having the additional column contain NULL values for pools that aren’t of the CityPool type and FALSE for those that are of the corresponding type.

This happens because we shouldn’t compare PoolID with NULL, as its value may or may not exist. And if it doesn't exist, the check won't be executed for all the tuples in our query.

Instead, we should perform this check on the result of the entire subquery. It can be NULL when the pool is not of type CityPool – and so we see values in the additional column filled with NULL in the final result. Or it can contain a valid identifier different from NULL, which violates the IS NOT NULL condition.

In short, the check to ensure that the additional column is of boolean type should compare the result of the entire subquery (which is either NULL or a specific value) with the NULL value itself. This checks to see if each tuple in our resulting table matches or not.

In summary, although it's not good practice to use subqueries in the SELECT clause that may result in an empty set, we can do so long as it doesn't make the readability or efficiency of the query worse. We also need to have certain guarantees that it does what it’s expected to do.

So far, we've performed membership checks with IN, as well as checks with other operators. We’ve used individual attributes to verify if the value of a certain attribute was in a set formed by the values of an attribute, among other conditions. And sometimes we need these conditions to involve more than one attribute for verification.

SELECT E.EntryTimestamp, E.PersonFK, E.PoolFK
FROM Entry E
WHERE (E.EntryTimestamp, E.PersonFK, E.PoolFK) IN (
        SELECT PS.EntryFK, PS.PersonFK, PS.PoolFK
        FROM PoolSanction PS
    );
For example, above we have a query that retrieves all the tuples from Entry that have been sanctioned with some pool sanction from the PoolSanction table. To do this, we simply need to go through the tuples in Entry and, for each one, check if it has a sanction. In other words, we verify if there is a tuple in PoolSanction whose foreign key to Entry references the tuple we’re examining.

When doing this, the first thing we notice is that the primary key of Entry doesn’t consist of a single attribute, but rather 3. This is just like the foreign key in PoolSanction – it determines that the entry that has been sanctioned doesn’t have one attribute, but three.

So under normal conditions, we could use a subquery to get all the foreign key values from PoolSanction, then check if the identifier (primary key) of each entry belongs to that set of values using the IN operator. But here we can’t do it the same way because we need to work with three attributes instead of one.

That's why, in the subquery, instead of returning a single attribute, we return all those that make up the foreign key to Entry (these are (EntryFK, PersonFK, PoolFK)). With this, we have a set of tuples where each one refers to a tuple in Entry that has been sanctioned.

Specifically, each of these tuples in the set refers to the three attributes that make up the primary key of Entry, which are (EntryTimestamp, PersonFK, PoolFK). So to check if an entry belongs to this set, we simply go through it, looking to see if any of the tuples match exactly with the tuple of the entry's primary key (with all three attributes having equal values).

We do this using the IN operator, where instead of specifying a single attribute, we can specify an arbitrary number of them in parentheses. Thus, the IN operator will perform the same operation as in previous cases, taking the primary key (EntryTimestamp, PersonFK, PoolFK) of each entry and comparing it with each of the tuples from the subquery, attribute by attribute. If any of them match, then it belongs to the set, fulfilling the condition.

Here, it's very important to note that the tuples compared by IN must be the same size. This means that they need to have the same number of attributes, the same data type (or at least be comparable), and their semantics must be the same. That is, if for each tuple in Entry we use (EntryTimestamp, PersonFK, PoolFK) to check if that three-attribute tuple is in the subquery set, then that subquery must contain tuples of three attributes where:

the first one is EntryFK, which refers to the EntryTimestamp attribute of the primary key of Entry,

The second one is PersonFK, referring to PersonFK of the primary key,

and so on. This ensures that the comparison is semantically correct, even though in the DDL the primary key might have been defined in a completely different order.

Another variation of this query is to list all the sanctioned entries along with the information of the person who has the entry.

SELECT P.*, E.EntryTimestamp, E.PersonFK, E.PoolFK
FROM Person P INNER JOIN Entry E ON P.PersonID=E.PersonFK
WHERE (E.EntryTimestamp, E.PersonFK, E.PoolFK) IN (
        SELECT PS.EntryFK, PS.PersonFK, PS.PoolFK
        FROM PoolSanction PS
    );
For this, based on the previous solution where we got the list of sanctioned entries, the only additional step we need to take is to perform a JOIN between Entry and Person. In doing this, we only keep those tuples from the Cartesian product where the foreign key PersonFK from Entry refers to the primary key {PersonID} from the information coming from Person.

We can also see that the condition checking whether the entry is sanctioned or not is the same. With this example, we can more clearly see the purpose of the JOIN operation, which is to gather information from multiple tables. So for each sanctioned entry we had before, if we now need to concatenate the information of the person to whom the foreign key PersonFK points, we can simply perform the Cartesian product between both tables and impose a condition to ensure that the reference of PersonFK is indeed the person present in the tuple.

Continuing with the uses of this last technique, where we use operators like IN to check if a certain combination of attribute values belongs to a set of tuples, in the following example we have a query that lists all the trips from the Voyage table for which there is a return trip. That is, we need to find all trips going from city A to city B for which there is at least one other different trip going from B to A.

SELECT *
FROM Voyage V
WHERE (V.DepartureCityFK, V.ArrivalCityFK) IN (
        SELECT V2.ArrivalCityFK,
            V2.DepartureCityFK
        FROM Voyage V2
    )
ORDER BY (V.DepartureCityFK, V.ArrivalCityFK);
To do this, the first thing we need to realize is that the primary key of Voyage includes the attributes DepartureCityFK and ArrivalCityFK, which refer to the start and end cities of the trip, respectively. So if we have multiple trips with different values in these attributes, we’ll definitely know that both trips are different. This is because even if the rest of the primary key attributes were the same, as long as at least one of them is different, the trips must necessarily be different.

So we can formulate the query similarly to the previous ones, going through all the tuples in Voyage and for each one, checking if there is a trip whose start and end cities are the same the end and start cities of the trip being checked. So for each trip in Voyage, we construct a correlated subquery where we again go through all the tuples in Voyage and only get the values of the DepartureCityFK and ArrivalCityFK attributes. Then, we check if the values of these attributes from the trip in the "higher level" query are in the set of tuples we just built.

But in this case, if we look at the code, the order of the attributes is swapped compared to the order of those same attributes in the subquery. What we really want to check is that the value of the DepartureCityFK attribute of the tuple we are checking in the query matches the value of the ArrivalCityFK attribute of some tuple in the subquery. Also, we need to check that the value of the ArrivalCityFK attribute of the query's tuple matches the value of the DepartureCityFK attribute of the same tuple that matched the previous pair of attributes.

We can more easily understand this by viewing the pair (V.DepartureCityFK, V.ArrivalCityFK) as if they were the start and end cities, A and B, of a trip. What we want to check is if there is any tuple in the subquery that has B and A as the start and end cities, respectively.

The simplest way to make this check is either to reverse the order of the attributes in the tuple (V.DepartureCityFK, V.ArrivalCityFK) or in the attributes of the SELECT in the subquery. This is what we’ve decided to do here, which is why ArrivalCityFK is returned before DepartureCityFK.

Finally, to more easily check for the existence of these round trips, we can add the ORDER BY clause, which orders by multiple attributes instead of just one. That is, we use the attributes (V.DepartureCityFK, V.ArrivalCityFK) as the sorting criteria. SQL orders by the pairs of values for each tuple, as if each possible pair of values were considered a single value that could be compared with others.

By doing this, we can easily focus on the departure city of a trip and then look for another trip whose arrival city has the same value. Then we can find one whose departure city matches the arrival city of the original trip, thus finding a pair of trips that form a round trip to a city.

Finally, let’s look at another query where we need to compare values of multiple attributes at once. Here, all trips are listed whose associated cruise (the one making the trip) has been assigned to its cruise line on the same start date of the trip.

SELECT V.*
FROM Voyage V
WHERE (V.ShipFK, V.DepartureDate)
  IN (
    SELECT SA.ShipFK, SA.StartDate
    FROM ShipAssignment SA
  );
To implement this, we need to consider all the attributes that Voyage has, including the information contained in the foreign keys, to avoid having to perform unnecessary operations like a JOIN with the CruiseShip or CruiseLine table.

In this case, we structure the query similarly, first going through all the tuples of Voyage and checking if the cruise has been assigned to its cruise line on the same date as the start of the trip.

To make this check easier, we construct a subquery that returns all the values of the ShipFK and StartDate attributes from the ShipAssignment table. This way, later in our query we can check if the cruise making the trip (which is referenced with the foreign key ShipFK of Voyage) was assigned on the DepartureDate of the Voyage tuple (start date of the trip) to any cruise line.

As you can see, we can simplify the query if we think of it as getting all trips for a cruise ship that has been assigned a start date with any cruise line. In other words, it doesn't have to be a specific line, but any line to which it was assigned on the date indicated by DepartureDate of Voyage. So in the WHERE clause, it checks if the pair of values taken by the attributes (V.ShipFK, V.DepartureDate) are found in the subquery. And this time it maintains the correct order of the attributes, since ShipFK of Voyage must match ShipFK of ShipAssignment, and DepartureDate of Voyage must match StartDate of ShipAssignment, respectively.

On one hand, the match of ShipFK ensures that the cruise ship making the trip is the same as the one assigned in the ShipAssignment tuple. Likewise, the match of the date attributes ensures that this assignment was made on the start date of the trip.

We have also solved this query using a correlated subquery and the IN operator, although it's not the only way. As you can guess, there's always the option to use JOIN operations and conditions to filter the tuples, which can be more or less efficient in certain cases. This is why it's important to understand what SQL does under the hood, like whether it actually builds and stores all the tuples of a subquery or Cartesian product in memory, and when it does so.

Common Table Expressions
We have seen that subqueries allow us to use the result of one query within another query. We can construct this once during the execution of the entire query if it’s not correlated, or once for each tuple of the table with which it’s correlated. In other words, we can see a subquery as a set of tuples that we operate with in a query.

But we don't always need queries to be correlated. We’ve seen that some queries can be resolved by non-correlated queries, meaning sets of tuples that are constructed only once and are sufficient to resolve the entire query in which they are contained.

In these situations, to simplify notation, we can use a tool called CTE (Common Table Expression) in SQL. These typically use the WITH clause. With this, we can define and store the result of a subquery in a temporary table that needs an alias. So instead of using a subquery in the construction of a query, we define a temporary intermediate table (Common Table Expression) that only exists during the execution of the query and contains all the tuples generated by a certain subquery. Again, we need to use an alias to refer to it, just as we have to provide tables with a name and a schema when we create them in the DDL.

To understand the WITH clause with an example, we can consider the query that gets information about all currently active cruises. Here, active means assigned to a cruise line at the current date when the query is executed.

Before writing code, it's helpful to think about how the query will be structured, meaning where we’ll get the data to respond, how we should combine the different tables with that data, what conditions or operations need to be applied to them or the tuples resulting from the operations performed, and so on.

WITH ActiveShips AS (
    SELECT ShipFK
    FROM ShipAssignment
    WHERE StartDate <= CURRENT_DATE
        AND EndDate >= CURRENT_DATE
)
SELECT CS.ShipID, CS.Speed, CS.PassengerCapacity
FROM ActiveShips A INNER JOIN CruiseShip CS ON CS.ShipID = A.ShipFK;
In this case, the information for all cruise assignments, whether current or not, is in the ShipAssignment table. So, to know which cruises are currently assigned to a cruise line, we can take advantage of the fact that this table has a foreign key ShipFK that identifies the assigned cruise in each tuple of ShipAssignment.

So, if we set the condition that the StartDate of the assignment should be before the current date gotten from CURRENT_DATE, and that the EndDate is after the current date, we’ll get all those assignments that are valid on the current date. By extracting the values taken by the foreign key ShipFK for those assignments, we can identify the cruises that are currently assigned.

But the query not only asks us to identify them – but also to get information about them stored in CruiseShip. So, we save the identifiers of the cruises we got earlier in a temporary table to use in the query. In other words, we could make the conditions on StartDate and EndDate apply to ShipAssignment in a subquery. But to simplify the notation and demonstrate how to use CTEs, we’ll use the WITH clause where we define all the subquery code and assign an alias to that temporary table (see above code).

Specifically, by doing this, we’ll be saving the identifiers of the currently active cruises in the temporary table named ActiveShips. This is the alias we assigned using the AS operator – but it works in reverse in the WITH clause: first, you write the alias name and then you writethe code that gets the data from the intermediate table (the element to which the alias name is assigned).

So, when we use the WITH statement, we see that we have constructed an ActiveShips table with the result of what could be a non-correlated subquery – but for simplicity, we’ve refactored it so that its result is stored in an intermediate table with a certain alias.

Now, we can treat ActiveShips as if it were another table in the database, performing a JOIN between it and CruiseShip to get all the information about the active cruises. We impose an equality condition on the ShipFK and ShipID attributes of the ActiveShips and CruiseShip tables, respectively. This means we only keep those tuples from the Cartesian product where the foreign key ShipFK refers to the ShipID identifier of that same tuple. This allows us to find the complete information about a specific cruise.

In the previous query, we could have easily skipped using WITH and made ActiveShips a subquery to which we could’ve also assigned an alias. But when using a subquery, even if we assign it an alias, we can’t use it in just any part of the query. That is, if we have a subquery in a FROM or a SELECT, we can’t use it in other parts of the query in the same way as we can use an intermediate table defined in a WITH. This (WITH) we can reference at any point in the query, regardless of whether it’s formed by more subqueries.

WITH VoyageDistance AS (
  SELECT * 
  FROM Voyage
  WHERE Distance > 1000
)
SELECT DepartureDate, ArrivalDate, Distance
FROM VoyageDistance 
WHERE DepartureDate BETWEEN '2025-01-01' AND '2025-06-30';
We have another similar example in the query above. Here, we consider a query that gets information about all voyages that started in the first half of 2025 (approximately) and have a distance greater than 1000 kilometers. The approach in this case is simpler since all the information we need is found in the Voyage table. So the condition that the distance is greater than 1000 kilometers is easily modeled with a WHERE clause and the expression Distance > 1000.

Just like before, in this query we could also skip using WITH and include both the distance and the condition on the start date of the voyage in a single WHERE. But often we might need to modify or expand a query – for example, in the future we might be asked for a query based on this one, but with more or fewer conditions. So if we conducted an analysis of our domain, user requirements, and the query code, we might conclude that tuples with voyages over 1000 kilometers could be needed in multiple parts of the same query.

In this example, this phenomenon might not occur, but it illustrates that in a real situation, we may need to consider various factors that affect query design.

So, say we assume that the Voyage tuples with Distance>1000 could potentially be used multiple times in a single query across multiple statements (in future modifications of this query). Then the most maintainable option is to use a WITH clause where we temporarily store these tuples and then use them in the query through the alias of this intermediate table (as if it were a regular database table). Then, we can add another WHERE clause at the very end of the query, declaring the condition that the start date of the voyage is in the first half of 2025. We can model this with the BETWEEN operator, the EXTRACT() function, or many other ways.

Finally, it’s worth noting that using the WITH clause without a clear reason isn’t considered a good practice. (Examples of such a clear reason might include a design decision based on user requirements or a thorough analysis of the query that concludes that it might be useful to have an intermediate table like VoyageDistance in the future).

This is mainly because, in situations like this, a WHERE clause is being used both in the construction of the intermediate table and in the resulting table from the query. This means multiple filters might be applied internally, which can be inefficient.

But the DBMS often automatically applies certain techniques like inlining to optimize query execution through refactorizations of the execution plan. In other words, even if our code is not the most optimal, the DBMS can automatically find an equivalent and more optimal way to resolve the query.

To illustrate that the intermediate tables we define in the WITH clause can be constructed with subqueries as "complex" as we want, consider this query:

WITH Pending AS (
    SELECT D.*
    FROM DrivingLicenseRequest D
        LEFT JOIN DrivingLicense A USING (LicenseID)
        LEFT JOIN RejectedDrivingLicense R USING (LicenseID)
    WHERE A.LicenseID IS NULL
        AND R.LicenseID IS NULL
)
SELECT P.Name, Pending.*
FROM Pending INNER JOIN Person P ON P.PersonID = Pending.PersonFK;
We’re getting information on all driving license requests currently being processed, meaning those that have not yet been accepted or rejected. We’re also including information about the person who made each request.

In this case, the key point is to realize that the requests in process are represented by tuples in DrivingLicenseRequest that aren’t referenced by any tuple in either DrivingLicense or RejectedDrivingLicense (since they aren’t yet accepted or rejected).

In this case, we can use LEFT JOIN so that by combining all these tables with LEFT JOIN operations, we can gather complete information about the requests. This means constructing a table formed by all the attributes of the three tables in the hierarchy, where some of them will be NULL or not in each tuple, depending on whether they represent accepted, rejected, or pending requests.

Specifically, since the foreign keys of the inheriting entities in the hierarchy are both called LicenseID (matching the identifier {LicenseID} of the superclass), the LEFT OUTER JOINs are performed by applying an equality condition on this attribute. This ensures that the tuples we get contain information about the same request, rather than multiple requests in the same tuple of the Cartesian product.

We use LEFT JOIN because the first table we combine is DrivingLicenseRequest. We know all its tuples are non-null because it represents the superclass of the hierarchy and contains information on all requests in the database, regardless of their status. So by placing this table on the left of the JOIN operation, we ensure that the information of all the tuples it contains appears – and it fills in NULL for the attributes from the other table, DrivingLicense.

Then, we do another LEFT JOIN with RejectedDrivingLicense following the same process. This results in a table where, despite using USING in the JOIN operations, we can impose conditions on the LicenseID attributes of all the tables. So for a tuple of the resulting Cartesian product to represent a pending request, the LicenseID attributes of the DrivingLicense and RejectedDrivingLicense tables must be NULL. This indicates that there are no tuples in the respective tables because the LEFT JOIN has been filled in with NULL if they didn't exist. We declare this condition using a WHERE clause and the IS operator, as you can’t compare an attribute with NULL directly using the = operator.

At this point, to simplify the query syntax and avoid chaining too many JOINs, we can create an intermediate table with the result we got by performing these LEFT JOINs and applying the previous condition. This way, we can later perform an INNER JOIN in the query to get the information of the person who made the request. We do this all through the PersonID attribute of the Person table and the foreign key PersonFK of the intermediate table Pending, which comes from the DrivingLicenseRequest table and refers to the person associated with the request.

In this query, we could also consider combining all the joins in a single FROM clause and skipping the WITH. This would be correct, but it would complicate the code by having all the JOINs chained. And, although this strategy can be more efficient under certain circumstances, we should seek a balance between code readability and efficiency.

To illustrate that the intermediate tables in the WITH clause can be defined by queries that contain subqueries, we’ll consider the same query as before and try to solve it using a different approach.

WITH Pending AS (
    SELECT D.*
    FROM DrivingLicenseRequest D
    WHERE NOT EXISTS (
            SELECT *
            FROM DrivingLicense A
            WHERE A.LicenseID = D.LicenseID
        )
        AND NOT EXISTS (
            SELECT *
            FROM RejectedDrivingLicense R
            WHERE R.LicenseID = D.LicenseID
        )
)
SELECT P.Name, Pending.*
FROM Pending INNER JOIN Person P ON P.PersonID = Pending.PersonFK;
To get pending requests, keep the tuples in DrivingLicenseRequest whose primary key {LicenseID} is not referenced (via the foreign key LicenseID) by any tuple in DrivingLicense or RejectedDrivingLicense.

The simplest option to implement this is to go through all the tuples in DrivingLicenseRequest using the FROM clause, and for each of them, construct two very similar correlated queries.

We can have one that gets all the tuples from DrivingLicense whose foreign key LicenseID refers to the primary key LicenseID of the tuple in DrivingLicenseRequest that we are going through, and

We can have another subquery that does the same but gets tuples from the RejectedDrivingLicense table.

In this way, we can later check if any of the tables returned by the subqueries contain tuples or not using the EXISTS operator.

If any of the subqueries return tuples, then the request is either accepted or rejected. But if both subqueries return an empty set, it means that for a certain request in DrivingLicenseRequest, there is no tuple in the respective DrivingLicense or RejectedDrivingLicense tables that references it. This then indicates that the request is being processed.

With this process, we get the pending requests, which we store in an intermediate table using the WITH clause. To combine the information of the person who made each request, we use the intermediate table in the query, specifically in an INNER JOIN operation with the Person table, just as we did before.

So with this example, we’ve seen that there are multiple SQL constructions that lead to the same result – meaning a query doesn't necessarily have to be solved in just one way.

Also, by using the WITH clause, we can define each intermediate table with SQL code that’s as "complex" as we need it to be. We can include subqueries, conditions, and generally any SQL statement, except for a WITH, which by default can’t appear inside another WITH.

If we need to use an intermediate table to solve a query defined as a CTE, we need to define it at the same level as the other intermediate tables in our query, meaning in a single WITH statement (as we’ll see below).

So far, we have seen that we can use the WITH statement to define an intermediate table that we use to solve the query more comfortably and easily in certain situations. But, we might need several intermediate tables to solve a query, not just one.

For example, in the below code we have a query that gets information about people who have lived in at least two cities. We solved this query in the “Tuple filtering” section using JOIN operations – but we can also follow a similar approach where we first create several different intermediate tables and finally solve the query based on the results of these intermediate tables.

WITH R1 AS (
    SELECT PersonFK, CityFK AS CityA
    FROM Residence
),
R2 AS (
    SELECT PersonFK, CityFK AS CityB
    FROM Residence
),
CityPairs AS (
    SELECT DISTINCT R1.PersonFK
    FROM R1 INNER JOIN R2 ON (R1.PersonFK = R2.PersonFK
        AND R1.CityA <> R2.CityB)
)
SELECT P.*
FROM CityPairs MC INNER JOIN Person P ON MC.PersonFK = P.PersonID
ORDER BY P.PersonID;
First, we can create several intermediate tables that contain all the tuples from Residence – specifically the information about the person and city that make up each residence. We can do this by obtaining the attributes PersonFK and CityFK, which are foreign keys that refer to the person who has lived in a certain city during that residence. By constructing several intermediate tables with this information, we can rename CityFK with an alias like CityA in one of them and CityB in the other intermediate table, so that later the JOIN between them has a clearer syntax.

To construct several intermediate tables in a single WITH statement, we can chain them with commas. Instead of using the WITH keyword multiple times, we have to use it only once and chain all the intermediate tables we want with commas, as shown above.

Subsequently, with the intermediate tables R1 and R2 containing this information, we can create another intermediate table where we get the identifiers of all the people who have had a residence in several different cities (or in at least two cities).

To do this, we can perform an INNER JOIN between R1 and R2 (a Cartesian product of their tuples) and keep the tuples from the Cartesian product where the foreign key values PersonFK match and the CityFK values do not match. This way, we keep those tuples from the Cartesian product that represent information about several residences of the same person in different cities.

These identifiers are for the people whose information we need to get from the Person table. So now we can finally perform an INNER JOIN between the intermediate table CityPairs and Person, so that the final result of the query is the information of the people who have had at least two residences in different cities. (They would not have appeared in a tuple of the Cartesian product between R1 and R2 otherwise.)

The important point about this query is to note that we have used multiple intermediate tables in the same WITH clause to solve it – and this is entirely possible but not always recommended. We can resolve this query in various ways, each with its own advantages or disadvantages depending on the characteristics we need the code to have, such as clarity, efficiency, maintainability, and so on.

To conclude this CTE section, let's consider another query where we need to get information about bus trips that have taken place after 2025 and where the bus has WiFi. The simplest way to create this query would be to gather information from the CityBus and BusTrip tables using a JOIN, and then apply conditions on the tuples of the corresponding Cartesian product. But to illustrate using multiple intermediate tables (CTEs) in a single WITH clause, in this case, we’ll divide the query resolution into several parts.

WITH WifiBuses AS (
    SELECT Plate, RouteNumber
    FROM CityBus
    WHERE FreeWifi = TRUE
),
AvailableTrips AS (
    SELECT TripDate, StartAddress, EndAddress, PlateFK
    FROM BusTrip
    WHERE EXTRACT(YEAR FROM TripDate) >= 2025 
)
SELECT T.TripDate, T.StartAddress, T.EndAddress, B.RouteNumber
FROM AvailableTrips T INNER JOIN WifiBuses B ON B.Plate = T.PlateFK;
First, we’ll get information about buses with WiFi in an intermediate table. To construct this table, we simply apply the condition FreeWifi=TRUE on the tuples of the CityBus table. In this case, when we do a SELECT * FROM CityBus; we can see that in the FreeWifi attribute, the boolean values are represented with the letters ‘t’ or ‘f’ – so we might think that in the query we should compare the attribute with ‘t’.

But boolean values in SQL are TRUE and FALSE, even though the DBMS represents them with another type of notation. So the correct way to check if the attribute contains the logical value true is to compare it with TRUE. Even though the representation of the boolean value might change, in SQL we should always operate with boolean values using the literals TRUE and FALSE.

Second, we construct another intermediate table with information about bus trips that have occurred in 2025 or later. We do this by getting all the tuples from BusTrip and filtering them using the EXTRACT() function and the YEAR feature of the date.

Finally, in the query, we perform a JOIN between both intermediate tables to gather all the information about trips and buses. This way, we get tuples with trips that occurred on dates equal to or after the year 2025, along with the information about the bus with WiFi that made that trip.

But in this case, we only return the route number of the bus to the user, which is also part of the information in the CityBus table. If this isn’t enough to identify the bus, we could also return its license plate in the SELECT, for example. This decision depends on what the end user needs.

Also, with this query, we can more clearly see the effect of coding a query using multiple intermediate tables on how efficiently it executes. For example, if we coded the query without WITH (and instead with JOIN operations between the respective CityBus and BusTrip tables and imposed conditions on the resulting tuples), we have to consider that the entire Cartesian product would be performed first and then filtered by the conditions.

But by using intermediate tables where each one imposes a certain condition on the tuples of each table, we can reduce the number of tuples in each intermediate table, since WifiBuses won’t contain all existing buses, but only those with WiFi (which will be fewer).

By applying this technique (known as early filtering), we ensure that when performing the final JOIN between the intermediate tables, the Cartesian product results in fewer tuples – meaning it works with smaller tables and is therefore more efficient.

Just keep in mind that in modern DBMS, this optimization can be carried out automatically even without using intermediate tables, depending on the nature of the query. So if it doesn’t significantly worsen the clarity of the query, we should filter the information from the tables to be combined via a JOIN as early as possible, which is why we have used multiple intermediate tables in a WITH clause.

Set Operations
We have seen that in most queries, we need to impose conditions on table tuples to filter them and keep only those that interest us. Sometimes, we even need to use logical operators to chain multiple conditions together.

But using logical operators AND, OR, and NOT is not the only way to chain multiple conditions. We can also take a different approach where, instead of applying a filter on all tuples, we divide the conditions that must be met and apply multiple filters, one for each condition based on logical operators. Finally, we get the resulting tuples from those filters and combine them using set theory operators, which perform functions equivalent to logical operators.

In other words, in SQL, we can chain multiple conditions in a WHERE clause, for example, using logical operators. Or we can use set theory operators to combine the resulting tuples from multiple filters, each applying one of those conditions, all without using logical operators.

As we will see below, the decision to use logical or set operators largely depends on the clarity of the resulting code and the efficiency we want to achieve in the query.

To start, let's consider a query where we need to get information on all pools that are currently in a maintenance or closed state. If we wanted to do this with what we already know, the simplest way would be to use a filter with a WHERE clause, combining the conditions that the Status attribute is 'closed' or 'maintenance' using the logical OR operator.

SELECT *
FROM Pool
WHERE Status='maintenance'
UNION
SELECT *
FROM Pool
WHERE Status='closed';
Besides using this operator, we can rethink the query to solve it using set theory operators. In this case, using only one logical OR operator, we divide the WHERE condition into several conditions by removing that OR operator. This results in the conditions Status='maintenance' and Status='closed', respectively.

Doing this, we can resolve two queries: one applying the first condition and another applying the second. This gives us two resulting tables, one with information about pools under maintenance and another table with all the closed pools.

But we wanted all of them in a single output table, not in several. So to combine all the tuples from both tables into one (so that they all appear in the resulting table), we use the UNION operator between both queries. It treats both queries as if they were multisets of tuples, resulting in another set of final tuples where the tuples from both tables are present. That is, all the tuples from both tables. This is just like in set theory where the union of sets AUB results in another set containing all the individuals from A, all from B, and all those in both A and B.

To apply a set theory operator, the schema of the tables returned by the queries we operate with has to be exactly the same. This means that they must have the same number of attributes with the same names and data types, and in the same order. Otherwise, we can’t compare tuples from both tables, and it wouldn’t be possible to determine if a tuple belongs to one of the sets involved in the operation.

When we run a query that includes a UNION, we can see that if there are duplicate tuples in the resulting tables from the queries we’re working with, those duplicate tuples disappear in the resulting table of the query. This happens because, by default, all set theory operators in SQL take multisets with tuples as input and produce a set of tuples, which means it won’t contain duplicate tuples. So if we want to force the appearance of duplicate tuples because the query requires it, we must add the ALL modifier after the corresponding UNION, INTERSECT, or EXCEPT operator.

For example, in this case, we want to get information about all the people who have rented at least one bike or owned at least one car in our system:

SELECT PersonFK
FROM Rental
UNION ALL 
SELECT PersonFK
FROM CarOwnership;
To do this, we first create a query that gets all the PersonID identifiers of people referenced by the tuples in the Rental table through their foreign key PersonFK. In other words, each tuple in Rental has a value in its foreign key PersonFK that corresponds to a certain person's identifier, which matches the value of their unique identifier PersonID. So selecting the PersonFK attribute is enough to get the identifiers of all the people who have at least one record in this table.

We do the same with another query on the CarOwnership table, which also has a foreign key PersonFK with the same characteristics.

Finally, when reviewing these partial results from the queries, we’ll see that some people may have rented several bikes or simply made several rentals, and they may also have multiple ownership records in CarOwnership, leading to duplicate tuples. So when building the final resulting table, we need to get the people who are present in one table or the other. This means we need to combine both tables with the UNION operator to get a final set with all the tuples from both tables.

In this specific case, we shouldn’t add the ALL modifier, as we simply want to know which people meet the condition of having at least one rental and at least one car ownership (so a person who has made multiple rentals doesn’t need to appear multiple times in the final table).

But if we wanted to keep the duplicate tuples, this example clearly shows that by adding ALL after the UNION operator, all duplicate tuples from both tables are preserved, resulting in the final table showing as many tuples with the same person's information as the number of rentals and properties they have had. In other words, the ALL modifier forces UNION to return a multiset, not a set that removes duplicate tuples.

We can see the effect of the ALL modifier at a glance by examining the resulting table from the query. But if we are working with a very large query or a database with a lot of information, we may want to wrap our query in another outer query that uses the aggregation function COUNT() to count how many tuples it returns.

For example, in the below code you can see that we have used the query we just looked at as a subquery in the FROM clause, so we get all the tuples it contains. Then in the SELECT, we use the COUNT(*) function to count how many tuples there are.

SELECT COUNT(*)
FROM (
        SELECT PersonFK
        FROM Rental
        UNION
        SELECT PersonFK
        FROM CarOwnership
    ) AS PersonTable;
It’s also important here to provide an alias to the subquery, because when subqueries appear in the FROM clause, the DBMS needs them to have aliases to distinguish them and avoid ambiguities regarding the origin of the attributes that are later selected or used in other clauses.

Continuing with the different set theory operators that SQL offers, we have INTERSECT, which combines the results of several tables to return a set with all the tuples that appear in all the tables simultaneously. To understand this, we have the query below that retrieves information about all the people who have a driver's license and have also rented at least one bike.

SELECT DR.PersonFK AS PersonID
FROM DrivingLicenseRequest DR INNER JOIN DrivingLicense DL 
  ON DR.LicenseID = DL.LicenseID
INTERSECT
SELECT R.PersonFK
FROM Rental R;
We could write this query using only JOIN operations. But this time it might be more natural and straightforward to think of it as a set operation.

First, as shown above, we can construct a query that returns the identifier of all the people who currently have an active driver's license. To do this, we perform a JOIN between DrivingLicenseRequest and DrivingLicense, so we can gather the driver's license information with the foreign key PersonFK from DrivingLicenseRequest, which identifies the person who applied for the license. Then, we can construct another query that returns all the people who have rented at least one bike using the foreign key PersonFK from the Rental table.

And finally, to know which people have a driver's license and have also rented at least one bike, we need to keep the tuples that are in both tables. In other words, if the tables are multisets containing tuples, we need to keep those that appear in both multisets at the same time.

Instead of using the UNION operation, which represents the union of sets, we can use INTERSECT. It performs the intersection between sets, resulting in the final table of the query where we only have those people who meet all the conditions.

In the previous query, we only got the identifier of each person, since knowing the value of their primary key {PersonID} is enough to uniquely identify a person. With the foreign keys PersonFK pointing to the primary key {PersonID} of the Person table, we don't need the query to return more information about the person, as we can identify them with their primary key.

But there are situations where we may want to get more information about the person in the same query, such as their name in addition to the identification. So, if we needed to modify the query to return this, the simplest way would be to use a WITH clause that stores the identifiers of the people and then perform an INNER JOIN with the Person table in the body of the query, thus obtaining all the information present in the Person table.

SELECT DR.PersonFK AS PersonID, (SELECT Name FROM Person WHERE PersonID=DR.PersonFK)
FROM DrivingLicenseRequest DR INNER JOIN DrivingLicense DL 
  ON DR.LicenseID = DL.LicenseID
INTERSECT
SELECT R.PersonFK, (SELECT Name FROM Person WHERE PersonID=R.PersonFK)
FROM Rental R
ORDER BY PersonID;
But to illustrate other ways to get more information about people in the system from their identification in the same query without using WITH, we have the option to use subqueries in the SELECT clause, as you can see in the example above. Specifically, if we only need to return the name in addition to the PersonID, we can create a correlated subquery that, for each tuple to be returned, gets the name of the person with a specific identifier. In other words, it searches the Person table for the tuple with a certain PersonID, retrieves the name, and adds it as an additional column.

In general, using correlated subqueries in the SELECT is not a good practice because, as you might guess, the result of the subquery must be computed for each tuple to be returned. This also complicates maintenance, code clarity, and makes optimization by the DBMS more difficult in most cases.

Basically, with a simple WITH and an INNER JOIN, you can avoid having to traverse the entire Person table for each person to obtain a specific characteristic, instead gathering data from both the intermediate table of the WITH and the Person table.

Just like with other set theory operators, INTERSECT takes multisets as input and by default outputs a set, so there can't be duplicate tuples in the output table, as we saw earlier with UNION. So if we need to force the operator to always work with multisets and also return a multiset as the output of the intersection operation, we can add the ALL modifier, as shown in the example query below:


SELECT BikeFK AS BikeID
FROM Rental
WHERE StartTimestamp >= '2024-01-01'
INTERSECT ALL
SELECT BikeFK
FROM Rental
WHERE Duration <= 3
ORDER BY BikeID;
In this query, we get information on bikes that have been rented at least once during or after 2024 and whose rental duration was at most 3 hours. To do this, we construct a query that gets all bikes that have had at least one rental on a date \>= '2024-01-01', and then another that gets those that have had at least one rental with a maximum duration of 3 hours.

Then, to keep the bikes that meet both conditions at once, we perform the intersection between the multisets returned by the queries, keeping the tuples that are in both multisets at once. And, since the same bike may have had several rentals with these characteristics, there may be duplicates in the final query table. If we want to preserve them, we’ll have to add the ALL modifier.

By default, we don’t usually use ALL in this type of query, since we simply want to know which bikes meet the conditions. But it’s possible that, due to some user requirement, the query needs to return duplicates, in which case we’d use ALL.

In most situations, the performance is similar enough to be negligible. But the more data there is in the database, the more noticeable the difference in performance will be between using ALL or not. This also depends on the algorithms the DBMS uses internally to implement this operation.

The last set operator we'll look at here is EXCEPT, which is called MINUS in some DBMS. Basically, it implements the difference operation between sets, meaning if we have several sets A and B with tuples, the difference A-B returns a set with all the tuples that are in A and are not in B.

SELECT p.EntryFK, p.PersonFK, p.PoolFK
FROM PoolSanction p
WHERE p.BanEndDate < CURRENT_DATE
EXCEPT
SELECT p.EntryFK, p.PersonFK, p.PoolFK
FROM PoolSanction p INNER JOIN Sanction s ON p.SanctionID = s.SanctionID
WHERE s.Status = 'active';
For example, in the query above, we get information about all pool sanctions where the ban end date is before the current date when the query is run and that aren’t active.

As always, to do this, we might think the simplest way is through JOIN operations and a WHERE clause where the conditions are implemented. But to illustrate the use of EXCEPT, we can frame the query as finding all sanctions that meet the first condition of having a ban end date before CURRENT_DATE, and then from all of them, selecting only those that aren’t active. That is, from all those that meet the date condition, we keep only those that aren’t active.

Viewed another way, by getting all those that meet the date condition, we get a set A with tuples, where each one represents a sanction. Among all of them, there may be some that are active and some that are not. So to keep the ones we’re interested in, we need to remove from set A all those that are active. In other words, if we consider all the active ones to be in another set called B, then the sanctions we are interested in will be in the difference A-B (this means all the sanctions that meet the date condition (are in A) and are not active (are not in B)).

So in the query, you can see that we use the EXCEPT operator to work with the queries that get sets A and B, respectively. That is, the first query constructs set A by imposing the condition p.BanEndDate < CURRENT_DATE on the tuples of PoolSanction, while the query following the EXCEPT operator constructs set B by imposing the condition s.Status = 'active', gathering data from PoolSanction and Sanction to filter by the Status attribute, which is in Sanction instead of PoolSanction.

To implement the difference A-B, we use EXCEPT, where the query above the EXCEPT is set A and the one below is set B. This is important to keep in mind because EXCEPT is the only operator where the order of the operands can change the result of the query.

For example, with the other operators UNION and INTERSECT, we can clearly see that it doesn't matter if we unite or intersect several sets A and B or B and A – in any order, the result will be the same. This is not the case with the difference A-B, which doesn't necessarily have to be equivalent to B-A. This property is called commutativity, and EXCEPT is the only set operator that is not commutative.

Ultimately, in this query, we can see that the table aliases are all in lowercase. This is allowed in SQL, and we can even declare an alias in uppercase and then use it in lowercase, or vice versa. But if we enclose the aliases in quotes, like in Person "P", we can only refer to the table with the alias exactly as it’s written in the quotes.

On one hand, not quoting it provides flexibility when writing the code, as we don’t need to remember exactly how it was written. In most SQL code, quotes aren’t commonly used. But, this can cause ambiguity issues if the alias is named exactly the same as a table or another element in the database. Quoting it avoids these potential collisions with names of other elements.

In the end, the decision of which alias to assign to each table or element in the query mainly depends on its complexity and the style guide followed, among other factors.

Just like with other set operators, EXCEPT also takes multisets as input by default and returns sets, removing any duplicate tuples. So if we need to keep those duplicate tuples, we simply need to add the ALL modifier.

SELECT BikeFK AS BikeID
FROM Rental
EXCEPT ALL
SELECT BikeFK
FROM Rental
WHERE Duration < 2;
For example, in this query, we get information on all the bikes that have been rented at least once and have never been rented for less than 2 hours.

To implement this, we first construct a query that retrieves all the bikes that have been rented at least once using the foreign key attribute BikeFK from the Rental table. Then, with another query, we get all the bikes that have been rented at least once for less than 2 hours. Finally, to keep only those we’re interested in, we get the difference between the first set and the second.

As you can see, a bike may have been rented an arbitrary number of times, so the first query might return many duplicate tuples. If we don't use the ALL modifier, all those duplicates will be lost, resulting in a table where each bike is guaranteed to appear at most once.

But if we want to keep the duplicates, simply using ALL will show that the query returns a result table with many more tuples, as many of them correspond to duplicate bikes.

Here, we also need to consider that, despite the possibility of duplicate bikes existing in both set A and set B, when we perform the difference A-B using EXCEPT, we won't get any bike that's in B, regardless of how many times it's duplicated in both sets, one, or neither.

But when performing the operation using EXCEPT ALL, if we have x repetitions of a tuple in set A and y repetitions of that same tuple in B, then in the resulting table, we will get max{x-y,0} repetitions of that tuple. That is, when there are more repetitions in A than in B, we will get x-y repetitions of the tuple in the final table. If there are more repetitions in B than in A, then x-y is negative, so we will simply get 0 repetitions of the tuple. This means that tuple won’t appear in the resulting table of the difference operation implemented with EXCEPT ALL.

To correctly understand the difference operator, let's consider a query where we need to get information on all cruise trips for which there is no return trip. In other words, we want to find all trips going from a city x to another city y with no return to city x.

SELECT V.DepartureCityFK, V.ArrivalCityFK
FROM Voyage V
EXCEPT
SELECT V2.ArrivalCityFK, V2.DepartureCityFK
FROM Voyage V2
ORDER BY DepartureCityFK, ArrivalCityFK;
The approach we'll take for this query is based on set theory, as it’s particularly easy to solve in this case. First, we construct a query that returns all existing trips. From these, we can get all the information present in the Voyage table – but for simplicity, we'll only focus on the attributes that determine the departure and arrival cities of the trip (which are their foreign keys DepartureCityFK and ArrivalCityFK).

Then, from all those trips returned by the first query, we need to remove the return trips. That is, for each existing trip that departs from city x and arrives at city y, we look for a return trip in the Voyage table that departs from city y and arrives at city x. If it exists, we remove the original trip from the result table of the first query.

We could implement this using the IN operator and a subquery. But it's simpler and more efficient to build a second query that gets all the trips from Voyage but swaps the departure and arrival cities for each trip as shown above. This second query is responsible for getting all possible return trips that might exist by swapping the values of the foreign keys DepartureCityFK and ArrivalCityFK, meaning swapping the departure cities with the arrival cities.

Finally, with these two queries, we apply the EXCEPT operator, where we remove from the result table of the first query (which contains all the trips from Voyage) all those contained in the result table of the second query. In other words, from all existing trips, we are removing those considered return trips because they were generated by the second query by swapping the departure and arrival cities.

Even if some of those return trips don't exist in Voyage (which can happen), the EXCEPT operator will simply ignore that tuple to remove since it doesn't exist in the set from which it needs to be removed.

To wrap up this type of query, so far we have seen some that apply a single set operator. But SQL allows us to use any number of them in the same query or subquery as needed. This is applied in the query below, which retrieves information about all the people who have or have had a driver's license application registered in the database, or an approved license, and who have never had a rejected application.

WITH Persons AS (
    SELECT DR.PersonFK
    FROM DrivingLicenseRequest DR
        INNER JOIN DrivingLicense DL ON DR.LicenseID = DL.LicenseID
    UNION
    SELECT DR2.PersonFK
    FROM DrivingLicenseRequest DR2
    EXCEPT
    SELECT DR3.PersonFK
    FROM DrivingLicenseRequest DR3
        INNER JOIN RejectedDrivingLicense RD ON DR3.LicenseID = RD.LicenseID
)
SELECT PersonFK
FROM Persons
ORDER BY PersonFK;
Regarding the implementation, we can see that in the first query used to build the intermediate table Persons, we get all the people who have or have had an accepted driver's license by using an INNER JOIN between the DrivingLicenseRequest and DrivingLicense tables. This way, with the data from DrivingLicenseRequest, we can access the foreign key PersonFK that identifies the person who made the application.

Then, we make a union with a second query that retrieves people who have made an application, whether pending, accepted, or rejected. This time we get the data from the DrivingLicenseRequest table, which encompasses all existing applications in the database.

By performing the union, we get all the people who have or have had pending, accepted, or rejected applications, since the first query returns only those with an approved license – but the second returns people who have also had rejected applications.

To exclude those with rejected applications, the EXTENT operator is used along with another query that retrieves these people with rejected applications. So they are all excluded from the final query result – or rather from the intermediate table Persons. From this table, we finally get all its tuples and order them by the attribute PersonFK – that is, by the identifier of the people we obtain.

As you can see, the order in which set operations are performed is from top to bottom. That is, these operators act on tables containing query results, so SQL performs these operations in a top-down order (although we can use parentheses to change the precedence of the operators according to the needs of the query). Also, in this case, we can see that the UNION operation is redundant since everything contained in the first query is also contained in the second.

In other words, the second query retrieves people who have or have had applications of any type, whether pending, accepted, or rejected, so all the people who have had accepted applications are included in this set of people generated by the second query. In a real-world environment, we should optimize it by dispensing with the first query, which also implies the elimination of the UNION operation. But here we leave it as is to illustrate its equivalence with the optimized query we have described.

Aggregation Queries
Next, we’ll look at a type of query that’s often used in temporal data analysis, calculating metrics, building dashboards aimed at strategic decision-making, and so on. These are aggregation queries, and they’re based on treating the tuples of a table as if they were groups on which we can perform certain operations. For example, we can use them to sum all the values of an attribute in a certain group, find their average, and calculate the maximum or minimum value, among others.

As you might guess, the basic statements to implement this type of query are GROUP BY, HAVING, and the different aggregation functions offered by SQL.

With GROUP BY, we can choose a series of attributes whose values will determine how we form groups of tuples in a particular table. This means that each of these groups will be formed by a combination of values taken by the selected attributes. Then with the aggregation functions, we will calculate a certain metric for each group. w

With HAVING, we’ll impose conditions related to the characteristics of each group, mainly the value taken by the metrics we calculate on them.

To understand all this with examples, let's first consider a query where we want to get a list of all the nationalities present in the Person table and the number of people with each nationality.

SELECT Nationality, COUNT(*) AS NumPersons
FROM Person
GROUP BY Nationality
ORDER BY NumPersons DESC;
At first glance, we realize that we need to count a certain number of people for each value that the Nationality attribute takes. So, the approaches we've seen so far aren’t straightforward for performing this operation (which in some cases may not be possible without grouping).

For example, here we could list all the different nationalities that appear in Nationality and, for each one, use a subquery in the SELECT statement to count how many people in the Person table have that nationality.

But this approach would be very inefficient since, for each different nationality, we would have to go through the entire Person table looking for people with that nationality. Even though these searches can be optimized, they are generally not as efficient as the approach we’re going to follow using grouping.

Instead, we use grouping with the GROUP BY statement. Specifically, we indicate the table attributes that guide generating the tuple groups in the table. In this case, since we want to calculate a metric for each value of the Nationality attribute, we use that attribute to group the tuples. This way, for each value of that attribute, a group of tuples is generated, represented by that value, which will represent all the people with that same nationality.

Then, if we want to count how many people have that nationality, we simply count how many tuples each group has. So, in the SELECT statement, we add an extra attribute where we use COUNT(*). This time it won’t count all the tuples in the table, but those in each group. Since using GROUP BY makes it mandatory to return in the SELECT the attributes we are grouping by, the final table will only show the distinct values of Nationality, meaning the "representative" values of each group of tuples.

For each of those values, we’ll attach the value of COUNT(*) in the same tuple of the output table, which will correspond to the number of tuples in the corresponding group. This conceptually represents the number of people with that nationality.

Finally, we can apply sorting with the ORDER BY statement – but we should keep in mind that we can only sort in this case with respect to the attributes we return in SELECT. This is because in the query, we’re creating groups represented by Nationality values, which means we can’t "return" the rest of the attributes in the SELECT as we did before.

We can only calculate metrics with them and return those – but not the attributes themselves with all their values. This is because when grouping, the resulting table necessarily contains only the "representative" values of each group and metrics of other attributes calculated from those groups (or metrics of the group itself, such as the number of tuples it has in this case).

There are various metrics we can calculate with the basic aggregation functions that SQL provides by default. Below we see a query where we get, for each possible pool status, the smallest minimum depth and the largest maximum depth of the pools with that status, as well as the average depth and the number of pools in that status.

SELECT Status,
    MIN(MinDepth) AS Shallowest,
    AVG((MinDepth + MaxDepth) / 2.0) AS AvgDepth,
    MAX(MaxDepth) AS Deepest,
    COUNT(*) AS NumPools
FROM Pool
GROUP BY Status;
The implementation of this query is very similar to the previous one, as we need to group the Pool tuples by the values of their Status attribute, which determines the status of the pools.

So in the GROUP BY clause, we only specify the Status attribute. This way, we group the tuples into as many groups as there are values present in the Status attribute in the table, and in each of these groups, we have all the tuples representing pools in that status.

So along with the information for each status, we can calculate metrics for its associated group of tuples – that is, for the pools in that status. For example, with MIN(MinDepth), we get the smallest value of the MinDepth attribute present in the group for which this metric is being calculated. In this case, it represents the smallest minimum depth of all pools in a certain status.

Similarly, with the aggregation operation MAX(MaxDepth), we get the largest maximum depth, or in other words, the largest value of the MaxDepth attribute in the corresponding group of pools. With COUNT(*), we get the number of pools in each group.

On the other hand, the average depth associated with the pools in each group is calculated with AVG((MinDepth + MaxDepth) / 2.0). First, it’s worth noting that both in the SELECT clause and in the input argument of an aggregation function like AVG(), we can perform arithmetic operations on the attributes.

For example, in this case, with (MinDepth + MaxDepth) / 2.0, we calculate the average value between the minimum and maximum depth of each pool – not of each group, but of each tuple in the group – all using decimal values like 2.0 so that the result isn’t automatically rounded to an integer. Then, with this value calculated for each tuple, we use the aggregation function AVG() to calculate the average of this value for each group.

That is, with (MinDepth + MaxDepth) / 2.0, we get a certain value for each tuple, and then with AVG(), we take all those calculated values for the tuples of a certain group and calculate their average. Thus, for each possible state of a pool, we obtain the average depth of all the pools in that state, first calculating the average depth of each pool and then calculating the average of these depths across all pools in a certain state.

But, in addition to calculating metrics for each group of tuples, we might need to keep only those groups whose metrics meet certain conditions, depending on the query to be resolved. For example, here we consider a query where we get, for each person, the number of bike rentals they have made since records began, as long as that person has made at least three rentals.

SELECT P.PersonID,
    P.Name,
    COUNT(*) AS RentalCount
FROM Rental R
    INNER JOIN Person P ON R.PersonFK = P.PersonID
GROUP BY P.PersonID, P.Name
HAVING COUNT(*) > 2
ORDER BY RentalCount;
To implement this query, we might have considered using a correlated subquery in the SELECT statement that counts how many tuples in Rental have their foreign key PersonFK pointing to each person. But this would be inefficient, since groupings are usually much faster for this type of task.

It’s also not possible to impose a condition WHERE COUNT(*) > 2, either in the subquery of the SELECT clause or in the main query (in general, conditions on aggregation functions can’t be imposed in a WHERE clause). So in this case, we would have to use another subquery in the WHERE clause that counts the number of rentals each person has and that then checks that this number is > 2.

To avoid using subqueries and make our implementation as fast as possible, we first perform an INNER JOIN between the Rental and Person tables. We combine all their information into tuples of their Cartesian product where we have rentals and data about the person who made them. We can do this by imposing the condition in the JOIN that the foreign key PersonFK of Rental points to the PersonID identifier of its same tuple in the Cartesian product.

After performing the JOIN, we use the GROUP BY clause to group the resulting tuples by the PersonID and Name attributes of the person table. We do this because we want to calculate a metric for each person, so we have to include their identifier (primary key) in the grouping of the GROUP BY statement (meaning all the attributes that form their primary key).

Also, since we want to return each person's name along with their identifier, we can include the Name attribute in the GROUP BY. But it's important to note that the attributes we group by must uniquely identify each group of tuples that is formed.

In other words, by grouping by PersonID, we are forming groups of tuples that contain all the rentals made by a certain person, identified by a value of their primary key PersonID. This serves as the "representative" of the group of tuples.

But since this PersonID attribute is enough to identify the group, it's fine if we include more information about the person in this "representative value" of the group. So instead of containing only their primary key, it includes more information about the person, like their name.

As you can guess, if instead of grouping by {PersonID} we group by a candidate key (or rather a superkey as in this case {PersonID, Name}), we’ll get the same groups as grouping by {PersonID}. This means that the same number of groups will still be generated as there are people in the table (since with a superkey we can uniquely identify each person, and therefore each group).

Adding the Name attribute to the grouping is not an arbitrary decision – we have to use the Name attribute in the SELECT statement. When using GROUP BY, we can only return in the SELECT statement those attributes that we have used in the GROUP BY clause (so, those we have used for grouping). So to get the person's name and not just their identifier, one option is to include the attribute in the GROUP BY so we can return it in the SELECT – or in other words, use the Name attribute for grouping.

But, this won’t always work because there are times when we group by an attribute A and want to return information about another attribute B. But for each value of attribute A, we have multiple tuples with multiple different values in attribute B. This prevents us from using B for grouping, although we can still calculate metrics on B.

On the other hand, to count how many rentals each person has made, we just need to use the aggregation function COUNT(*) after grouping by {PersonID, Name}. This forms groups of tuples from the Cartesian product where we have the same information for the same person, but each represents a different rental. By counting how many tuples each group has, we get the number of rentals made.

To get only those groups (people) who have made more than 2 rentals, we use the HAVING clause to impose that condition, since aggregation functions can’t be used in the WHERE clause. Also, we can’t use the alias given to the attribute constructed with COUNT(*) that’s returned in the SELECT in HAVING. Instead, we need to rewrite the definition of the attribute in HAVING.

That is, just like with WHERE, we can’t impose conditions on the attributes or columns of the resulting table we return by simply referring to their aliases – we have to use their definitions, as in this case with COUNT(*).

It's worth noting that including the Name attribute in the GROUP BY to "return" it in the SELECT isn’t the only option we have to do this (or even to get more information about the person). We always have the option to save the query result in an intermediate table with a WITH clause and then join it with the Person table or the appropriate one.

But we have another option, as shown below, which involves grouping only by the PersonID attribute and then using correlated subqueries in the SELECT to get the rest of the information for each person.

SELECT P.PersonID,
    (SELECT Name FROM Person WHERE PersonID=P.PersonID),
    COUNT(*) AS RentalCount
FROM Rental R
    INNER JOIN Person P ON R.PersonFK = P.PersonID
GROUP BY P.PersonID
HAVING COUNT(*) > 2
ORDER BY RentalCount;
But this option isn’t the most optimal: as with a correlated subquery in the SELECT, we can only add one attribute of information per subquery. This forces us to use one subquery per attribute we want to add, which is very inefficient as you can see. Also, correlating the subqueries reduces maintainability and possibly also the clarity of the code, which are qualities worth considering.

With these queries, we have seen how we can use GROUP BY to group tuples by one attribute, or even several if we need to add more information to the resulting table from the query. Also, we’ve seen the correct way to impose conditions on expressions with aggregation functions, which is by using the HAVING clause.

But, we’re not always trying to return more information to the user every time we use multiple attributes in the GROUP BY statement. Sometimes, we need to group tuples by more than one attribute.

For example, in the query below, we get all the person-pool pairs (only those present in CityPool) that exist in the system. Then for each of them, we calculate the average duration the user has spent in that pool across all their entries.

SELECT E.PersonFK AS PersonID,
    E.PoolFK AS PoolID,
    COUNT(*) AS VisitCount,
    AVG(E.Duration) AS AvgDuration
FROM Entry E
GROUP BY E.PersonFK, E.PoolFK
ORDER BY E.PersonFK, E.PoolFK;
As you can see, we get the data from the Entry table, where we have to perform a grouping with the attributes PersonFK and PoolFK, since we need to calculate metrics for each person-pool pair. With this grouping, each pair of person-pool values is a group formed by all the tuples in Entry that represent times the person entered that pool.

In this way, with AVG(E.Duration), we calculate the average of the Duration attribute for each group (so how long, on average, a person stayed at the pool on each visit) while COUNT(*) counts the number of those entries.

Finally, it's important to note that in this query, we’re only getting the person-pool pairs that appear in the Entry table – we’re not constructing all possible pairs. So we won't find any tuple in the resulting table of the query where a person has never entered a certain pool.

If we wanted to include this information, we would need to structure the query differently, constructing all combinations of person-pool in an intermediate table and then calculating how many entries each person has in each pool in another way (either using subqueries, OUTER JOIN operations, or even more advanced functions that aren’t covered here).

In the GROUP BY clause, we can use an arbitrary number of attributes for grouping. The query below shows how we can get the number of times the cruise has traveled that route and the sum of the distances covered on those trips for each cruise and route between two ports. We can also display the information of the cities where those ports are located.

SELECT V.ShipFK AS ShipID,
    V.DepartureNameFK AS DeparturePort,
    V.DepartureCityFK AS DepartureCity,
    V.ArrivalNameFK AS ArrivalPort,
    V.ArrivalCityFK AS ArrivalCity,
    COUNT(*) AS TripCount,
    SUM(V.Distance) AS TotalDistance
FROM Voyage V
GROUP BY V.ShipFK,
    V.DepartureNameFK,
    V.DepartureCityFK,
    V.ArrivalNameFK,
    V.ArrivalCityFK
ORDER BY V.ShipFK, TotalDistance DESC;
As you can see, in this case, we get all this information from the Voyage table, as it has multiple foreign keys to CruiseShip and Port, as well as City. These can help us implement this query easily.

Of all the attributes it has, we group by ShipFK, DepartureNameFK, DepartureCityFK, ArrivalNameFK, and ArrivalCityFK. This allows us to group the tuples of the Voyage table based on the combinations of values representing a cruise-route pair (where a route is considered as a pair of ports along with the city values where they are located).

These are redundant for the grouping itself, as clearly all ports belong to one and only one city (according to the domain). But if we want to know the city where the port is located, the simplest option is to include the DepartureCityFK and ArrivalCityFK attributes in the grouping so we can return them in the SELECT.

So for each cruise-route pair, we can count how many trips the cruise has made on that route using COUNT(*), and with SUM(V.Distance) we can get the sum of all distances covered on those trips (as the tuples of each group in this case are the trips the cruise makes or has made on the corresponding route).

On the other hand, in this type of query, it’s also common to use the DISTINCT modifier to count values in a group or perform a specific aggregation operation on them. For example, in the query below, we get all the people who have ever lived in a city. Then for each of them, we count how many different cities they have lived in.

SELECT R.PersonFK AS PersonID,
    COUNT(DISTINCT R.CityFK) AS NumCities
FROM Residence R
GROUP BY R.PersonFK
ORDER BY NumCities DESC;
To do this, we use the data stored in the Residence table, which has a foreign key PersonFK that can determine, in each tuple, the person associated with that residence record. Since we want to calculate a certain metric for each person who has had at least one residence, we group by the PersonFK attribute, as selecting data from the Residence table ensures that all these people have or have had at least one residence record.

Then, for each group of tuples formed, we could use COUNT(*) to count how many residences the "representative" person of each group has had. But in this case, we want to count the number of different cities they have lived in.

To do this, we will give COUNT() the CityFK attribute as the input argument, which is a foreign key that determines the city associated with a residence record. This would count all the values the CityFK attribute takes for each group, but not the distinct values. So, we’ll need to add the DISTINCT modifier before the attribute and inside the COUNT() aggregation function so that it only counts the distinct values that the CityFK attribute takes in each group. This corresponds to the number of different cities a certain person has been associated with through Residence, meaning where they have lived.

When using the DISTINCT modifier in aggregation functions, we may also need to apply conditions on these aggregation functions. So we’ll need to use the same DISTINCT modifier in other clauses like HAVING, in addition to SELECT where most aggregations are calculated and returned.

WITH PersonsTable AS (
    SELECT CB.PersonFK AS PersonID,
        COUNT(DISTINCT CB.PaymentMethod) AS NumPaymentMethods
    FROM CruiseBooking CB
    GROUP BY CB.PersonFK
    HAVING COUNT(DISTINCT CB.PaymentMethod) > 1
    ORDER BY NumPaymentMethods DESC
)
SELECT *
FROM PersonsTable PT
    INNER JOIN Person P USING (PersonID);
For example, above we have a query that retrieves information about all the people who have made at least one cruise booking. It also gets the number of different payment methods they have used to pay for all those bookings, as long as that number is at least two different payment methods.

First, to implement this query, we use the CruiseBooking table and group by PersonFK – as when needing to calculate a number for each person, we should group the tuples of the table by the PersonFK attribute. This way, each group corresponds to the bookings made by a certain person.

So we can easily use COUNT(DISTINCT CB.PaymentMethod) to count how many distinct values the PaymentMethod attribute takes in each group of tuples. This corresponds to the number of different payment methods the representative person of that group of tuples has used to pay for their bookings.

Also, to require that they’ve used at least two payment methods, we use a HAVING clause where we declare that the value returned by the aggregation function COUNT(DISTINCT CB.PaymentMethod) must be \>1.

We can’t use its alias name NumPaymentMethods to declare this condition (some DBMS allow it, but for portability reasons, it’s better to code it without using the alias in the condition), nor can we use a WHERE because it’s an aggregation function. The correct way is using a HAVING.

Although it may seem that the value of NumPaymentMethods is being calculated multiple times unnecessarily, internally the DBMS can optimize the query automatically to avoid this type of unnecessary calculation. But we still have to write the aggregation function multiple times: once to define the attribute NumPaymentMethods in the SELECT and another in the HAVING to impose a filtering condition on the tuples of the resulting table from a grouping.

Finally, we save the result of this grouping in an intermediate table called PersonsTable, where only the identifier of each person and their corresponding number of payment methods are stored. Later, we can use this intermediate table in a JOIN operation with the Person table. This will gather all the information of each person along with the attribute containing the number of payment methods in one table. Then this is ultimately returned as the query output to the user.

So as expected, if we use the DISTINCT modifier on an attribute in an aggregation function in the SELECT clause and want to impose a condition on it, we have to write it exactly as it appears in a HAVING clause – regardless of whether it uses the modifier or not, since we write it exactly as it appears in the SELECT.

So far, we have seen that we can give an aggregation function input attributes with which it will perform the corresponding aggregation operation. Also, if we need only the distinct values of a certain attribute or result of an arithmetic operation between attributes, we can add the DISTINCT modifier.

But DISTINCT is not only used for a single attribute – we can also use it to. getunique combinations of values from a series of attributes, or even unique results obtained from an arithmetic operation involving multiple attributes. For example, in the query below, we want to get all the cruises along with the number of pairs of departure and arrival cities they have visited throughout their journeys.

SELECT V.ShipFK,
    COUNT(DISTINCT (V.DepartureCityFK, V.ArrivalCityFK)) AS NumRoutes
FROM Voyage V
GROUP BY V.ShipFK
ORDER BY NumRoutes DESC;
To do this, we simply group the tuples of the Voyage table by the ShipFK attribute, since we want to calculate a number for each cruise (and the ShipFK foreign key is what determines the cruise that made the journey). Thus, each group of tuples will be “represented” by a certain value of ShipFK that uniquely identifies a cruise. Those tuples, in turn, will represent all the journeys that cruise has made.

So to count how many distinct pairs of departure and arrival cities each cruise has traveled to, we can use the aggregation function COUNT(DISTINCT (V.DepartureCityFK, V.ArrivalCityFK)).

As you might guess, a cruise can make the same trip several times, which means that within the same group of tuples, we might find the same combination of values for the attributes (V.DepartureCityFK, V.ArrivalCityFK) multiple times. These attributes uniquely identify the departure and arrival cities of the trip, so if the trip is made several times, there must be several "duplicate" tuples – or at least tuples with the same values in those attributes, since there can be multiple different ports in the same city.

If we look at all the combinations of values that the attributes (V.DepartureCityFK, V.ArrivalCityFK) take in each group, we’ll see that they represent the departure and arrival cities of the cruise in each trip it has made. By applying the DISTINCT modifier, we treat each pair of values as if it were unique, and we keep all those that are unique. This refers to pairs of different departure and arrival cities in the group on which this aggregation operation is calculated, ignoring all duplicate pairs (which would inflate the count artificially). This would represent the total trips the ship has made.

Finally, when solving a query using grouping, we might need to calculate metrics using aggregation functions with the DISTINCT modifier. Because of this, we have to consider that if we need to impose a condition on that metric, we’ll need to use the aggregation function in the HAVING clause exactly as declared in the SELECT (with the same DISTINCT modifier and attributes present in the input argument of the aggregation function).

SELECT CB.PersonFK AS PersonID,
    COUNT(DISTINCT (CB.CabinNumber, CB.PaymentMethod))
FROM CruiseBooking CB
GROUP BY CB.PersonFK
HAVING COUNT(DISTINCT (CB.CabinNumber, CB.PaymentMethod)) > 1
ORDER BY count DESC;
For example, in the query above, we get the identifiers of certain people, and for each one, we calculate the number of distinct pairs composed of cabin number-payment method that the person has generated through their various CruiseBooking reservations in their name. Here, we’re only interested in those people whose number of pairs is at least two.

To implement this, we first group by the PersonFK attribute of the CruiseBooking table, as it contains all the information we need to calculate the previous metric for each person. This is why we only need to group by the foreign key PersonFK attribute. So for each group of CruiseBooking tuples, we’ll have all the reservations associated with a single person.

Then, with COUNT(DISTINCT (CB.CabinNumber, CB.PaymentMethod)), we can calculate the number of distinct combinations of values that the attributes (CB.CabinNumber, CB.PaymentMethod) take in the group of tuples. As you can see, when we want to count combinations of values from several attributes, we declare both attributes in a "tuple" (CB.CabinNumber, CB.PaymentMethod), which we provide as input to the COUNT() aggregation function. We use the DISTINCT modifier to ensure it only counts distinct combinations of these two attributes.

Later, if we want to say that the result of the aggregation function has to be greater than 1 to consider the corresponding group of tuples in constructing the query output, we use the HAVING clause. In it, we can declare the condition by rewriting the aggregation function again in the HAVING clause in the same way we declared it in the SELECT.

Note that we haven't assigned an alias to the attribute we built with the aggregation function, so SQL automatically assigns the name “count” to this additional attribute. This corresponds to the name of the aggregation function used in its construction.

But if we create more attributes using the same aggregation function COUNT(), then all those attributes will be called “count” by default at the same time, creating an ambiguity problem. This is why it's essential to use aliases for attributes that are expected to be named the same way by SQL (the DBMS is responsible for assigning default aliases).

Finally, it's important to note that queries don't necessarily have to contain only one grouping. The GROUP BY clause can be used an indefinite number of times in a query, especially if it includes a subquery, which can use the GROUP BY clause again.

But it's important to remember that you can't have multiple groupings “at the same time” in the same query. In other words, if we use GROUP BY multiple times, it must be because our query consists of subqueries, with each subquery performing a grouping, avoiding doing them all at the same level. In other words, for each GROUP BY clause, there must be one and only one SELECT clause.

Division Queries
At this point, with everything we've seen, we have enough tools to solve practically any query with SQL. (But there are some that we won't focus on here because they require operating on data recursively or hierarchically, which is more complex.)

Now, we’ll look at a series of queries where we use these previous tools to implement a relational algebra operator that doesn't have a direct implementation in SQL. For example, we have seen that the SELECT statement represents the implementation of the projection operator in relational algebra. And other statements such as certain types of JOIN or UNION, INTERSECT, and EXCEPT have direct equivalent operators in relational algebra as well.

But the division operator doesn’t have an equivalent clause or function in SQL due to its nature. In short, if we have several tables, this operator is responsible for getting all the tuples from one of the tables that are “associated” with each and every tuple of the other table.

To understand how this works, let's consider the following example:

SELECT R.PersonFK
FROM Rental R
GROUP BY R.PersonFK
HAVING COUNT(DISTINCT R.BikeFK) = (
        SELECT COUNT(*)
        FROM Bike
    );
Say we want to find the people who have rented each and every bike registered in our database. To implement this, we’ll have to use the division operator, since in the query setup we will have two tables: one with tuples containing information about a person and a bike, indicating that the person has rented the bike, and another table with all the bikes registered in the system. If we apply a division operation from relational algebra on these tables, we can find all the people who appear in enough tuples in the first table to have rented all the bikes in the second table.

This can be implemented in many ways, and here we will focus on two. The first method involves counting how many different/distinct bikes each person has rented and checking if that number matches the total number of bikes in the system (we’ll see how to do this below). If it matches, then we know that person has rented all the bikes.

To do this, we can group the tuples in the Rental table by the foreign key PersonFK attribute, since we need to calculate how many bikes each person has rented. So we form groups of tuples representing the rentals of each person who has rented at least once (people who have never rented don’t appear in PersonFK of the Rental table).

Next, using the HAVING clause, we count how many different bikes each person has rented with COUNT(DISTINCT R.BikeFK). This means that for each group of tuples, we count how many different values the BikeFK attribute takes in that group. This represents the number of different bikes rented, since BikeFK is the foreign key pointing to Bike and uniquely identifies the bike that has been rented.

Finally, we compare this number with the total number of bikes in our database, which we can get through a subquery using the aggregation function COUNT(*). Remember that we can use COUNT(*) without the GROUP BY clause being present in the subquery.

We can also approach the division of tables from a perspective closer to set theory. For example, below is the same query solved using NOT EXISTS and subqueries only:

SELECT P.PersonID
FROM Person P
WHERE NOT EXISTS (
        SELECT *
        FROM Bike B
        WHERE NOT EXISTS (
                SELECT *
                FROM Rental R
                WHERE R.PersonFK = P.PersonID
                    AND R.BikeFK = B.BikeID
            )
    );
Here, if each person has rented every bike in the database, then there's not a bike that exists that they haven't rented. Let’s try to translate this concept into a SQL query literally: first, with a SELECT and FROM, we can “traverse” all the tuples of Person. Then for each one, we check that there is no bike the person hasn't rented. To do this, we construct a correlated subquery that returns all bikes that person hasn’t rented.

This subquery is simply constructed by "traversing" all the tuples of Bike and checking that there is no rental record between the person and the bike being traversed. We do this another correlated subquery that traverses the tuples of Rental and returns only those in which the person has rented the bike. If it doesn't return any tuples, then there were none with those characteristics, which tells us that the person traversed has never rented the bike traversed. In this case, we’re not interested in that person since they have not rented all the bikes in the database.

If someone really had rented them all, then the correlated subquery that traverses the tuples of Rental would always return at least one tuple. Then the the correlated subquery that traverses the tuples of Bike would never return tuples. And this would satisfy the NOT EXISTS condition that we imposed on the people.

If we read the SQL code we’ve implemented "literally," we’re traversing the people, and for each one we’re checking that they’ve rented every bike. So we finally get the same people as we do with the query that uses a grouping and a count of bikes rented by each person.

If we execute either of these queries, they probably won't return any results. After all, the probability of a person having rented each and every bike registered in the database is small.

But if we want to check whether the queries work or not, we can always manually insert tuples into Person, Bike, and Rental, especially in Rental. Then there would a person who has a tuple in Rental for each bike in the Bike table, and thus can be present in the result of the division operation.

Another query we can solve using a division operation from relational algebra is the one below. In it, we find all the people who have entered all the pools in a certain city (specifically the one with the CityID value of 55).

SELECT E.PersonFK
FROM Entry E
    INNER JOIN CityPool CP ON E.PoolFK = CP.PoolID
    INNER JOIN Pool P ON CP.PoolID = P.PoolID
WHERE P.CityFK = 55
GROUP BY E.PersonFK
HAVING COUNT(DISTINCT E.PoolFK) = (
        SELECT COUNT(*)
        FROM CityPool CP2
            INNER JOIN Pool P2 ON CP2.PoolID = P2.PoolID
        WHERE P2.CityFK = 55
    );
In this case, we first gather all the information from the Entry, CityPool, and Pool tables. This lets us get the information of the people who have entered the pools and the information of the city where the pool is located. So after gathering this information with the INNER JOIN operations, we impose the condition that the foreign key CityFK must reference a certain city, specifically the one identified with the value 55 in its primary key {CityID}. We do this to filter the resulting tuples from the JOINs, so that we only have those where people have entered pools in the specific city we are considering in the query.

But this condition doesn’t necessarily have to be in that WHERE clause, as there are other equally valid alternatives (using subqueries, CTE, and so on.).

Then we group the tuples by PersonFK, so that each group of tuples represents all the pool entries of a certain person (specifically entries in pools of the city identified by the value CityID=55). To find only the people who have entered all the pools in that city, we use COUNT(DISTINCT E.PoolFK) to count the number of different pools they have entered. This equals the number of distinct values taken by the foreign key PoolFK in the Entry table. We then compare this number with the total number of city pools located in the city with CityID=55, all obtained through a simple uncorrelated subquery.

In this subquery, we perform another INNER JOIN between CityPool and Pool to gather data on all city pools with the foreign key CityFK that determines the city they are located in. This lets us declare the condition P2.CityFK = 55 to count all the pools in that city using COUNT(*). Also, the advantage of the subquery being uncorrelated is that it only needs to be computed once, since the number of pools in that city doesn't change while the query is running.

If we try to solve the previous query using the approach closest to set theory, as we did earlier, we will end up with an implementation that mainly uses the NOT EXISTS operator and correlated subqueries.

Conceptually, we can solve the query by going through all the people in the database and checking that there’s no city pool located in the city with CityID=55 for which there is no entry associating the person with the pool. In other words, for each person, there must be an entry for every city pool in the city with CityID=55.

SELECT * 
FROM Person P
WHERE NOT EXISTS (
        SELECT *
        FROM CityPool CP
            INNER JOIN Pool PL ON CP.PoolID = PL.PoolID
        WHERE PL.CityFK = 55
            AND NOT EXISTS (
                SELECT *
                FROM Entry E
                WHERE E.PersonFK = P.PersonID
                    AND E.PoolFK = CP.PoolID
            )
    );
Now, when coding all this, we arrive at a query very similar to the previous one where we looked for people who had rented all the bikes. As you can see, we first go through all the tuples of Person, and for each one, we construct a correlated subquery that gets all the city pools the corresponding person has not entered.

To do this, we perform a JOIN between CityPool and Pool and impose the condition that ensures all the city pools we consider are located in the city with CityID=55. Also, we verify with another correlated subquery that there is no entry of the person in the pool we are examining.

Each of these approaches to the same query have significant performance differences. In this last implementation, we nest several subqueries, leading to traversing many tuples, which is often unnecessary. On the other hand, using grouping tends to be faster since the traversal of tuples mainly depends on how the GROUP BY operation is implemented (which usually provides adequate performance for most queries).

Besides performance, in this last query, we can easily return all the information for each person because we directly use the Person table in the implementation. In contrast, in the previous approach using GROUP BY, we only returned each person's identifier, forcing us to use CTEs to perform a JOIN with the Person table if we want to return more information besides the identifier.

So when performing a division in SQL, we should consider not only the efficiency of the implementation but also the ease of modifying the query, as well as its clarity and maintainability.

The previous two queries were formulated considering the city with CityID=55, although this is an arbitrary decision. If we want to choose an appropriate value for CityID so that the two previous queries return data, since there may be cities where no person has entered all their pools, we can use the query below. For each person and city, it gets the number of pools in that city the person has entered, along with the total number of pools in that city.

SELECT E.PersonFK,
    P.CityFK,
    COUNT(DISTINCT E.PoolFK) AS EnteredPools,
    (
        SELECT COUNT(*)
        FROM CityPool AS CP2
            INNER JOIN Pool AS P2 ON CP2.PoolID = P2.PoolID
        WHERE P2.CityFK = P.CityFK
    ) AS TotalPools
FROM Entry AS E
    INNER JOIN CityPool AS CP ON E.PoolFK = CP.PoolID
    INNER JOIN Pool AS P ON CP.PoolID = P.PoolID
GROUP BY E.PersonFK, P.CityFK
ORDER BY EnteredPools, TotalPools;
As you can see, several JOIN operations are first performed to gather all the information from Entry, CityPool, and Pool, so we can later group the resulting tuples by PersonFK and CityFK. This means grouping the tuples into groups where each represents the entries a certain person has made in the pools of a certain city. Then, with COUNT(DISTINCT E.PoolFK), we count the pools they have entered, since PoolFK is the foreign key in the Entry table that determines the pool the person has entered. Finally, with a correlated subquery in the SELECT, we get the total number of pools in the city identified by CityFK.

Finally, it's important to note that with this query, we will never get a value of 0 in the EnteredPools attribute. If a person has never entered any pool in a certain city, there will be no resulting tuples from these JOIN operations with (E.PersonFK, P.CityFK) attributes that reference both the person and the city.

This happens because no entry (Entry tuple) will have its foreign key PersonFK as the person and its foreign key PoolFK as a pool in the corresponding city that the person has visited (since they haven't visited any pool in that city).

So if we also want to include in our query's resulting table tuples with (E.PersonFK, P.CityFK) pairs of people who have never visited any pool in the city CityFK, we would need to use set operations to find those (E.PersonFK, P.CityFK) pairs and add the tuples constructed from these pairs to the final resulting table.

We can see another fundamental case we might encounter when implementing a division operation in SQL below. Here, we have a query where we get all the people who have or have had pool sanctions in all possible states.

SELECT PS.PersonFK
FROM PoolSanction PS
    INNER JOIN Sanction S ON PS.SanctionID = S.SanctionID
GROUP BY PS.PersonFK
HAVING COUNT(DISTINCT S.Status) = 3;
To implement this, we first perform a JOIN between PoolSanction and Sanction, ensuring with the first table that the sanction occurred in a pool and using the second table to get the sanction's state (which is stored in the Status attribute).

Then, as we need to count how many states each person has sanctions in, we group by the PersonFK attribute, creating groups of tuples that represent the sanctions each person has or has had. This way, we can use HAVING to require that the number of states in which a person has sanctions is equal to the total number of possible states a sanction can have.

On one hand, with COUNT(DISTINCT S.Status), we can count how many different values the Status attribute takes in each group – or in other words, the number of states of the sanctions associated with a person. And, since there are three possible states ('created', 'active', 'expired'), we simply compare the resulting count from the aggregation function with 3.

But if we use the constant 3 in the comparison and later modify the database to include more or fewer states in the sanctions, we will be forced to change that number. This makes the query not as maintainable as it could be.

So another another option we have for declaring the condition in the HAVING clause is to compare the result of the aggregation function COUNT(*) with the result of a subquery that calculates how many possible states a sanction can have.

SELECT PS.PersonFK
FROM PoolSanction PS
    INNER JOIN Sanction S ON PS.SanctionID = S.SanctionID
GROUP BY PS.PersonFK
HAVING COUNT(DISTINCT S.Status) = (SELECT COUNT(DISTINCT Status) FROM Sanction);
As up can see above, this subquery is non-correlated, as it simply counts how many distinct values the Status attribute takes in the Sanction table. But implementing the query this way has a problem: we’re assuming that in the Sanction table, specifically in the Status attribute, we can find all the possible values that Status can take. But this might not be the case, as if the table is empty, no distinct values can be counted in the Status attribute.

This means that this last implementation of the query only works when the Sanction table contains tuples representing sanctions where there is at least one sanction in all possible states. If we can guarantee that the database meets this condition, then the above implementation is more convenient for us because it requires no maintenance.

But this condition is not usually met, so it’s not a good practice to assume that we’ll find all possible values an attribute can take in that attribute. For example, if we think of an integer attribute, it’s clear that there don’t have to be tuples that take a different value for every possible value the attribute can have.

Another option we have is to skip grouping and use a set theory-based approach. As you can see, in the implementation above, we go through all the tuples of Person, and for each one, we check that there exists a pool sanction with the status ‘created’. Also, using the logical operator AND, we require that at the same time there exists another pool sanction with the status ‘active’. Finally, we use another logical operator AND to also require that for that person there exists another pool sanction in the status ‘expired’.

SELECT p.PersonID
FROM Person p
WHERE EXISTS (
        SELECT *
        FROM PoolSanction ps
            INNER JOIN Sanction s ON ps.SanctionID = s.SanctionID
        WHERE ps.PersonFK = p.PersonID
            AND s.Status = 'created'
    )
    AND EXISTS (
        SELECT *
        FROM PoolSanction ps
            INNER JOIN Sanction s ON ps.SanctionID = s.SanctionID
        WHERE ps.PersonFK = p.PersonID
            AND s.Status = 'active'
    )
    AND EXISTS (
        SELECT *
        FROM PoolSanction ps
            INNER JOIN Sanction s ON ps.SanctionID = s.SanctionID
        WHERE ps.PersonFK = p.PersonID
            AND s.Status = 'expired'
    );
As you can see above, this implementation is equivalent to the previous one where we used groupings and counts to implement the division operation. But this approach is clearly less maintainable, though possibly easier to understand in some aspects.

For example, in this implementation, the names of the different Status values appear explicitly in the conditions we impose in each correlated subquery, which is generally not a good practice. If you want to modify the database domain, you will also have to modify these values.

Also, we’re duplicating the same code multiple times, making the query code as a whole less maintainable. This is because if the query itself needs to be modified, it’s very likely that we’ll need to make changes in all three subqueries, slowing down the management process.

So although the set theory-based approach may be impractical in certain situations, it can work for a small database like the one we're dealing with here. But, whenever possible, it's best to choose solutions that are more maintainable and require fewer changes in the future.

In this specific case, the best option would be to use the grouping approach where the number of sanction statuses for a person is compared with the total number of statuses, as changing that number in the query is easier than modifying the code of several subqueries. This also avoids having to make assumptions about the Sanction tuples.

Let’s look at another query that’s similar to the previous ones, where we can see a different way the division operator can appear. It retrieves all the people who, for every city they have lived in, have visited at least one pool located in that city.

SELECT P.PersonID
FROM Person P
WHERE NOT EXISTS (
        SELECT *
        FROM Residence R
        WHERE R.PersonFK = P.PersonID
            AND NOT EXISTS (
                SELECT *
                FROM Entry E
                    INNER JOIN Pool PO ON E.PoolFK = PO.PoolID
                WHERE E.PersonFK = P.PersonID
                    AND PO.CityFK = R.CityFK
            )
    );
For each person, keep them only if there is no residence of theirs that lacks a matching pool-visit in the same city. Equivalently: for every city a person has lived in (from Residence), there must be at least one record showing they visited a pool in that city.

The implementation of this approach is very similar to how we express it in natural language. On one hand, we go through the tuples of Person with a SELECT and a FROM, and we set the condition that the result of a subquery is empty using NOT EXISTS.

In this correlated subquery, we go through the tuples of Residence for the person we are currently checking the condition for, so to keep only the residences we are interested in, we impose the condition R.PersonFK = P.PersonID in the subquery. This ensures that the selected Residence tuples have their foreign key PersonFK pointing to the person we are going through, whose identification is given by P.PersonID.

On the other hand, within this subquery, we also check that another correlated and nested subquery doesn’t return any tuples either. This last subquery is dedicated to getting all the entries where the person identified by P.PersonID has entered a pool located in the city identified by R.CityFK – that is, the city of the residence we are going through at the time of executing this subquery.

In summary, in this query, we have seen that divisions don’t always refer to situations where the tuples we want to obtain are "associated" with all the tuples of another table. Instead, as in this case, they can also refer to the output tuples of our query needing to meet a certain condition in relation to all the tuples of another table.

Similar to the previous query, we can consider another one where we need to find people who have or have had at least one travel booking in all existing cruise classes.

SELECT CB.PersonFK
FROM CruiseBooking CB
    INNER JOIN CruiseShip CS ON CB.ShipFK = CS.ShipID
GROUP BY CB.PersonFK
HAVING COUNT(DISTINCT CS.Class) = (
        SELECT COUNT(DISTINCT Class)
        FROM CruiseShip
    );
In this case, we start by setting up the division operation through grouping and counting. First, we perform an INNER JOIN between the CruiseBooking and CruiseShip tables. This allows us to gather information about the person who made each travel booking using the foreign key PersonFK from CruiseBooking and the information about the cruise class for the trip. This same table has a foreign key ShipFK that uniquely identifies the cruise ship for the trip, from which we can determine its class.

So after this operation, we group by the PersonFK attribute, as we’ll need to count how many different cruise classes each person has booked to perform the division.

Regarding this quantity, we calculate it using the aggregation function COUNT(DISTINCT CS.Class), which is executed once for each group of tuples. Then we compare it with the total number of cruise classes in our database.

In this case, we could have directly written the number instead of using an uncorrelated subquery to get the total number of classes by looking at the distinct values of the Class attribute from the CruiseShip table. So as it stands, with the subquery, we’re implicitly assuming that the CruiseShip table contains cruises in all existing classes (but this may not be the case).

Imagine if the table is empty, for example – the subquery would result in a total of 0 cruise classes, when in reality, there may be more (the domain of the Class attribute may contain more values than those actually appearing in the table).

But it’s important to clarify here that by “all cruise classes” we mean all possible values that the Status attribute can take – that is, the values we define as the domain of the attribute. On the other hand, in some circumstances, we can assume that all cruise classes correspond to the distinct values that the Status attribute takes in the CruiseShip table, all depending on the domain we are working with.

For simplicity, from now on in this domain, we’ll assume that the distinct values of an attribute like Class found in its corresponding table are equivalent to all the values it can take. If we think about it, this makes sense because if there are only 2 distinct values in the Class attribute of the CruiseShip table, then all the bookings made throughout the time the database has existed will have to reference some cruise in the CruiseShip table (whose Class will have one of those two values). There might be no bookings referencing cruises of a certain class, but if there are cruises of two classes, then it makes sense to assume that those two classes make up “all the classes the Class attribute can hold.”

So, by assuming that the Class attribute of the CruiseShip table contains "all the classes" of cruises, we can solve the query using a set theory approach as shown in the query below.

Here, we first go through all the tuples in CruiseBooking that represent bookings. In each one, we check that there is no cruise (of any class, rather) for which no booking has been made by the person referenced by the foreign key PersonFK of the CruiseBooking tuple we are examining.

SELECT DISTINCT CB.PersonFK
FROM CruiseBooking CB
WHERE NOT EXISTS (
        SELECT *
        FROM CruiseShip C
        WHERE NOT EXISTS (
                SELECT *
                FROM CruiseBooking CB2
                    INNER JOIN CruiseShip CS2 ON CB2.ShipFK = CS2.ShipID
                WHERE CB2.PersonFK = CB.PersonFK
                    AND CS2.Class = C.Class
            )
    );
That is, since we are only interested in getting the people who have ever booked, we start the query by going through CruiseBooking, not Person, because there may be people in Person who have never booked.

So to check that there is no cruise with these characteristics, we use the NOT EXISTS operator and a correlated subquery in which we go through all the cruises registered in the CruiseShip table. For each one, we check that there is no travel booking where the cruise is the one whose class is the same as the cruise and person we are currently examining in the query.

By doing this, we ensure that for all the people returned by our query, there is no cruise of any class that hasn’t been booked at least once by that person. But we can do this correctly only if we are sure that the Class attribute in CruiseShip includes what we consider as “all possible cruise classes“. If we didn't have this assurance, then this set theory approach would not be correct, because the correlated subquery that goes through the tuples of CruiseShip might not be covering all possible cruise classes.

For example, imagine the CruiseShip table is empty. In that case, this approach would return more people than it should, since that subquery would never return tuples.

On the contrary, in the other approach based on groupings, if the CruiseShip table is empty, then the uncorrelated subquery that counts the total number of classes would return 0. Also, the HAVING condition would never be met, preventing the return of people who do not meet the condition defined in the query statement.

So as you can see, it’s not always better to use just one approach based on either groupings or set theory – it varies depending on the situation.

In this specific case, it’s more practical to use groupings – mainly for efficiency (since internally a grouping is usually faster than executing a correlated subquery multiple times) but also for simplicity of maintenance and code clarity.

To wrap up our discussion on the division operator of relational algebra, it’s important to note that there are times when we have to do division using intermediate tables (CTE) instead of tables from the database itself.

For example, in the query below, we obtain the ships that have made at least one trip departing from and arriving at each pair of cities with an area of at most 11 km². In other words, all ships that have made at least one trip between each pair of cities with this characteristic.

WITH AllPairs AS (
    SELECT C1.CityID AS Dep, C2.CityID AS Arr
    FROM City C1 CROSS JOIN City C2
    WHERE C1.CityID <> C2.CityID AND C1.Area<11 AND C2.Area<11
),
ShipVisits AS (
    SELECT V.ShipFK,
        V.DepartureCityFK AS Dep,
        V.ArrivalCityFK AS Arr
    FROM Voyage V
        INNER JOIN City C1 ON V.DepartureCityFK = C1.CityID
        INNER JOIN City C2 ON V.ArrivalCityFK = C2.CityID
    WHERE C1.Area<11 AND C2.Area<11
)
SELECT SV.ShipFK
FROM ShipVisits SV
GROUP BY SV.ShipFK
HAVING COUNT(DISTINCT (SV.Dep, SV.Arr)) = (
        SELECT COUNT(*)
        FROM AllPairs
    );
As you can see in the implementation, to make the code simpler, we can first build an intermediate table with all possible pairs of cities with an area value <11. We. cando this by executing a CROSS JOIN between the City table and itself, as it contains all the cities registered in our database. Then we require that both cities in the pair have an area <11.

It’s important to note that the Area attribute of the City table contains values representing square kilometers, so it’s straightforward to declare the <11 condition in our query. But if this attribute had values in other units, we’d need to adapt to them or convert them to other units that we could easily work with. It’s crucial to consider the units in which the values we compare are measured to correctly code the query.

Finally, this table includes all possible pairs of cities that meet the area characteristic, meaning it doesn't matter if the same pair of cities (A,B) also appears in the table as (B,A).

Then, we build another intermediate table where we store the different pairs of cities each cruise has visited throughout all its trips, considering only those cities that meet the query conditions (area <11).

To do this, we simply extract the foreign key attributes ShipFK, DepartureCityFK, and ArrivalCityFK. These determine the cruise that made the trip and the departure and arrival cities of the trip from the resulting table of the INNER JOIN operations between the Voyage table and the City table itself.

We perform these operations to access the area information of each city, allowing us to impose the same area conditions as in the first intermediate table AllPairs. If we didn't do this, we might consider cruise trips between cities that don't meet the conditions we are looking for. This would increase the number of “valid” city pairs the cruise has traveled between. Since we’re going to structure the query using a grouping and a count, it’s essential not to count irrelevant elements for our query.

Once both CTEs are built, we perform a grouping on the ShipVisits tables based on the ShipFK attribute. We do this to calculate, for each cruise, the number of distinct pairs of cities it has traveled between. We easily calculate this using the aggregation function COUNT(DISTINCT (SV.Dep, SV.Arr)). Then we can compare the returned value with the total number of city pairs that exist and that we have stored in the first CTE called AllPairs, all within the HAVING clause.

To keep only those cruises that have traveled through every pair of cities calculated in AllPairs, we compare the output of the COUNT() aggregation function with the result of an uncorrelated subquery that simply counts how many tuples the intermediate table AllPairs has.

In the total count of pairs, we don’t have to use the DISTINCT modifier, since the CROSS JOIN never generates repeated city pairs given the very definition of the cross product operation. And there there are no identical tuples in the City table, meaning there are no identical cities in our database (much less with the same value of their primary key CityID). But if we wanted to use the DISTINCT modifier to count how many distinct tuples are in AllPairs, we could use the syntax COUNT(DISTINCT AllPairs.*).

Regarding this last subquery, we could have avoided explicitly constructing all the city pairs in AllPairs if we had directly performed the same computation as in AllPairs – but returning only COUNT(*). This would directly count all the city pairs with the characteristics we are looking for. But we can only do this if we code the query using grouping and counting, as we’ll see that it can also be implemented based on set operations, for which we’ll necessarily need to construct and store the pairs in AllPairs.

So just as we have shown with other queries, we can also approach this one using set theory operators. As you can see below, the intermediate tables are constructed in the same way except for ShipVisits, where we don't need the cities involved in the trips to meet the condition of having an area <11.

This is because those ShipVisits tuples will later be compared with the city pairs in AllPairs, which do meet the condition. This way, we end up with cruises that have made a trip in all the pairs of AllPairs, regardless of additional tuples in ShipVisits with trips between cities that don't meet the condition we're looking for.

Although this isn't crucial for resolving the query, it's important to note that ShipVisits contains more tuples than necessary, which might slow down the query since ShipVisits will later be used in a correlated subquery, resulting in multiple scans of its tuples.

WITH AllPairs AS (
    SELECT C1.CityID AS Dep, C2.CityID AS Arr
    FROM City C1 CROSS JOIN City C2
    WHERE C1.CityID <> C2.CityID AND C1.Area<11 AND C2.Area<11
),
ShipVisits AS (
    SELECT V.ShipFK,
        V.DepartureCityFK AS Dep,
        V.ArrivalCityFK AS Arr
    FROM Voyage V
)
SELECT SV.ShipFK
FROM ShipVisits SV
WHERE NOT EXISTS (
        SELECT *
        FROM AllPairs AP
        WHERE NOT EXISTS (
                SELECT *
                FROM ShipVisits SV2
                WHERE SV2.ShipFK = SV.ShipFK
                    AND SV.Dep = AP.Dep
                    AND SV.Arr = AP.Arr
            )
    );
After constructing the CTEs, we solve the query in a way similar to the other divisions we've seen. First, we go through the tuples of ShipVisits (although we could also choose to go through those of CruiseShip, since what we want is to go through all the cruises in the database, or at least those that have made a trip). So instead of using CruiseShip, which might contain cruises that have never made a trip, we choose to go through the tuples of ShipVisits, where we can find cruises referenced by the foreign key ShipFK from the Voyage table, which we know have made at least one trip.

In each of these tuples, we check that there is no pair of cities from AllPairs for which there is no trip made by the cruise we are currently going through between the cities of that pair.

To do this, we use the NOT EXISTS operator and two nested correlated subqueries. In the first, we go through the tuples of AllPairs – that is, the pairs of cities that do meet the condition of having an area <11. Then for each pair, we use NOT EXISTS again on another correlated subquery that gets all the trips made by the cruise currently being processed in the query execution over the cities of the corresponding pair from AllPairs.

In a more intuitive way, we’re getting all the cruises for which there is no pair of cities from AllPairs where the cruise hasn't traveled at least once. As you can guess, since the cities in AllPairs do meet the condition of having an area less than 11 km², it doesn't matter that ShipVisits has trips with cities that don't meet this condition – because in the query we check that for a certain pair of cities from AllPairs there is no trip of a cruise in those cities. So it’s really indifferent which cities are present in the trips of ShipVisits, as those that meet the condition will definitely be there since we don't impose any condition when constructing that intermediate table.

In summary, with this approach, we can solve the query just as we did before using groupings and counts. But the difference here is that we can save the conditions (area <11) that we imposed when constructing the tuples of ShipVisits.

At first glance, this might seem like an improvement in code clarity, as it’s shorter. This makes it more maintainable in this case because fewer operations and statements are needed to construct the CTE. But the resulting CTE contains more tuples, specifically those that represent all the trips each cruise has made, not just those made between cities that meet the condition of having an area <11.

This additional number of tuples impacts the computation of the query. But to analyze this impact, we must also consider that in constructing ShipVisits, we are saving two JOIN operations, which are highly costly, in addition to the expected amount of data with which the query will be executed.

For example, if the amount of data in the involved tables is small, the performance difference won’t be significantly noticeable. But if it’s large, it’s more beneficial to have the smallest possible number of tuples in ShipVisits, even if it requires performing an additional JOIN.

This is because the correlated subquery that goes through the tuples of ShipVisits is executed once for each tuple of AllPairs, and all of this is executed once for each tuple of ShipVisits (we could have replaced this last one with CruiseShip to improve performance, as the number of cruises is fixed and tends to be smaller than the number of trips).

So the computation involved in going through all the tuples of ShipVisits is much greater than the computation of a simple JOIN used to construct the CTE itself – which, despite being computationally costly, only needs to be executed once (not multiple times depending on the number of tuples in other tables).

To finish with the division operation, we've seen that we can implement it in SQL using the EXISTS operator (either as is or negated with the logical NOT operator) and a correlated subquery. In it, the SELECT statement uses the * notation to return all the attributes of the corresponding table. This means that to check if the subquery returns any tuple or not, we construct its result so that each tuple possibly has multiple attributes – meaning all those that result from using the SELECT * notation. But sometimes instead of returning several attributes, it simply returns a column with a fixed value like the integer 1.

In general, using the SELECT * notation in a correlated subquery to which the EXISTS operator is applied is considered good practice, so it’s coded this way by default. But there are also other possibilities like SELECT 1, which at first glance might seem more efficient because it doesn't return unnecessary attributes since it only checks if the subquery results in any tuple or not.

In summary, the decision on which attributes to return in a correlated subquery using the EXISTS operator is mainly determined by the characteristics of the DBMS, as each implementation of the DBMS handles these operations differently at the physical level.

Ranking Queries
To conclude with the different "types" of queries we might encounter, there are queries where we need to calculate a ranking – that is, ordering elements based on the value they have for a certain metric. For example, ordering people by the number of bike rentals they have made, allowing us to find out who has made the most or fewest rentals, among many other similar tasks.

In this case, these approaches don’t have any equivalent operator in relational algebra. This is because the calculation of rankings is based on the combination of multiple techniques and tools like groupings, aggregations, or uncorrelated subqueries that aren’t present in relational algebra as specific operators.

This is mainly because in relational algebra, there is no concept of order, and since tables are treated as sets of tuples, there is no unique way to number the tuples positionally to establish an internal order of the set. In other words, within a set, its elements don’t necessarily have an order among them unless we explicitly define it.

We can start by finding the maximum value of whatever we’re ranking for (and, optionally, where in the table this occurs). For example, in the query below, we get the maximum passenger capacity among all the cruise ships in the CruiseShip table.

SELECT MAX(PassengerCapacity) AS MaxCapacity
FROM CruiseShip;
In terms of approach, solving this query involves establishing a ranking of the cruise ships based on their passenger capacity (this is their metric). The one with the highest capacity occupies the first place in the ranking, followed by the other cruise ships. So if we take the first in the ranking and access its metric, we will have the maximum passenger capacity, which is what we want to obtain.

In SQL, implementing this query is very simple if we only want to get the metric value and its values are already calculated in an attribute. As you can see, we simply use the MAX() aggregation function, which we give the attribute where the metric values are calculated as an input argument. Finally, when we execute the query, we will see that only one tuple is returned with that maximum value in the attribute we have named with the alias MaxCapacity.

But the implementation is not always that simple. For example, if we want to get not only the maximum value of the metric but also the specific element associated with that metric – in this case, the cruise ship with the highest passenger capacity – we first need to go through the tuples in CruiseShip and check each one to see if it corresponds to the cruise ship with the highest passenger capacity.

Specifically, what we check in each tuple is whether the passenger count is equal to the maximum or not, so that we only keep those tuples where the PassengerCapacity value is exactly equal to the maximum value of that attribute.

SELECT ShipID, PassengerCapacity
FROM CruiseShip
WHERE PassengerCapacity = (
    SELECT MAX(PassengerCapacity)
    FROM CruiseShip
);
This is reflected in the query code above. In the WHERE clause, we check that the PassengerCapacity attribute value is equal to the result of the uncorrelated subquery that returns its maximum value. We use the MAX() aggregation function for this just like before.

If the values match, we will have found the tuple of the cruise ship with the highest passenger capacity. But there may be several cruise ships with that same capacity, so our query will return them as well.

If we want to get only one cruise ship, we have the option to add an additional clause LIMIT 1 at the very end of the query, which basically returns only the first tuple of the resulting table from the query.

This LIMIT clause, it’s not part of the SQL-92 standard, but it can still be used in any query we need as long as the DBMS supports it (all modern DBMSs support it). Its use is simple: we just give it a number that indicates the number of tuples from the resulting table of the query that we want to get from the first tuple located at the top of the table, ignoring the rest.

Another option we have is to do without the MAX() aggregation function. As you can see below, most of the query code is the same, except for the subquery. Instead of returning the maximum value of the PassengerCapacity attribute, it returns the attribute itself – meaning all the values in its corresponding column in the CruiseShip table.

SELECT ShipID, PassengerCapacity
FROM CruiseShip
WHERE PassengerCapacity >= ALL (
    SELECT PassengerCapacity
    FROM CruiseShip
);
In this way, the condition of the WHERE clause uses the operator >= along with the ALL modifier, which establishes that, for a certain tuple of CruiseShip, its PassengerCapacity value must be greater than all the values returned by the subquery. Or put another way, our query retrieves information on all cruise ships whose passenger capacity is greater than or equal to each and every capacity stored in the PassengerCapacity attribute of the CruiseShip table.

Specifically, here we have to use the operator \>=, not \>, because if we are going through the tuple of a cruise ship that does have the highest passenger capacity, its capacity will at most be equal to the maximum capacity of the CruiseShip table (but never greater). That is, if the maximum is a value X, then there will be no cruise ship with a capacity \>X, but there will be one or more with a capacity \=X, which are the ones we want to find.

At the same time, these have a capacity X that is greater than the rest of the capacities of the other cruise ships, which is why we use the operator \>=.

The ALL modifier is necessary to ensure that the value of the PassengerCapacity attribute meets the condition imposed by the >= operator with respect to each and every tuple returned by the subquery. In this case, it only returns tuples with an attribute or column with the values of all the passenger capacities that it must be compared with.

As you can guess, even though this way of implementing the query is equivalent to the previous one, here the subquery returns a series of values that are compared for each tuple of CruiseShip. That is, for each cruise ship, all the tuples of the subquery are traversed to perform the comparison declared in the WHERE clause. This requires much more computation than simply comparing with a number like the maximum capacity obtained from the MAX() function.

So since the subquery is non-correlated and is computed only once, it’s more optimal to use the previous approach where we used MAX() than this one, as this approach uses more space to store the subquery tuples and more time unnecessarily traversing them to make the comparisons.

Continuing with queries where we need to calculate a maximum, here we have another one where we get the person (or people) who have had the most residences in cities, along with that maximum number of residences.

SELECT R.PersonFK AS PersonID, COUNT(*) AS NumResidences
FROM Residence AS R
GROUP BY R.PersonFK
HAVING COUNT(*) >= ALL (
        SELECT COUNT(*)
        FROM Residence
        GROUP BY PersonFK
    );
We can find the information to solve this query in the Residence table – specifically in the tuples themselves, where each one represents a residence. The person is referenced by the foreign key PersonFK and the city where the person has lived is referenced by the foreign key CityFK.

So, in this table, we don't have a number in an attribute that tells us the number of residences a person has had. Instead, the tuples themselves represent the residences of the people, and we need to count them to know which person has or has had the most residences.

To do this, we can group the tuples in Residence by the attribute PersonFK, since we need to count residences for each person. In this way, we form groups of tuples that represent all the residences a person has had.

Once the groups are made, we can use COUNT(*) to count how many residences the "representative" person of that group of tuples has or has had. Then, to ensure that this number is the maximum, we use the operator >= along with the ALL modifier and a subquery.

In this case, the subquery calculates, for each person, the total number of residences they have or have had in the same way as in the main query, using a grouping by the PersonFK attribute of Residence and the aggregation function COUNT(*).

With this, we can verify, in the HAVING clause, that the number of residences of a certain person is greater than or equal to all the numbers of residences that all the people present in the Residence table have or have had.

On the other hand, we could try to implement the query without using the >= operator and the ALL modifier, and instead use only a non-correlated subquery and the aggregation function MAX().

SELECT
  R.PersonFK   AS PersonID,
  COUNT(*)     AS NumResidences
FROM Residence AS R
GROUP BY R.PersonFK
HAVING COUNT(*) = (
  SELECT MAX(COUNT(*))
  FROM Residence
  GROUP BY PersonFK
);
As you can see above, the query construction is very similar, except that in the HAVING clause, we directly compare COUNT(*), which returns the number of residences a person has or has had with the result of the subquery, which seems to obtain the maximum number of residences any person has had.

But if we look at the SELECT clause of the subquery, several nested aggregation functions like MAX(COUNT(*)) appear, intending to calculate the maximum value of the numbers of residences people have had. But this is not allowed in SQL. In fact, if we run the query, the DBMS will give us an error because an aggregation function can’t be used as an input argument to another aggregation function.

If we really want to use the aggregation function MAX() to solve the query, we have no choice but to first build a CTE where we store all the people who have ever had a residence and their respective number of residences.

You can see this in the code below, and it’s very similar to the approach we followed before to solve the query. This involves grouping the residence tuples by their foreign key attribute PersonFK and using COUNT(*) to count how many tuples each group has, that is, how many residences each person has.

WITH ResCount AS (
    SELECT PersonFK AS PersonID, COUNT(*) AS NumResidences
    FROM Residence
    GROUP BY PersonFK
)
SELECT RC.PersonID,
    RC.NumResidences
FROM ResCount RC
WHERE RC.NumResidences = (
        SELECT MAX(NumResidences)
        FROM ResCount
    );
Then, once this intermediate table ResCount is built, we are in the same situation as in the queries at the beginning of this section, where the numbers of residences are now values stored in an attribute.

So we can follow the usual approach to get the tuple or tuples from ResCount with the maximum value in their attribute NumResidences. This involves going through all its tuples and checking if their NumResidences value matches the maximum. We can easily calculate this with a non-correlated subquery and the aggregation function MAX().

After these queries, we can consider solving them by obtaining the element with the lowest value in its metric in the ranking.

For example, in this last case, it would correspond to finding the person or people who have had the fewest residences (which doesn't make much sense in this query, but it does in others).

So, to calculate minimums instead of maximums in SQL, you use exactly the same constructions we just saw, with the difference that the operators and aggregation functions used change, such as the operator >= to <= and the aggregation function MIN() is used instead of MAX().

In addition to calculating maximums and minimums, in SQL it's sometimes useful to calculate the ranking positions of elements based on the value of their metrics.

SELECT
  P1.PoolID,
  P1.Name,
  P1.MaxDepth,
  (
    SELECT COUNT(*) + 1
    FROM Pool AS P2
    WHERE P2.MaxDepth > P1.MaxDepth
  ) AS DepthRank
FROM Pool AS P1
ORDER BY DepthRank;
For example, in the query above, we get a list of all the pools in the database, where for each one, we calculate its position in the pool ranking ordered by the value of its MaxDepth attribute, that is, by its maximum depth.

Also, since there can be multiple pools with the same MaxDepth value, in that case, both pools will have the same position in the ranking. So the next position with a lower MaxDepth value won’t be the immediate next position – instead, you must add the number of pools from the previous position that had the same MaxDepth value to that ranking position.

PoolID	Name	MaxDepth	DepthRank
1	Sample Pool Name 1	5	1
2	Sample Pool Name 2	5	1
3	Sample Pool Name 3	3	3
4	Sample Pool Name 4	2	4
5	Sample Pool Name 5	2	4
To understand this, here we have a table where you can see that the first two pools have the same position (DepthRank) in the pool ranking because they have the same MaxDepth value. Then, the next pool with PoolID=3 has position 3 in the ranking, as there are two pools before it in the ranking. Finally, the next two pools with PoolID=4 and PoolID=5 again have the same position in the ranking for the same reason as before.

As we can see, this way of defining and building the ranking is not what we might expect, where each pool has a unique position. Instead, we slightly modify the ranking definition to allow pools with the same MaxDepth value to share the same position in the ranking, so SQL implementation doesn't require more advanced functions.

Regarding the implementation, if we look at the attributes of the example table, specifically MaxDepth and its relationship with DepthRank, we can conclude that the position we should assign to each pool in the ranking matches the number of pools with a MaxDepth strictly greater than its own plus 1.

For example, for the pool with PoolID=2, we see that there is no pool with a MaxDepth greater than its own – at most, there are some with an equivalent MaxDepth, but never greater because this pool has the highest MaxDepth value (meaning the maximum). Meanwhile, the pool with PoolID=3 has two pools with a MaxDepth greater than its own.

So if we add one to the number of pools with a metric value, which in this case we can find in the MaxDepth attribute, greater than the MaxDepth value of a certain pool, then the amount we obtain is the ranking position of that pool.

The simplest way to implement this calculation in SQL is through a correlated subquery in the SELECT, where, as you can see, we get all Pool tuples with a MaxDepth greater than the pool we are iterating over in the query. And finally, with COUNT(*)+1, we add 1 to the number of tuples returned by the subquery, thus generating the position in the ranking of the pool being iterated over in the query.

Continuing with the idea of getting the ranking position of the elements, we also have the option to select only those elements with a ranking position greater or less than a certain amount we need to set.

SELECT PoolID, Name, MaxDepth
FROM Pool AS P
WHERE (
        SELECT COUNT(*)
        FROM Pool AS P2
        WHERE P2.MaxDepth > P.MaxDepth
    ) < 5
ORDER BY MaxDepth DESC;
For example, above we have a query where we get the pools that are among the top 5 distinct positions in the ranking. In other words, we don’t get the first 5 rows with pools ordered in the ranking according to their MaxDepth value, but we get all those whose ranking position is among the top 5 distinct positions.

As you can see, the implementation is simple. We go through all the Pool tuples and for each one, we execute a subquery like the one we saw in the previous query: it gets the number of pools with a MaxDepth greater than the pool we are iterating over – that is, its position in the ranking. Then, we compare that number with 5 to ensure it’s strictly less.

Also, here we need to note that we have not added 1 to COUNT(*), which means the ranking starts counting at position 0, not 1, so we can later check that the position is among the top 5 distinct ones with < 5 and not < 6. This doesn't have to be done this way necessarily, as we could have added 1 to COUNT(*) and declared the comparison using <6, or <=5.

In summary, in this query, we used a correlated subquery to get the ranking position (starting from position 0) of each pool, so we only keep those whose position is strictly less than 5. But we could have also pre-calculated the positions of each pool in a CTE and then applied this condition to an attribute instead of the value returned by a subquery.

This alternative will likely use more memory than is necessary, since the computing the execution of the subquery that calculates the ranking position will be present whether we use a CTE or not. So the most optimal approach would be to avoid wasting memory unless we really need an intermediate table with that information for other uses.

So now we’ve have seen a series of queries that follow certain patterns that are the most basic and fundamental in SQL. But there are many other queries we could perform on the schema of this example with a wide variety of purposes. These are essential to know how to formulate and code.

To learn more queries, you can visit the following resource: PostgreSQL.ipynb.

https://github.com/cardstdani/sql-storage/blob/main/PostgreSQL.ipynb

This is a Jupyter notebook that you can run from Google Collab. It contains Python code and Bash commands that allow you to install the PostgreSQL DBMS on a Linux virtual machine like those used by Google Compute Engine (the backend of Google Collab). You can also execute SQL code to create the database from the DDL and then run queries and obtain their results.

The notebook contains a series of query statements with solutions, along with everything needed to execute them. These queries aren’t ordered or classified like those we saw in the last chapter, as the goal is for you to try to solve them from the statements without looking at the solution. This way, you can later see how they were solved and gain practice in formulating queries, which is one of the most valuable skills for providing services to end users from the database.

You don’t necessarily have to do this in a Google Collab environment – you can also do it on a PostgreSQL installation on a local machine and execute the queries by copying and pasting the query code into the PostgreSQL terminal. But doing it in a remote environment like the one offered by Google Collab has certain advantages, such as not having to worry about installing anything manually, as everything is set up automatically by simply running the code cells or being able to see the text of the statements in the notebook rendered with markdown.

Still, there are some disadvantages, such as the database being stored on a Google virtual machine, which means you don't have full control over the machine and environment in which the DBMS runs. Its execution can also be interrupted depending on how you use the virtual machine and the plan you have with Google Collab.

So even though it may not be an environment where you can deploy a fully functional production database, it’s sufficiently similar to a real environment where you might have a database deployed for a project, making it worthwhile to work in Google Collab.

Conclusion
In this book, we’ve covered all the key concepts you need to know to design a database, based on certain requirements, for a software project.

But again, these concepts and commands are only the most basic and fundamental ones. So to learn more about SQL database design, check out other resources as well like reference books, articles, or the many resources available on the internet.

Your goal should be to gain a deeper understanding of what you’ve learned here. This will help you design robust DBs according to client requirements and code even more efficient queries.

Thank you for reading!