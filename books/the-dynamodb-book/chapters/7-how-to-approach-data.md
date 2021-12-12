# Chapter 7: How to approach data modeling in DynamoDB
This chapter covers the basics of data modeling in DynamoDB, starting with the differences between NoSQL databases and relational databases.

## 7.1 Differences with relational databases
It is a *bad* idea to model your data in DynamoDB the same way you model your data in a relational database. If the data is modeled in the same way it would be in a relational database it is difficult, if not impossible, to reap many of the benefits of using a NoSQL datastore.

### 7.1.1 Joins
The `JOIN` operator is a way for relational databases to connect data from different tables in a single query. This works well for relational databases, but it becomes incredibly inefficient at scale, and DynamoDB is built for scale. Often times people try to replicate this behavior in DynamoDB by querying a record and then querying all related records. This pattern should be *avoided* when using DynamoDB.

The key to structuring data in DynamoDB is to *preassemble data* in the exact shape that is needed for read operations.

### 7.1.2 Normalization
Normalization is essentially the "Don't Repeat Yourself" ("DRY") principle applied to data modeling practices, meaning that no data is replicated and that each piece of information is modeled independently. Normalization occurs on a spectrum from first normal form (1NF) to (6NF), but the most common are first, second and third normal forms.

#### 1NF
In 1NF all column values must be *atomic*, which means that they can only have *one* value, or that they cannot be a list.

#### 2NF
In 2NF each record much be *uniquely* identifiable by a key, which can be a single value or a combination of values. Practically this means that all non-key values must be defined based on the *whole* of the key, and not just a part of the key.

#### 3NF
In 3NF there are no transitive dependencies in the table, which means that all columns must be independent of each other.

#### Benefits of Normalization
There are two primary benefits to normalization.
The first is space efficiency. This is less prevalent these days as storage has becoming increasingly inexpensive.
The second is data integrity. In normalized databases only one record needs to be updated and all consumers pick up changes.

#### Why denormalize with DynamoDB
The primary benefit or normalization is no longer applicable as storage has become cheap and plentiful.
The second reason for normalization is still applicable and one to be considered when modeling data.

### 7.1.3 Multiple entity types per table
Since there is no `JOIN` operation in DynamoDB tables will often contain multiple entity types to facilitate fetching complex data from a single table. This has a few implications that will feel odd coming from a relational background:

1. The primary keys will contain different identifying information based on the entity type. This will likely result in primary keys that do *not* have descriptive names, opting instead of generic names like `PK` for a primary key or `SK` for a sort key.

2. The attributes in records will vary. Not every row will have every attribute, which should not be much of an issue for well-crafted application code, but will be rather difficult for browsing data in a console.

### 7.1.4 Filtering
In relational databases the `WHERE` clause reigns supreme and can be used to filter large amounts of data. However, this requires reading and discarding a lot of records, which is not sustainable at scale. In DynamoDB filtering is much more limited and built directly into the data model. The primary keys of the tables and the indices are intended to act as the primary filtering method and allow for precise, surgical requests to fetch the exact data needed.

## 7.2 Steps for Modeling with DynamoDB
Data modeling in DynamoDB is driven entirely by the access patterns. It is not possible to model data in a generic way that allows for flexible access in the future. The data *must* be shaped to fit the access patterns up front. At a high level, the steps for modeling data in DynamoDB are:

1. Understand the application
2. Create an entity-relationship diagram ("ERD")
3. Write out all of the access patterns
4. Model the primary key structure
5. Satisfy additional access patterns with secondary indices and streams

## 7.2.1 Create an entity-relationship diagram (ERD)
Creating an ERD gives a view into the different entities associated with an application and how they relate to each other. This can be done by:

1. List out each of the entities and their attributes
2. Describing how the entities relate to each otherwise. The relationships can be defined as `has many`, `has one`, `belongs to`, etc.

## 7.2.2 Define the access patterns
In relational databases the ERD just becomes the database with each entity representing a table. In DynamoDB, the data must be designed to handle specific access patterns, so the next step is to be *specific and thorough* in defining all of the access patterns.

There are two different strategies for building these access patterns. The first is API-centric, which makes a lot of sense when implementing a REST API. This would entail listing out all of the API endpoints and the shape of the data to be returned. The second approach is more UI-centric, which requires looking at each screen and identifying all of the information that needs to be present in a particular view.

A good way to approach this is to take an entity and list out all of the access patterns, along with the index that will be used and any parameters that will need to be provided.

## 7.2.3 Model your primary key structure
The primary key is the foundation of the table, so it is best to start modeling there. One way to approach this is to create an "entity chart" which lists all the different types of items that will live in the table along with the associated primary key structure.

While iterating through this exercise it is common for entities to be removed or added from the "entity chart" as different data is moved into lists or maps, or pulled up to be a top-level entity.

After deciding on the entities and relationship to model, the next step is to decide on a simple or composite primary key.

The last step is to start designing the primary key format for each entity type such that it satisfies the uniqueness requirement first. Then if there is any additional flexibility, the key can be used to try to solve some of the "fetch many" access patterns.

There are many things to consider when designing a primary key, but there a few common things to consider:

* What will the client know at read time?
* Use primary key prefixes to distinguish between entity types (e.g. Customer -> `CUSTOMER#<CustomerId>`)

## 7.2.4 Handle additional access patterns with secondary indices and streams
A well-crafted primary key can solve multiple access patterns, but it will often be difficult to solve for everything. Secondary indices can solve for additional access patterns, but do *not* fall into the trap of creating a new secondary index for every additional access pattern. Just as the primary key of the table is generic, make the primary and sort keys of secondary indices generic as well and craft them to solve multiple access patterns.
