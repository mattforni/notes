# Chapter 8: The What, Why and When of Single-Table Design in DynamoDB
This chapter covers the desirability or using as few tables as possible when modeling data in DynamoDB. This is one of the most distinct differences between relational databases and DynamoDB, and usually the toughest to overcome.

## 8.1 What is single-table design
In relational databases it is common to create a single table for each type of entity in the application. Each record can have many foreign keys that associate the record to other entities that can be looked up at read-time using the `joins` key. While this is very convenient, it is all incredibly expensive, because the database must scan several large portions of different tables.

DynamoDB was built explicitly for huge, high-velocity use cases, which cannot tolerate inconsistency or the slow performance of `joins` as datasets scale. One of the big benefits of using `joins` is the ability to fetch multiple, heterogenous items in a single request. The solution DynamoDB uses to provide this functionality is to pre-join all of the necessary data using **item collections**.

An **item collection** in DynamoDB is the set of all items in a table (or index) that share the same partition key. The Query API  allows for the retrieval of multiple items that share the same partition key whether they are the same type of item or not.

Single-table design is all about structuring data based on access patterns so that data can be fetched with the fewest requests to DynamoDB. Reducing the number of tables needed to run an application also reduced operational overhead (e.g. monitoring, alarming, tuning provisioning, etc.)

## 8.2 Downsides of a single-table design
While single-table design has a huge benefit when it comes to performance, there are a few drawbacks.

### Learning Curve
There is undoubtedly a steep learning curve when it comes to correctly architecting data to fit single-table design. Viewing the table via the console can be daunting and can overwhelm those used to the clean boundaries of relational data modeling.

### Access Pattern Inflexibility
Designing a single-table database starts with the access patterns and goes from there. That means that if and when a new access pattern is needed there is usually some ETL overhead to update the existing items to support the new access pattern.

### Analytics
DynamoDB is designed for On-line Transaction Processing (OLTP) **not** on-line analytics processing (OLAP), which means that it can be quite difficult to perform analytics on the raw, untransformed data stored in DynamoDB. This means that any sort of analytics will most likely need to be offloaded to another tool that is meant to handle OLAP.

## 8.3 When not to use single-table design
The short answer for when to **not** use single-table design is whenever *query flexibility* and/or *analytics accessibility* is **more important** than *performance*. The two most prominent use cases in which these conditions arise are:

### New applications that prioritize flexibility
DynamoDB was built for large-scale, high-velocity applications that have out-scaled the capabilities of relational databases. For new applications that have not yet hit scalability issues and where flexibility is more important than performance, DynamoDB can be used in a Faux-SQL format where data is normalized across many tables and accessed via multiple, serial calls to DynamoDB.

### GraphQL & Single-table design
GraphQL and single-table design *can* be used together, *but* most of the benefits of a single-table design are lost when using GraphQL as an execution engine. The issue with how GraphQL handles queries on the backend. Each field on each type in the GraphQL schema is handled by a resolver, which understands how to fill the data for that field. This usually translates to a resolver per entity type, each of which are independent of each other, meaning multiple requests would still need to be made even if all the data were in a single-table.
