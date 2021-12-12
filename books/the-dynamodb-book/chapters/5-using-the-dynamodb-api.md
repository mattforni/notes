# Chapter 5: Using the DynamoDB API
The biggest hurdle in learning DynamoDB is the mindset shift required in learning the data modeling principles. Getting the principles down and writing out access patterns makes understanding the proper API calls much easier.

## 5.1 Expression Names and Values
There are two types of placeholders, ones that start with `#` and ones that start with `:`.

The placeholders that start with colons (`:`) are *expression attribute values*. They represent the value of the attribute you are evaluating. Each attribute value in DynamoDB has a type. DynamoDB does *not* infer types, which means you must explicitly provide it when you make a request. Attributes with the same underlying value, but a different type are *not* equal.

The placeholders that start with pound (`#`) are *expression attribute names*. They represent the name of the attribute you are evaluating. Unlike `ExpressionAttributeValues` there is no requirement to use `ExpressionAttributeNames` since the names of the attributes are *not* typed. The *only* time that `ExpressionAttributeNames` are required is when the name of your attribute is a *reserved word* in DynamoDB. There are a whole lot of [reserved words](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html) in DynamoDb, but the following are common attribute names that are reserved in DynamoDB:

* `BUCKET`
* `BY`
* `COUNT`
* `MONTH`
* `NAME`
* `TIMESTAMP`
* `TIMEZONE`

If any of your attribute names conflict, you will need to use `ExpressionAttributeNames`. It is also common to use `ExpressionAttributeNames` if accessing attributes in nested documents or attributes that include a period (`.`) in their name since DynamoDB inteprets a period as accessing a nested attribute.

## 5.3 The Optional Request Properties
There are five (5) optional properies you can add to individual requests. The first three can affect the items you receive from your request and the last two can return additional metrics information about table usage.

* `ConsistentRead`
* `ScanIndexForward`
* `ReturnValues`
* `ReturnConsumedCapacity`
* `ReturnItemCollectionMetrics`

### `ConsistentRead`
By default, reads from DynamoDB are *eventually consistent*, meaning that reads will likely, but not definitely, reflect all write operations that were performed prior to the read.
