# Databases

Data are everywhere nowadays, and in the few years we generated more data then all of the past years _combined_.
Without a database we couldn't store this massive amount of data.

> A database is a collection of data, a method for accessing and manipulating that data.

They're just hardware and software. Nothing really special. At the end of the day, a database is just a container of data in the hard disk, but in a way that are more understandable by the computer and also by humans thanks to DBMS softwares, faster to access and easier to manage.

There are a lot of database softwares out there, and each of them has its pros and cons.

Right now we can just focus on three main things about databases:

1. How to put data in DB
2. How to use/updated/learn from data
3. How to remove data

But first, let's take a quick lookup at the most common acronyms:

- **DBMS** - DataBase Management System
- **RDBMS** - Relational Database Management System.
- **SQL** - Structured Query Language

Fun Fact:
Have you ever wondered why databases are always represented by a cylinder?

Me neither.

Anyway, that's just a representation of a considerably older technology: drum memory:

![Drum Memory](https://i.stack.imgur.com/BHNxf.jpg)

Hope it answers the question :D

Ok, now let's continue...

## Types of databases

There are actually 5 main types of databases:

1. Relational model (e.g. MySQL or PostgreSQL)
2. Document model (e.g MongoDB)
3. Key value databases (e.g. Redis)
4. Graph database (e.g. GraphQL)
5. Wide columnar models (e.g. Apache or Cassandra)

## What is a database

A database is a structured set of data that is structured in a certain way to scale with organizations that have massive amounts of data.

File Processing Systems, limitations on relationships between data.

## Database models

Just to mention a few database models:

* **Hierarchical**
* **Netwworking**
* **Entity-Relationship**
* **Relational**
* **Object Oriented**
* **Flat**
* **Semi-Structured**
* ...

All of these models are designed with specific rules. The most used right now is the relational one.
The Hirarchical model was the first one actually adopted, which was in a tree-like structure. Data used to be represented in XML files. The problem with XML files is that it could only represent a one-to-many relationship (or less) because of it's structure (which for who don't know what, is HTML-like).

The relational model came up to solve this problem, and the logic for the relationships between data are defined by the database sofware.

## CRUD

DB operations are generalized with the acronym **CRUD**, which stands for:

* Create
* Read
* Update
* Delete

Each operation has specific rules defined by the DBMS.

## Codd's 12 Rules

**Edgar Frank Codd** was a computer scientist working for IBM, and he was the person who invented the relational model for database management (theoretical basis for relational databases).
Codd proposed thirteen rules (numbered zero to twelve) and said that if a Database Management System (DBMS) meets these rules, it can be called as a Relational Database Management System. These rules are called **[Codd's 12 rules](https://www.w3resource.com/sql/sql-basic/codd-12-rule-relation.php)**.

## Relational Model

Each relational model and DBMS follow specific "terms":

* Tables
* Columns
* Degrees
* Domains
* Attrubutes
* Tuple
* Cardinality
* Relation Schema
* Relation Key
* Relation Instance

Let's analyze each term.

What is a **table**?
A Table is a _representation of an object_ (e.g User). Each table has a _name_, and refers to the concept of the data we are going to store.
Each table has specific **Columns** (e.g. firstName, lastName, ecc...).
Next to the column we have rows, which represent the pieces of data.

Each column has a name, and store a specific data. We call a **Degree** the collection of all columns in a table.
When we talk about what a column can store, we talk about **Domains**/constraints: the "domain" of the data, the "domain" of the column. Another way of talking about columns is saying something like: "My column has this **Attributes**".

Another word for rows is **Tuples**, which is nothing more than a single record of data. Each tuple follow the columns contraints.
When we talk about the collection of rows (just like "degrees" represent the collection of columns), we should refer to them as **Cardinality**.

What makes the relational model so special?
The answer is the ability to link relationships between different type of data, with a **primary key** and a **foreign key** from two different tables. A primary key is a unique identifier that can identify a single record (row/tuple).
We can define the relationship simply by adding a new column to a table that only contains foreign keys which are connected to the primary keys of another table.

There are two different uses that are distinctly grouped for reational databases:

1. Support Day to Day operations of a company
2. Support Analysis of a company

We call the Day To Day Transactions the **OLTP** (On Line Transactions Processing).
The second type is called **OLAP** (On Line Analytical Processing).

## SQL

This little file it's not an SQL guide/tutorial, so I'll not explain all the single SQL commands, concepts, and so on.
But, I'll try to only give generic informations that I found useful.

### SQL Commands

When talking about SQL commands, you can hear different acronyms which groups different commands:

* **DCL** - Data Control Language, which are the following commands:
  * GRANT
  * REVOKE
* **DDL** - Data Definition Language, which are the following commands:
  * CREATE
  * ALTER
  * DROP
  * RENAME
  * TRUNCATE
  * COMMENT
* **DQL** - Data Query Language, which are the following commands:
  * SELECT
* **DML** - Data Modification Language, which are the following commands:
  * INSERT
  * UPDATE
  * DELETE
  * MERGE
  * CALL
  * EXPLAIN PLAN
  * LOCK TABLE

### Functions

A language like SQL offer a different set of functions (just like SUM, COUNT, CONCAT, etc...).
There are actually 2 different types of functions:

* **Aggregate** - Run against all of the data and produce one output: it operate on many record to procude 1 value (e.g. SUM, AVG, COUNT, MIN, MAX).
* **Scalar** - Run against each individual row (e.g. CONCAT).

### Dates

**UTC** is the common standard for dates, but it's not set by default in your environment. You can do that by changing the default `TIMEZONE` of your user, with this command:

```sql
ALTER USER postgres SET timezone='UTC'
```

Also, In PostgreSQL dates are show with the **ISO-8601** format standard, like this:

```
YYYY-MM-DDTHH:mm:ss
2022-12-29T12:47:16+02:00
```

Anoooother way of representing dates, are **Timestamps**, which is a date with time, and potentially a timezone information.

Little example:

```sql
CREATE TABLE timezones (
  ts TIMESTAMP WITHOUT TIME ZONE,
  tz TIMESTAMP WITH TIME ZONE
)

INSERT INTO timezones VALUES (
  TIMESTAMP WITHOUT TIME ZONE '2000-01-01 10:00:00-05',
  TIMESTAMP WITH TIME ZONE '2000-01-01 10:00:00-05'
)

SELECT * FROM timezones
/* 
Will return
ts = 2000-01-01 10:00:00
tz = 2000-01-01 15:00:00+00
*/
```

## Indexes

The purpose of indexes is to speed up queries by telling the syste that a specific column(s) is "important", and must be easy to access. For example, if you constanly search a user by its username, it's common to set a unique index for the username column, so that the query to find that specific field is much much faster. The drawback with indexes, is that they occupy more space in memory, so you really have to consider when to use them.
The `index` concept is not only an SQL concept, but it's shared among different (or maybe all) types of databases.

Suggestions:

* Don't add an index just to add an index.
* Don't index on small tables.
* Don't use indexes on tables that are updated frequently (the system woul have to constantly update indexes).
* Don't use on columns that can contain NULL values.
* Don't use on columns that have large values.

Indexes can be:

* **Single column**
* **Multi-column**
* **Unique**
* **Partial**
* **Implicit Indexes**

Implicit indexes are simply indexes that the system automatically generates for important columns (like _IDs_, _Primary Keys_ and _Foreign Keys_).

## Database Organization

Databases often contains many tables, view, etc...
Depending on how much you care, you may want to organize them in a logical way. Most databases offer the concept of "**Schemas**", which represent a box where you can organize things. By default each database (in PostgreSQL) gets a "public" schema, unsless you specify a schema. For example, if you normally created a table, you can access it in these ways:

```sql
SELECT * FROM table
SELECT * FROM "public".table
```

The reasons to use a schema are the following:

1. To allow users to use one database without interfering with each other.
2. To organize database objects into logical groups to make them more manageable.
3. Third-party applications can be put into separate schemas so they do not collide with the names of other objects.

### Roles

In every database you should create specific roles for other users, so that they are only allowed to perform specific actions. For example, you should not let all users create databases.
A role can be a user or a group. It depends on how you setup the role.
Roles have the ability to grant membership to another role.

In PostgreSQL, you can create a role like this (example):

```sql
CREATE ROLE readonly WITH LOGIN ENCRYPTED PASSWORD FOR 'readonly';
```

When managing roles and permissions, always go with the "_principle of least privilege_": give no permissions at all, and only start to give privileges whne stictly needed.

## UUIDs

UUIDs (Universal Unique IDentifiers) are a way to identify data. They are usualli better than storing simple numbers (1, 2, 3, ...) as ID.
Here's the PROS and CONS:

**PROS**:

* Unique everywhere
* It's easier to shard
* It's easier to merge/replicate
* You expose less information about your system

**CONS**:

* Larger Values to store
* Can have performance impact
* More difficult to debug

## Backups

Databases contains data, so they are really, really, important. We wanto to be sure that if a disaster happens, we have a recent copy of all the data.
That's why we want a **backup**. Safety first.

> Disaster will Strike!

When something happens, we need a strategy to restore the database.
How you store and manage the data is **Important**.
There are 3 thing you need when thinking about a backup:

1. Backup plan
2. Disater recovery plan
3. Test your plans

Anyway, what can technicall go wrong? This are just a few of possible scenarios:

* Hardware failures
* Viruses
* Power outages
* Hackers
* Human Error

How do I make a backup plan?

1. Determine what need to be backed up
2. Determine backup strategy, based on the speed of restoring and the importance of the data that you have. Stategies can can be:
  * **Full Backup**
  * **Incremental** - First needs a full backup, and change
  * **Differential** - Check what changed since the last full backup
  * **Transaction log** - Log every command executed to create/change/delete the data, so that you can re-execute all commands.
3. Determine what's the appropriate way to backup.
4. Determine how frequently you're going to backup.
5. Decide where you're going to store the backup.
6. Have a retention policy for the backup.

### Backup stategies

Here's a table of the backup strategies, which we already covered, but it's nice to have all here:

| TYPE | PURPOSE | FREQUENCY |
| ---- | ------- | --------- |
| Full backup | Bacckup all the data |  Less often |
| Incremental | Backup since last incremental | Often |
| Differential | Backup since last full backup | Often |
| Transaction log | Backup of the database transactions | A lot |

## Transaction

**Transactions** are a way to restore the database in a previouse state if an operation fails. They are used when operating on multiple rows/tables, because we wanto to be sure that if an operation fails, the others must stop.

A transaction is useful when deleting a row and all the data that references it. So, for example, if I delete a `Course`, and all the `Enrollments` connected, if the course is sucesfully delelted, but then along the way the deletion of an enrollment fails, a transaction allows us to restore the state of the database just like it was before starting the transaction. So, the deleted course and all the deleted enrollments will be restored.

Ok, Let's get technical.

Many many users can access the database concurrently. Transactions are a unit of instructions that run in isolation.

> The concept of transactions is to keep things **consistent**.

The flow of a transaction is the following:

```
Begin -> Active -> Partially committed -> Committed -> End

Or

Begin -> Active -> [Partially committed] -> Failed -> Aborted -> End
```

To maintain the integrity of a database, all transactions must obey ACID properties (See file [06-System-Design-Architecture#databases](./06-System-Design-Architecture.md#databases) for more).

## SDLC - Database Design

Database design is one of the main part about system design.
But first, we want to see the overall principles that go into the system design and to do that we have to look at something called the **SDLC** (Software Development Life Cycle), which go in 4 phases:

1. System planning and Selection
2. System Analysis
3. System design
4. System Implementation and Operation

It's a _life cycle_ because a system is never completely finished

> The goal of this process is to design robust systems!

The process of a SDLC can be implemented in different ways: **Agile** (most common in software development, small changes, continuosly, based on the necessity), **Waterfall** (for industries heavily regulated), **V-Model** (more V-shaped, down and then up again), ...

Anyway, every way must follow the SDLC phases. The only difference is that they cycle through those steps at difference paces.

Let's see in details those phases.

### SDLC Phases

* **Phase 1**: This entire phase is about getting the information on what needs to be done (scope)
* **Phase 2**: Taking the requirements and analyze if it can be done on time and on budget.
* **Phase 3**: Designing the system architecture for all related components: Databases, Apps, etc.
* **Phase 4**: Building the software.

There are also more phases that can be added, just like testing, maintenance, and so on, but this 4 are the main one.

What we are going to focus right now, is the phase 3, specifically for databases, because our goal is to build a robust database structure.

### System Design 

Before you're going to implement a database, you need to design it, otherwise it will become really difficult to built it without a concrete _abstraction_ on a paper.

> From chaos, to structure - System design is all about creating a structure that can be understood and communicated.

In any system, there is data, and in every system these data are vital to the operations of that system. When wh design the database, we want to reflect the requirements that that system has.

There are different techniques to design a database, that help us formulate a way to structure our data to accomplish the system requirements. There are two very popular ways to design a database: **Top-Down** and **Bottom-Up**.

### Top-Down VS Bottom-Up

When you look at **Top-Down**, you're starting from zero: no existing structure, no existing data. So, it's the optimal choice when creating a new database. All requirements are gathered up-front. You have to build the _universe_ that satisfy the system's requirements.

**Bottom-Up** is used when there is an existing system or specific data in place. So, you want to shape a new system around the existing data. This is the optimal choice when migrating an existing database.

Often, you'll end up using a bit of both.

When building a universe (top-down approach), you first have to identify the objects that compose it.

The goal is to create a data model based on the requirements, which can be:

* High-level requirements
* User interviews
* Data collection
* Deep Understanding

One of the key methods on top-down design is the ER-Modelling.

### Top-down design

#### ER Model

An **ER Model** (or ER diagram) (Entity-Relationships Modelling) is a way of top-down design. It's a diagram that functions as a way to strucutre high-level requirements. With this diagram we can define in an undertstandable way our shape of the universe by defining all the objects (Entities or Tables) that compose it and what type of relationships each entity has with another one.

So, the first step when building an ER diagram, is _determining the entities in the system_. Thinkinig about breaking down a concept into defined entities can be really hard, so, to help, let's think about what is an entity. An entity:

* Is a person, place or thing
* Has a singular name
* Has an identifier
* Should contain more than one instace of data

There are various tools for building this kind of diagrams, but I suggest using **[LucidChart](https://www.lucidchart.com/)**.

An example of a simple ER Diagram is the following:

![LucidChart ER](https://d2slcw3kip6qmk.cloudfront.net/marketing/pages/consideration-page/ERD/Database-ER-Diagram-Example.jpeg)

As you can see, each entity has also some attributes.
In fact, the second step when building an ER Diagram is defining the attributes of each entity.
Each attribute:

* Must be a property of the entity
* Must be atomic (e.g. Address is NOT atomic, becuase has street, number, postal code, and so on)
* Single/Multivalues (e.g. Phone number)
* Keys

The last point (keys), is used to determine the relationships between the entities. With these keys we can now finish our building of the universe by creating the relationships between every object.Relationships can be one to many (or zero), many (or zero) to one, one to one, or many to many.

For example, a Library can have only one location (which is an entity) (one to one relationship), but can have a lot of books. The same book can be present in multiple libraries. So, a book as a many to many relationship with the libraries (a library has many books, and a book can be present in many libraries).

Little note about many to many relationships:

> **_Always AVOID many to many relationships_**

Many to many relationships are painful for the system and for the brain, because it complicates a lot of things. Basically, you create more overhead:

* Insert Overhead
* Update Overhead
* Delete Overhead
* Potential Redundancy

### Bottom-Up Design

In bottom-up design we have two initial steps:

1. Identify the data (attributes)
2. Group them (entities)

So, unlike the top-down design, we do the reverse: first, attributes, and from the attributes we get the entities we need.

The goal is to create a "perfect" data model without redundancy (duplicate data) and anomalies.
Now, what are exactly anomalies?

#### Anomalies

Modification anomalies are problems that arise when your database is not structured correctly.
There are 3 types of anomalies:

* Update anomalies
* Insert anomalies
* Delete anomalies

You always want to avoid anomalies, in every system.
For example, in this table:

| customer | balance | branch | address |
| -------- | ------- | ------ | ------- |
| 1 | 1000 | Toronto | 100 Front Steet |
| 2 | 1300 | New York | 5000 Younge Street |
| 3 | 990 | Scarborough | 20 beach street |
| 4 | 540 | Toronto | 100 Front Steet |
| 5 | 200 | Toronto | 100 Front Steet |

* Update anomaly: If we have 1 million customers, and want to update the address on rows with a branch = Toronto, we have to do 1 million operations.
* Delete anomaly: If we delete a record, we loose information about a branch
* Insert anomaly: If we make a mistake on adding a branch equals to Toronto (we type Torontola), how do we make sure it's wrong? We can't.

In those cases, data are not **Consistent**.

We want data to be low coupled. Each entity must have ONLY the data strictly needed by itself.

So, in the example above, a solution would be to separate the branch and the address in a different table, and make a customer reference to a record in the branch table.

This type of modification to the data is called **Normalization**.

#### Normalization

Normalization is a design technique to reduce redundancy and anomalies. To do it we must understand two things:

* **Functional dependencies**
* **Normal forms**

##### Functional dependencies

Functional dependencies shows a relationship between attributes (inside the table). It exists when you can uniquely determine the corresponding attribute's value (col B functionally depends on col. A.):

```
Determinant ---> Dependant
student_id ---> birth_date
```

Can't be the reverse.

##### Normal Forms

There are actually 8 (0-1-2-3-BCNF-4-5-6) normal forms, which you should implement gradually. Each normal form aims to further separate relationships into smaller instances as to create less redundancy and anomalies. BCNF stands for "Boyce-Codd Normal form", and it's also referred as 3.5. 0 to BCNF are the most common normal forms you'll see, while the 4 and 5 are there to further reduce anomalies. The 6th is not yet standardized.
The ideal Normal form is the BCNF, while the 4th and 5th are used in specific scenarios. Let's see each normal form.

###### 0NF

Data is unnormalized:

1. Repeating groups of fields (two+ cols. that are closely related).
2. Positional dependence of data
3. Non-atomic data

```
branch
first_name
last_name
title
hours
```

###### 1NF

1. Eliminate repeating columns of the same data
2. Each attribute should contain a single value
3. Determine a primary key

```
EMPLOYEE
emp_no
first_name
last_name
title

BRANCH
branch_no
street
street_no
province
postal_code
emp_no
hours_logged
work_date
country
```

###### 2NF

1. It is in 1NF.
2. All non-key attributes are fully [functional dependent](#functional-dependencies) on the primary key.

```
EMPLOYEE
emp_no
first_name
last_name
title

BRANCH
branch_no
street
street_no
province
postal_code
country

TIMESHEET
branch_no
emp_no
hours_logged
work_date
```

###### 3NF

1. It is in 2NF
2. No transitive dependencies (A -> B and B -> C, so C is transitively dependent on A via B) (-> = functionally dependent)

```
EMPLOYEE
emp_no
first_name
last_name
title

BRANCH
branch_no
street
street_no
country_id
postal_code

TIMESHEET
branch_no
emp_no
hours_logged
work_date

COUNTRY
country_id
country
province
```

###### BCNF

1. It is in 3NF
2. For any dependecy A -> B, A should be a super key.

3NF allows attributes to be part of a candidate key that is not the primary key. _BCNF does not_.

A relationship is not in **BCNF** if:

1. The primary key is a composite key
2. There is more than one candidate key
3. Some attributes have keys in common

###### 4th, 5th, and 6th Normal Forms

4th, 5th and 6th (which is not yet standardized) are generally not used. Its purpose is of course to avoid the things we said before: anomalies. But, they went just a litlle too far, and working with this normal forms is unpractical. Generally, this normal forms are used for really important data, such as health data.

## Database Landscaoe

### Scalability

Scalability is all about the capability of the database to handle the heavy amount of data. We can scale a database in two ways:

* **Vertical**: Adding more capacity to a single machine. Add physical hardware. It's the easiest one, but it has limits.
* **Horizontal**: Add more machines. Add more databases. They talk together. It's more flexible, but harder to implement.

### Sharding

Sharding is a way to handle the huge amount of data we have by splitting those data into more different databases. You have to implement a "routing" system so that each request access the correct database. That's why it can become really complicated, but it's really efficient.

### Replication

Replication is a method of replicating data across different machines to prevent disasters. So, if something happens to the original data and it get corrupted, we still have a secound source of data.

Can be implemented in two ways:

* **Synchronously**: The server does not send a response until the DB replicate the operations in all the replications. So, it takes more time to get a response.
* **Asynchronously**: The server do the "aligning" operations in the background, so the client immediately receives the response. But, if there is an error, data can be not aligned anymore.

### Backups

Backups and replication are similar, but are not the same thing. Replication happens usually in real time. Backups, are not done as often. Commonly, they are done every night, or even after more time.

A backup is done when creating a dump of your database.
Having a backup stategy in your company is a MUST.

### Distributed VS Centralized Databases

Centralized Databases are databases that are located in the same place, or they are managed by a single organization. For example, services like Amazon. You have a centralized place where you store your data (Amazon), so, if that "place" goes down, you loose all your data. But, having all centralized data, make their management a lot easier.

In Distributed, you store your data in different locations or organizations. Of course, this implies that you have to manage the alignment of your data.

### Security

I already covered security when talking about going from Junior to Senior developer.
Let's check the [security chapter](./03-Junior-to-Senior.md#security) of my notes.

## TOP Databases to Use

Ehehe. I know you were waiting for this. Here's the list:

* PostgreSQL
* MongoDB
* Amazon DocumentDB
* Firebase
* Elasticsearch
* Redis
* Amazon S3

## Conclusions

oooh boy.
Another chapter done.

Congrats 🤓.

I'm getting tired... You too, I know it.