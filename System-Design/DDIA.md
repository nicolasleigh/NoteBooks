# Reliable, Scalable, and Maintainable Applications

## Thinking About Data Systems

### Reliability

The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or soft‐ware faults, and even human error).

### Scalability

As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.

### Maintainability

Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.

## Reliability

Reliability means making systems work correctly, even when faults occur. Faults can be in hardware (typically random and uncorrelated), software (bugs are typically systematic and hard to deal with), and humans (who inevitably make mistakes from time to time). Fault-tolerance techniques can hide certain types of faults from the end user.

## Scalability

### Describing Load

Load can be described with a few numbers which we call **load parameters**. The best choice of parameters depends on the architecture of your system: it may be requests per second to a web server, the read/write ratio in a database, the number of simultaneously active users in a chat room, the hit rate on a cache, or something else. Perhaps the average case is what matters for you, or perhaps your bottleneck is dominated by a small number of extreme cases.

### Describing Performance

In a batch processing system such as Hadoop, we usually care about **throughput**—the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size. In online systems, what’s usually more important is the service’s **response time**—that is, the time between a client sending a request and receiving a response.

In practice, in a system handling a variety of requests, the response time can vary a lot. We therefore need to think of response time not as a single number, but as a **distribution** of values that you can measure.

## Maintainability

### Three design principles for software systems:

1. Operability: Make it easy for operations teams to keep the system running smoothly.

2. Simplicity: Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system. (Note this is not the same as simplicity of the user interface.)

3. Evolvability: Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as extensibility, modifiability, or plasticity.

# Data Models and Query Languages

## The Object-Relational Mismatch

### Impedance mismatch

Impedance mismatch is the term used to refer to the problems that occurs due to differences between the database model and the programming language model.

To address the impedance mismatch problem, developers often use object-relational mapping (ORM) tools or other middleware solutions that provide a bridge between the OOP model and the relational model. These tools can automate the mapping process, allowing developers to work with objects in code while transparently interacting with the underlying database.

Object-relational mapping (ORM) frameworks like ActiveRecord and Hibernate have been developed to bridge the gap between the object-oriented and relational worlds. These frameworks provide a layer of abstraction that allows developers to work with objects in their code, while the ORM library translates that to the appropriate SQL queries to fetch or store those objects in the database.

> [Impedance Mismatch in DBMS](https://www.geeksforgeeks.org/impedance-mismatch-in-dbms/)

> DDIA - P52

---

## Many-to-One and Many-to-Many Relationships

### Normalization

Normalization is the process of minimizing redundancy from a relation or set of relations. Normal forms are used to eliminate or reduce redundancy in database tables. You can design the database to follow any of the types of normalization such as 1NF, 2NF, and 3NF.

#### The First Normal Form – 1NF

For a table to be in the first normal form, it must meet the following criteria:

- a single cell must not hold more than one value (atomicity)
- there must be a primary key for identification
- no duplicated rows or columns
- each column must have only one value for each row in the table

#### The Second Normal Form – 2NF

The 1NF only eliminates repeating groups, not redundancy. That’s why there is 2NF.

A table is said to be in 2NF if it meets the following criteria:

- it’s already in 1NF
- has no partial dependency. That is, all non-key attributes are fully dependent on a primary key.

#### The Third Normal Form – 3NF

When a table is in 2NF, it eliminates repeating groups and redundancy, but it does not eliminate transitive partial dependency.

This means a non-prime attribute (an attribute that is not part of the candidate’s key) is dependent on another non-prime attribute. This is what the third normal form (3NF) eliminates.

So, for a table to be in 3NF, it must:

- be in 2NF
- have no transitive partial dependency.

> [Database Normalization – Normal Forms 1nf 2nf 3nf Table Examples](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

---

## Relational Versus Document Databases Today

### The difference between relational and document databases

- **Relational databases** are based on the relational model of data, which organizes data into one or more tables (or “relations”) of columns and rows. Each table has a schema that describes the columns, and each row in the table has a value for each column. The schema defines the tables, the fields in each table, and the relationships between fields and tables.

- **Document databases** are a type of NoSQL database that use a denormalized data model, which is designed to store, query, and retrieve data as JSON-like documents. Each document is a collection of key-value pairs, and each key is a field in the document. Document databases are designed to store and manage collections of JSON-like documents.
