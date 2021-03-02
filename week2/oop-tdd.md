# Object Oriented Programming / Test Driven Development

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Name 3 advantages to Test Driven Development

1. Encourages modular design as each piece is tested separately (and tests are drawn before program build).

2. Builds product documentation during programming process.

3. Isolates problems allowing for a faster development cycle.

## In what case would you need to use beforeEach() or afterEach() in a test suite?

You use ```beforeEach()``` when you need to set up a scenario to run tests against. For example, in the recent assignment, I added some data to the fake database model before each test in order to document how function calls on the database behaved (and how they affected existing data). ```afterEach()``` allows you to "tear down" or "reset" the preconditions between tests. This allows testing to truly be isolated in testing each component singularly and your test outcome will not be affected by the behavior or side effects from previous tests.

## What is one downside of Test Driven Development

To me the main discouraging factor is the up front time investment is pretty high (although the tradeoff is a less time spent on debugging and a more realistic idea of how the project is progressing throughout development).

## What’s the primary difference between ES6 Classes and Constructor/Prototype Classes?

ES6 classes allow for hierarchical inheritance, tight coupling of the child and parent class (and the child is inheriting everything from the parent, you have to be thoughtful about that in design to avoid always getting more than you need). Prototype classes can have singular inheritance and are themselves object instances (they don't have the type assignment like class objects do).

## Why REST?

The biggest benefit is that it allows for loose coupling between the client/server. What that means is, the client and the server only need to conform to HTTP protocols. The server can therefore change/update the exposed resources at will without affecting the functionality of the client/front end and the views can be adapted without affecting this communciation procedure as well.

Loose coupling allows for **scale**: many web apps can communicate with one another at will due to common communication protocols. 

Loose coupling also allows for **stateless client/server interactions** meaning each request/response has the complete set of information to fulfill the request and they do not build on previous requests.

## Document the following Vocabulary Terms

Term | Definition
---- | ----------
Functional Programming | A strategy where programs are built from applying and composing functions
Object-Oriented Programming (OOP) | A strategy where programs are based on "objects" that contain attributes and methods and interact with other objects; objects have notions of ```this``` or ```self``` and can update themselves
Class | A coded template that is extensible and can be used to generate objects conforming to it's specs
super | A function inside a child class that calls the constructor value of the parent class (necessary to instantiate the parent object values/methods and attribute them to the child)
this | A keyword used by an object to refer to itself; can be called to reference object attributes/methods
Test Driven Development (TDD) | A strategy where unit tests are written before the program is built to identify the exact desirable/expected behavior and help direct the designers to the minimal implementation that acheives the required outcomes
Jest | A JavaScript testing program that can be imported and used to create and run unit tests
Continuous Integration (CI) | 
REST | Representational State Transfer, refers to the WRRC cycle of HTTP requests characterized by statelessness and separation of client/server
Data Model | Data Structures/Models are explicit structures of how data is stored, organized and manipulated

## Reading Notes

** [Academind: MySQL vs Mongo (video)](#mysql-vs-mongo)

** [The Geek Stuff: NoSQL vs SQL Example](#nosql-vs-sql-example)

** [Highly Scalable Blog: NoSQL vs SQL](#nosql-vs-sql)

### [MySQL Vs Mongo](https://www.youtube.com/watch?v=ZS_kXvOeQ5Y)

SQL = structured query language, you build and access the database using these commands

- These are relational databases: support SQL language, works on principle of normalization and joins
- Tables are like data bins/storage containers with strict requirements on format/type
- All data must adhere to the table schema
- Normalized, distributed data

MongoDB = most popular NoSQL database

- Have "collections" in place of tables 
- Documents in the collections are similar to rows in the tables, BUT there is no schema implied on you so there can be differing formats
- Pros: less rigid solution; Cons: you're not guaranteed to have the properly formatted or complete data
- There are no built in relations in a NoSQL solution, instead the idea is to put all the information in one place
- Can be problems with having to update in multiple collections in NoSQL world

There is no clear "better" option, it depends on the project and the type of data/how it will be used.

Horizontal scaling means adding more servers; in the case of SQL this is harder than it may seem and may be an issue because Data cannot be split across multiple servers. Vertical Scaling (improving server capacity) also has a finite limit. NoSQL approaches are easily split horizontally due to the way the data is stored (no relations, stand alone collections, can split even one collection across multiple servers). At a very large scale, a huge volume of complex requests coming in will be computationally expensive.

Using NoSQL, you typically want to make just a few collections with a lot of nested information. Idea is to have minimal relations (if any). Has great preformance for simple read/write (unless the data exists in multiple tables and you have to update it in multiple areas).

### [NoSQL Vs SQL Example](https://www.thegeekstuff.com/2014/01/sql-vs-nosql-db/?utm_source=tuicool)

SQL | NoSQL
--- | -----
Relational databases | Not relational
Table based | Key-value pairs
Adhere to predefined schemas | Dynamic schema
Vertically scalable | Horizontally scalable (shallow)
Use SQL language | Use different syntax
Better for complex queries | Better for large data sets/heierarchical data
Scale by adding to single server | Distributable
Best for high volume, stable | Not as stable
Professional support available | More limited support
ACID (Atomicity, Consistency, Isolation, Durability) | Brewers CAP theorem (Consistency, Availability, Partition tolerance)

#### Popular SQL Databases

1. MySQL Community Edition: open source, free, well tested and reliable, connectors to many languages

2. MS-SQL Server Express Edition: good stability, support of Microsoft, cloud backup available, integrates with other Microsoft products

3. Oracle Express Edition: free for development and deployment, easily upgradable, wide platform support, scales well

#### Popular NoSQL Databases

1. MongoDB: stores in JSON like documents, non relational, very fast, scales well, flexible, becoming more popular and starting to be used by some big companies

2. CouchDB: document based NoSQL language (uses JSON objects), schema less, accessible through HTTP queries, automatic conflict detection, easy replication

3. Redis: open source, used mainly due to quick speed, provides efficient data structures, can use as a cache by implementing keys with limited time to live to improve performance, works with in memory dataset (that's why it's faster)

### [NoSQL Vs SQL](https://highlyscalable.wordpress.com/2012/03/01/nosql-data-modeling-techniques/)

SQL/relational model were developed to interact with end user, implying that

- end user is often interested in aggregate data (not single items)
- consistency, integrity, data type validity are well enforced

NoSQL models

- key:value system is simple, but powerful
- ordered key:value overcomes limitations of searching key ranges
- lacks value modeling, this is addressed with document databases

A key design difference is that SQL databases are driven by the design to best organize the available information; NoSQL databases are generally more driven by application specific patterns (how does the data need to be used?)

#### Conceptual Techniques for NoSQL

1. Denormalization

It's ok to repeat data into multiple documents

2. Aggregates

All major genres provide "soft schema" capabilites in some implementation

- Key/Value Storea and Graph Databases do not place constaints on values
- BigTable models support columns and versions as cells
- Document databases are inherently schema-less, although you can implement user defined schemas
- Soft schemas allow you to replace some joins with nested structures (embedding with denormalization)

3. Application side joins

Joins are rarely supported in NoSQL implementations, the workaround being to structure the data differently. In SQL joins are designed at query time, in NoSQL, joins are handled at the design time (how to initialize data structure).

4. Atomic Aggregates

Data is often aggregated into one object in NoSQL rather than normalized to minimize update locations.

5. Composite Keys, Other Nested Applications

The BigTable implementations are just making key/value type spreadsheets, you can use composite keys and then regex searches to have multidimensional keys within a since key. Tree aggregation inside of objects is another strategy. You can use flattening strategies and string parsing strategies in your data storage.