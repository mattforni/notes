# Chapter 10: The Important of Strategies
The concept of *strategies* is crucial to modeling well with DynamoDB. When modeling for a relational database there is usually one, straightforward, way to modeling.

* *Duplicated data?* Normalize it and put it in different tables.
* *One-to-many relationship?* Use a foreign key to represent the relationship.
* *Many-to-many relationship?* Use a join table to connect the entities.

This is not true when modeling with DynamoDB. Modeling this data and these relationships is much more like an art than a science and requires judgement to decide which approach works best for the situation. Take modeling a *one-to-many* relationship for instance. There are five different ways to do this in DynamoDB:

* Denormalize the data and store the nested objects as a document attribute.
* Denormalize the data by duplicating it across multiple items.
* Use a composite primary key.
* Create a secondary index.
* Usage a composite sort key to handle hierarchical data.

The next six chapters will cover some of the strategies that can be used in modeling with DynamoDB.
