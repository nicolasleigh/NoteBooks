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

---

### Schemaless database

Schemaless databases mean there is no predefined schema the data must conform to before it’s added to the database. As a result, you don’t need to know the structure of your data, enabling you to store all your data easily and quickly.

Document databases are sometimes called schemaless, but that’s misleading, as the code that reads the data usually assumes some kind of structure—i.e., there is an implicit schema, but it is not enforced by the database.

A more accurate term is schema-on-read (the structure of the data is implicit, and only interpreted when the data is read), in contrast with schema-on-write (the traditional approach of relational databases, where the schema is explicit and the database ensures all written data con‐forms to it).

> [What is a Schemaless Database?](https://www.mongodb.com/resources/basics/unstructured-data/schemaless)

> DDIA - P61

---

## Query Languages for Data

### Declarative query language vs. imperative language

Imperative query languages are used to describe how you want something done specifically. This is accomplished with explicit control in a detailed, step-by step manner; the sequence and wording of each line of code plays a critical role. Some well-known general imperative programming languages include Python, C and Java.

Declarative query languages let users express what data to retrieve, letting the engine underneath take care of seamlessly retrieving it. They function in a more general manner and involve giving broad instructions about what task is to be completed, rather than the specifics on how to complete it. They deal with the results rather than the process, focusing less on the finer details of each task. Some well-known general declarative programming languages include Ruby, R and Haskell. SQL is a declarative query language and is the industry standard for relational databases.

> [Imperative vs. Declarative Query Languages: What’s the Difference?](https://neo4j.com/blog/imperative-vs-declarative-query-languages/)

> DDIA - P64

---

# Storage and Retrieval

### Log-Structured Storage Engines

You can categorize most of the databases in the market today in 2 families of storage engines:

1. Log-structured storage engines.
2. Page oriented storage engines.

Log-structured storage engine indexes the data in 2 data structures:

1. An in-memory balanced tree data structure, also known as memtable. e.g. AVL trees, Red-black trees. You can keep adding the keys to this data structure and read them back in a sorted manner.
2. And, a collection of storage segments , also known as Sorted String Table or SSTable in short. Each SSTable is a sorted collection of key-value pair.

A few properties of SSTables are:

1. Keys are sorted in a particular segment.
2. A particular key can only appear once in a particular segment.
3. A key can be repeated in multiple segments but has to be unique in a particular segment.
4. The size SSTables depends on the number of keys contained within it and thus, different SSTables can have different sizes.

Since, the writes are performed in append only fashion, which results in sequential write operation on disk, thus, are much faster than the random writes. Therefore, Log-structured storage engines using LSM-Tree(Log-Structured Merge-Tree) as index are optimized and better suited choice for write-heavy applications. The databases which use Log-structured storage engine includes Bitcask, LevelDB, Google BigTable, RockDB, Cassandra.

> [Log-Structured Storage Engines](https://medium.com/@pnk.tanwar/log-structured-storage-engines-a0c6e78273c)

> DDIA - P92-P101

---

#### Bitcask

Bitcask offers high-performance reads and writes, subject to the requirement that all the keys fit in the available RAM, since the hash map is kept completely in memory. The values can use more space than there is available memory, since they can be loaded from disk with just one disk seek. If that part of the data file is already in the filesystem cache, a read doesn’t require any disk I/O at all.

A storage engine like Bitcask is well suited to situations where the value for each key is updated frequently. For example, the key might be the URL of a cat video, and the value might be the number of times it has been played (incremented every time someone hits the play button). In this kind of workload, there are a lot of writes, but there are not too many distinct keys—you have a large number of writes per key, but it’s feasible to keep all keys in memory.

If the database is restarted, the in-memory hash maps are lost. Bitcask speeds up recovery by storing a snapshot of each segment’s hash map on disk, which can be loaded into mem‐ory more quickly.

The database may crash at any time, including halfway through appending a record to the log. Bitcask files include checksums, allowing such corrupted parts of the log to be detected and ignored.

#### The limitations of the hash table index

- The hash table must fit in memory, so if you have a very large number of keys, you’re out of luck.

- Range queries are not efficient. For example, you cannot easily scan over all keys between kitty00000 and kitty99999—you’d have to look up each key individually in the hash maps.
