# Chapter 6: Expressions
This chapter covers DynamoDB expressions that are used by the DynamoDB API. Expressions are statements that operate on `items` and are a key component to working with the DynamoDB API. There are five types of expressions in DynamoDB:

* Key Condition Expressions: Used in a *Query* to describe which items to retrieve.
* Filter Expressions: Used in a *Query* and *Scan* operation to filter items that match the key condition expression.
* Projection Expressions: Used in *Read* opertions to describe which attributes to return on read items.
* Condition Expressions: Used in *Write* operations to assert a condition on an item before writing.
* Update Expressions: Used in the *UpdateItem* operation to describe desired updates to an existing item.

|                | Key condition  | Filter         | Projection     | Condition      | Update         |
| :------------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| Query          | - [x]          | - [x]          | - [x]          | - [ ]          | - [ ]          |
| Scan           | - [ ]          | - [x]          | - [x]          | - [ ]          | - [ ]          |
| PutItem        | - [ ]          | - [ ]          | - [ ]          | - [x]          | - [ ]          |
| UpdateItem     | - [x]          | - [ ]          | - [ ]          | - [x]          | - [x]          |

All expressions may use the previously discussed expression attribute names and values.

## 6.1 Key Condition Expressions
The key condition expression is the most commonly used expression and is used predominantly in the Query API. The Query API must specify the partition key and can *optionally* specify conditions on the sort key. A key condition can *only* be used on elements of the primary key, meaning it cannot be applied to any attributes.

Conditions on the sort key can be specified using simple comparison operators, such as greater than (`>`), less than (`<`) or equal to (`=`). Since these operators can be combined to create a range the `BETWEEN` operator is also available for specifying conditions on a sort key.

Key expressions are *critical* when fetching multiple, heterogeneous items in a single request. This concept, including strategies for modeling one-to-many relationships will be covered in Chapter 11.

## 6.2 Filter Expressions
The filter expression is also applicable for read-based operations (i.e. `Query` and `Scan`) with one distinct difference between it and the key condition expression -- filter expressions can be applied to *any* attribute in the table. While this may seem more flexible than a key condition expression, there are caveats related to the way DynamoDB returns results that make filter expressions helpful, but far from a silver bullet.

Whenever a `Query` or `Scan` request is issued to DynamoDB, DynamoDB performs the following actions in order:

1. Read items from table
2. If `FilterExpression`, remove items that don't match
3. Return items

The caveat is that DynamoDB applied a *1MB* maximum on the data it will return on a single request, and that maximum is applied in *Step 1* before the filter expression is applied. In other words, if the initial read contains more than 1MB of data, the request will be paginated and each page will likely have many empty results. This is why it is best to model access patterns directly into the table and indices to properly leverage the key condition expression.

Filter expressions are useful for:

* Reducing response payload size on the server-side (instead of client side)
* Reducing the results returned from DynamoDB instead of filtering in application logic

## 6.3 Projection Expressions
The projection expression serves a similar purpose to the filter expression in that it is primarily used to reduct the amount of data sent over the wire. However, while a filter expression works on an item-by-item basis a projection expression works on an attribute-by-attribute basis, specifying exactly what attributes to return from a read operation.

A projection expression is used to tell DynamoDB which attributes of an item should be retrieved during a read operation, including nested properties, specifically nested properties of large list or map attributes.

Projection expressions are subject to the same caveats as filter expressions in that they are evaluated *after* the items are read from the table and the 1MB limit is reached. If the intention of a read is to fetch a large number of items each with a large attribute, it would likely be better to create a secondary index with a custom projection that only copies particular attributes into the index to get around this limitation.

## 6.4 Condition Expressions
The condition expression is the first expression that is applied to write operations and is available for any operation that may alter an item - `PutItem`, `UpdateItem`, `DeleteItem` and their batch and transactional equivalents. These expressions allow for the assertion of a specific statement before performing the write operation(s).

There are numerous reasons to add condition expressions to write operations, some of which are:

* To avoid overwriting an existing item when using `PutItem`
* To prevent an `UpdateItem` operation from putting an item into a bad state (i.e. account balance below 0)
* To evaluate permissions and assert that a user has access to perform the associated action

Without condition expressions many complex actions would require multiple reads and would be subject to race conditions. Condition expressions use a similar syntax to other types of expressions, but have access to several functions as well:

* `attribute_exists()`
* `attribute_not_exists()`
* `attribute_type()`
* `beings_with()`
* `contains()`
* `size()`

Condition expressions can operate on any attribute since condition expressions are used with item-based actions where an item has already been identified by passing the key in a different parameter.

## 6.5 Update Expressions
The update expression is also used for write-baed actions, but unlike other expressions it is explicitly manipulating an item and it requires that all changes be stated explicitly in the request. There are four (4) verbs for stating these changes:

| Verb           | Description                                                                 |
| :------------- | :-------------------------------------------------------------------------- |
| `SET`          | Used to add or overwrite an attribute on an item                            |
| `REMOVE`       | Used to delete an attribute from an item, including nested properties       |
| `ADD`          | Used to add to a number attribute or insert an element into a set attribute |
| `DELETE`       | Used to remove an element from a set attribute                              |

Any combination of these four verbs can be used in a single update statement and any number of operations can be preformed for a single verb. If a verb has multiple operations, the verb is only specified once and commas are used to separate the clauses.

E.G.
```
UpdateExpression="SET Name = :name, UpdatedAt = :updatedAt"
```

Several common usages of the update expressions are:

* Updating or setting an attribute on an item
* Deleting an attribute from an item
* Incrementing a numeric value
* Adding a nested property
* Adding and removing from a set

Update expressions often save the trouble of having to query an item and then update the item and used in conjunction with a condition expression much of the application logic can be pushed to the data layer and executed in a single request.
