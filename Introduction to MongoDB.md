
2024-11-04 07:40

Tags: [[MongoDB]]


# Introduction to MongoDB

## Unit 3: MongoDB and the Document Model

### Lesson 1: Introduction to MongoDB

1. **MongoDB Classification & Usage**:
    
    - **Type**: General-purpose document database.
    - **Data Structure**: Organizes data in documents, resembling JSON objects, contrasting with relational databases (tables, rows, columns).
    - **Flexibility**: Documents align well with how developers think in code, making data modeling intuitive and development faster.
2. **Document Model Benefits**:
    
    - **Adaptability**: Supports data of various shapes, including simple key-value pairs, text, geospatial indexes, time series, graph data, etc.
    - **Single Format**: Enables a unified approach to modeling and querying data for diverse applications, boosting productivity.
    - **Language Compatibility**: MongoDB provides drivers for major programming languages, allowing easy integration across applications.
3. **Use Cases**:
    
    - MongoDB is versatile, used in e-commerce, content management, IoT, time series data, trading, payments, gaming, mobile apps, real-time analytics, and AI applications.
4. **Advantages of MongoDB**:
    
    - **Scalability**
    - **Resilience**
    - **Speed of Development**
    - **Data Privacy**
    - **Security**

#### Essential Terms in MongoDB:

- **Document**: The basic unit of data in MongoDB, similar to JSON objects.
- **Collection**: A grouping of documents. Collections typically hold similar data but can have varied structures due to MongoDB’s flexible schema model.
- **Database**: A container for collections.

#### MongoDB and MongoDB Atlas:

- **Atlas**: MongoDB’s cloud-hosted data platform, built on MongoDB databases.
- **Atlas Functionality**: Extends MongoDB's core capabilities with additional features like full-text search and data visualization.

#### Summary:

- **MongoDB** is a flexible, general-purpose document database.
- **Documents** allow intuitive data structuring and development.
- **Key Terms**: Document, Collection, Database.
- **MongoDB Atlas** builds on MongoDB to provide additional cloud-hosted functionalities.


---
### Lesson 2: The MongoDB Document Model
#### Key Concepts:

1. **Document Structure**:
    
    - MongoDB organizes data in **documents**, resembling JSON objects.
    - Example: A product document in an electronic store may include fields like `colors` (an array of strings) and `available` (a Boolean).
2. **BSON (Binary JSON)**:
    
    - **Storage Format**: Although documents display in JSON format, MongoDB stores them in BSON.
    - **BSON Benefits**: Extends JSON, adding support for data types like dates, different types of numbers, and unique identifiers (ObjectIDs).
3. **Data Types**:
    
    - MongoDB supports standard JSON types (e.g., string, object, array, Boolean, null) and additional types in BSON:
        - **Dates**
        - **Various numeric types**
        - **ObjectIDs**: Special identifiers used for the `ID` field, which is required as a primary key in each document.
    - **Automatic ID Creation**: MongoDB generates an ObjectID if a document lacks an ID field.
4. **Flexible Schema Model**:
    
    - **Schema Flexibility**: MongoDB allows documents within a single collection to have different fields and data types, supporting **polymorphic data**.
    - **Benefits**:
        - **Rapid Iteration**: Schema changes don’t require table redefinition, as they do in relational databases.
        - **Adaptability**: Developers can modify document structures as requirements evolve, reducing dependency issues and eliminating downtime for schema changes.
5. **Example of Flexible Schema**:
    
    - Scenario: An online furniture store initially includes an ID, name, and price for items.
    - **Adding Fields**: If a description field is needed later, MongoDB allows this change without requiring scripts or affecting existing data structures, unlike traditional relational databases.
6. **Schema Validation**:
    
    - **Optional Constraints**: Schema validation rules allow for control over document structure, ensuring consistency within collections when needed.

#### Summary:

- **BSON Format**: MongoDB stores data in BSON, which supports additional data types beyond JSON, such as ObjectIDs for unique identification.
- **ObjectID**: A unique identifier used by default as a document’s primary key.
- **Flexible Schema**: MongoDB’s flexible schema allows for evolving document structures within a collection, accelerating development.
- **Schema Validation**: Optional rules provide structure constraints for collections if needed.

---
## Unit 4: Connecting to a MongoDB Database

The MongoDB Shell is a Node.js __REPL__ environment. This gives us access to:
- JavaScript variables
- functions, conditionals
- loops, and control flow statements inside of the shell.

MongoDB drivers allow an application to connect to the database using the programming language of your choice.
Our driver uses a connection string to connect to our cluster.

### MongoDB Connection Issues with Error Messages
#### Common Connection Issues in MongoDB

1. **Network Access Errors**:
   - **Error Message**: `"IP address not allowed to connect"`
   - **Cause**: IP address not whitelisted in MongoDB Atlas.
   - **Solution**:
     - Go to the **Atlas dashboard**.
     - Navigate to **Security > Network Access**.
     - Click **Add IP Address** and add your IP.
     - You can choose permanent or temporary access.
     - Wait up to 30 seconds for changes to take effect.

2. **User Authentication Errors**:
   - **Error Message**: `"Authentication failed"`
   - **Cause**: Incorrect credentials in the connection string.
   - **Solution**:
     - Verify that the **username and password** in the connection string are correct.
     - Ensure you have accurately copied the connection string from Atlas, paying close attention to the credentials.

---

These error messages should help identify and troubleshoot the two most common MongoDB connection issues.

___

## Unit 5: CRUD Operations: Insert and Find Documents

### Lesson 01: Inserting Documents in a MongoDB Collection

Use `insertOne()` to insert a document into a collection. Within the parentheses of `insertOne()`, include an object that contains the document data.

```js
db.grades.insertOne({
  student_id: 654321,
  products: [
    {
      type: "exam",
      score: 90,
    },
    {
      type: "homework",
      score: 59,
    },
  ],
  class_id: 550,
})
```

```js
// Output
{
   "acknowledged" : true,
   "insertedId" : ObjectId("56fc40f9d735c28df206d078")
}
```

Use `insertMany()` to insert multiple documents at once. Within `insertMany()`, include the documents within an array. Each document should be separated by a comma.

```js
db.grades.insertMany([
  {
    student_id: 546789,
    products: [
      {
        type: "quiz",
        score: 50,
      },
      {
        type: "homework",
        score: 70,
      },
    ],
    class_id: 551,
  },
  {
    student_id: 777777,
    products: [
      {
        type: "exam",
        score: 83,
      },
      {
        type: "quiz",
        score: 59,
      },
    ],
    class_id: 550,
  },
])
```

```js
// Output
{
   "acknowledged" : true,
   "insertedIds" : [
      ObjectId("562a94d381cb9f1cd6eb0e1a"),
      ObjectId("562a94d381cb9f1cd6eb0e1b"),
   ]
}
```

___
### Lesson 2: Finding Documents in MongoDB Collection
1. **Using the `find()` Method**:
   - Syntax for finding all documents:
     ```javascript
     db.collection.find();
     ```
   - Example:
     ```javascript
     db.zips.find();
     ```
   - This example retrieves all documents from the `zips` collection in the `Training` database.

2. **Finding Specific Documents with Equality (`$eq`)**:
   - To retrieve documents with a specific field value, use `$eq` or the implicit equality match.
   - **Examples**:
     ```javascript
     db.zips.find({ "state": { "$eq": "AZ" } });
     ```
     - This example finds all documents where the `state` field is `"AZ"`.
   - **Implicit Equality Syntax**:
     ```javascript
     db.zips.find({ "state": "AZ" });
     ```
     - This query is equivalent to the one above but shorter.

3. **Using `$in` Operator**:
   - **Purpose**: `$in` selects documents where a field's value matches any value within a specified array.
   - **Syntax**:
     ```javascript
     db.collection.find({ "field": { "$in": ["value1", "value2", ...] } });
     ```
   - **Example**:
     ```javascript
     db.zips.find({ "city": { "$in": ["Phoenix", "Chicago"] } });
     ```
     - This query finds all documents in the `zips` collection where the `city` field is either `"Phoenix"` or `"Chicago"`.

---

#### Summary
- Use `find()` to query MongoDB collections and retrieve documents.
- `$eq` operator and implicit syntax help find documents based on specific field values.
- `$in` operator enables querying for documents where a field matches any value in a given array.

These methods and operators provide a solid foundation for retrieving documents based on various conditions.

---

### Lesson 03: Finding Documents by Using Comparison Operators

#### Key Comparison Operators:

1. **Greater Than (`$gt`)**:
   - Finds documents where a field’s value is greater than a specified value.
   - Syntax:
     ```javascript
     { "fieldName": { "$gt": value } }
     ```
   - Example:
     ```javascript
     db.sales.find({ "items.price": { "$gt": 50 } });
     ```
     - This returns documents where at least one item has a price greater than 50.

2. **Less Than (`$lt`)**:
   - Finds documents where a field’s value is less than a specified value.
   - Syntax:
     ```javascript
     { "fieldName": { "$lt": value } }
     ```
   - Example:
     ```javascript
     db.sales.find({ "items.price": { "$lt": 50 } });
     ```
     - This returns documents with at least one item priced less than 50.

3. **Less Than or Equal To (`$lte`)**:
   - Finds documents where a field’s value is less than or equal to a specified value.
   - Syntax:
     ```javascript
     { "fieldName": { "$lte": value } }
     ```
   - Example:
     ```javascript
     db.sales.find({ "customer.age": { "$lte": 65 } });
     ```
     - This query returns documents containing customers aged 65 or younger.

4. **Greater Than or Equal To (`$gte`)**:
   - Finds documents where a field’s value is greater than or equal to a specified value.
   - Syntax:
     ```javascript
     { "fieldName": { "$gte": value } }
     ```
   - Example:
     ```javascript
     db.sales.find({ "customer.age": { "$gte": 65 } });
     ```
     - This retrieves documents with customers aged 65 or older.

#### Summary:
- The comparison operators (`$gt`, `$lt`, `$lte`, `$gte`) allow flexible querying of documents based on field values.
- Operators must be prefixed with `$` and are applied by specifying the field, operator, and comparison value.
  
MongoDB offers additional comparison operators beyond these core ones—exploring the MongoDB documentation can provide insights into more advanced querying techniques.

---

### Lesson 04: Querying on Array Elements in MongoDB

#### Key Concepts

1. **Basic Array Query**:
	   - To find documents containing a specific value within an array field, use a simple equality match.
   - Example:
     ```javascript
     db.accounts.find({ "products": "investment stock" });
     ```
   - This query finds all documents in the `accounts` collection where the `products` field contains `"investment stock"` as a value, either as a single element in an array or a scalar value.
	   ![Screenshot (55) 1.png](assets/Screenshot%20(55)%201.png)

2. **Using `$elemMatch`**:
   - **Purpose**: Ensures that only documents with a specified value as an element of an array are returned.
   - **Syntax**:
     ```javascript
     { "field": { "$elemMatch": { "operator": "value" } } }
     ```
   - **Example**:
     ```javascript
     db.accounts.find({ "products": { "$elemMatch": { "$eq": "investment stock" } } });
     ```
     - This returns documents where the `products` field is an array that contains `"investment stock"` as an element.

3. **Multiple Criteria with `$elemMatch`**:
   - `$elemMatch` can also apply multiple query criteria to a single element within an array of sub-documents.
   - **Example**:
     ```javascript
     db.sales.find({
       "items": {
         "$elemMatch": {
           "name": "laptop",
           "price": { "$gt": 800 },
           "quantity": { "$gte": 1 }
         }
       }
     });
     ```
     - This query retrieves documents in the `sales` collection where at least one element in the `items` array meets the following conditions:
       - The item `name` is `"laptop"`,
       - The `price` is greater than `800`, and
       - The `quantity` is `1` or more.

4. ** Equivalent to `$and` Operation[**
- The [`$all`](https://www.mongodb.com/docs/manual/reference/operator/query/all/#mongodb-query-op.-all) is equivalent to an [`$and`](https://www.mongodb.com/docs/manual/reference/operator/query/and/#mongodb-query-op.-and) operation of the specified values; i.e. the following statement:

```js
{ tags: { $all: [ "ssl" , "security" ] } }
```

is equivalent to:

```js
{ $and: [ { tags: "ssl" }, { tags: "security" } ] }
```
---

#### Summary
- Basic equality queries can find documents where a field contains a specified value, whether in an array or scalar form.
- `$elemMatch` refines queries by ensuring that a specific value exists as an element of an array or that a sub-document within an array matches multiple criteria.

These examples highlight the versatility of array queries in MongoDB, providing powerful tools to pinpoint specific data within complex document structures.

---

### Lesson 05: Finding Documents by Using Logical Operators

`$and`
```js
db.collection.find(
	$and: [
		{expression},
		{expression},
		...
	]
)
```

__Implicit and__
```js
db.collction.find({expression, expression})
```


`$or`  is the same

Both
![Screenshot (59) 2.png](assets/Screenshot%20(59)%202.png)

![Screenshot (60) 2.png](assets/Screenshot%20(60)%202.png)

But if we used the implicit `$and`, that's what will happen
![Screenshot (62) 2.png](assets/Screenshot%20(62)%202.png)
The first $or expression was overwritten by the second one; This happen because we can't store two fields with the same name in the same JSON object 
![Screenshot (64) 2.png](assets/Screenshot%20(64)%202.png)
As a general rule, When including the same operator more than once in your query, you need to use the explicit $and operator.

___


## Unit 6: MongoDB CRUD Operations: Replace and Delete Documents

### Lesson 01: Replacing a Document in MongoDB

1. **Replacing a Document with `replaceOne()`**:
   - The `replaceOne()` method is used to replace an entire document in a MongoDB collection.
   - **Syntax**:
     ```javascript
     db.collection.replaceOne(filter, replacementDocument, options);
     ```
     - **Parameters**:
       - `filter`: Specifies which document to replace (often by `_id`).
       - `replacementDocument`: The new document data to replace the old one.
       - `options` (optional): Additional settings (not covered in this video).

2. **Example Usage**:
   - Suppose we have an incorrectly inserted document, like an unpublished book entry with placeholder values. We want to replace this with a document that has accurate information.

   - **Filter by Unique Identifier**:
     - When identifying the document, using `_id` in the filter is ideal since `_id` values are unique.
   - **Replace with Updated Document**:
     ```javascript
     db.books.replaceOne(
       { "_id": ObjectId("exampleId123") },
       {
         "title": "Corrected Title",
         "ISBN": "978-3-16-148410-0",
         "publicationDate": "2024-01-01",
         "thumbnailURL": "http://newthumbnail.com/image.jpg"
       }
     );
     ```

3. **Output Verification**:
   - Running `replaceOne()` returns an output with two important counts:
     - `matchedCount`: The number of documents that matched the filter criteria.
     - `modifiedCount`: The number of documents that were replaced.
   - Example output:
     ```json
     {
	   "acknowledged": true,
	   insertedId: null,
       "matchedCount": 1,
       "modifiedCount": 1,
       upsertedCount: 0
     }
     ```
   - If `matchedCount` and `modifiedCount` both equal `1`, the replacement was successful.

4. **Double-Checking with `findOne()`**:
   - Use `findOne()` to verify the update:
     ```javascript
     db.books.findOne({ "_id": ObjectId("exampleId123") });
     ```
   - This command retrieves the document, allowing you to confirm that it has been updated and still retains the same `_id`.

---

#### Summary
- Use `replaceOne()` to fully replace a document in a MongoDB collection.
- Ensure you use a unique identifier (e.g., `_id`) as a filter for precise targeting.
- `matchedCount` and `modifiedCount` confirm the success of the operation.
- Verify the replacement with `findOne()` if necessary.

This approach helps maintain data accuracy by replacing documents with correct and updated information when needed.

---

### Lesson 02: Updating MongoDB Documents by Using `updateOne()`

The `updateOne()` method accepts a __filter document__, an __update document__, and an optional __options object__. MongoDB provides update operators and options to help you update documents. In this section, we'll cover three of them: `$set`, `upsert`, and `$push`.

## `$set`

The `$set` operator replaces the value of a field with the specified value or Adds new fields and values to a document, as shown in the following code:

```json
db.podcasts.updateOne(
  {
    _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8"),
  },

  {
    $set: {
      subscribers: 98562,
    },
  }
)
```

The output message shows the matched and modified document counts
```json
    {
	   "acknowledged": true,
	   insertedId: null,
       "matchedCount": 1,
       "modifiedCount": 1,
       upsertedCount: 0
     }
```
If the update filter doesn't match any documents, then no update occurs.
## `upsert`

The `upsert` option creates a new document if no documents match the filtered criteria. Here's an example:

```json
db.podcasts.updateOne(
  { title: "The Developer Hub" },
  { $set: { topics: ["databases", "MongoDB"] } },
  { upsert: true }
)
```

The `upsert` is short for _update or insert_
## `$push`

The `$push` operator adds a new value to the `hosts` array field. If the field is absent, `$push` adds the array field with the value as its element Here's an example:

```json
db.podcasts.updateOne(
  { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
  { $push: { hosts: "Nic Raboy" } }
)
```

Use [`$push`](https://www.mongodb.com/docs/manual/reference/operator/update/push/#mongodb-update-up.-push) with the [`$each`](https://www.mongodb.com/docs/manual/reference/operator/update/each/#mongodb-update-up.-each) modifier to append multiple values to the array field.
```json
db.students.updateOne(
   { name: "joe" },
   { $push: { scores: { $each: [ 90, 92, 85 ] } } }
)
```


## `$inc`

The [`$inc`](https://www.mongodb.com/docs/manual/reference/operator/update/inc/#mongodb-update-up.-inc) operator increments a field by a specified value.
`{ $inc: { <field1>: <amount1>, <field2>: <amount2>, ... } }`

```json
db.products.updateOne(
   { sku: "abc123" },
   { $inc: { quantity: -2, "metrics.orders": 1 } }
)
```

___

### Lesson 03: Updating MongoDB Documents by Using `findAndModify()`
#### Key Concepts

1. **Purpose of `findAndModify()`**:
   - The `findAndModify()` method is used to update a document in MongoDB and return the modified version in a single operation. This ensures data consistency by preventing other processes from altering the document between update and retrieval.

2. **Advantages of `findAndModify()` over `updateOne()` + `findOne()`**:
   - **Efficiency**: `findAndModify()` completes the update and retrieval in one round trip to the server, reducing latency.
   - **Data Consistency**: By using `findAndModify()`, the exact version of the document just updated is returned. In contrast, `updateOne()` followed by `findOne()` could result in retrieving a different version if another user modifies the document in between.

3. **Syntax of `findAndModify()`**:
   ```javascript
   db.collection.findAndModify({
     query: { /* criteria */ },
     update: { /* modifications */ },
     new: true
   });
   ```

4. **Example: Tracking Podcast Downloads**:
   - Suppose we have a podcast document with a `downloads` field we want to increment and retrieve in a single operation.
   - **Query Document**: Specifies the document to update, often by `_id`.
   - **Update Document**: Specifies the update operation, e.g., incrementing `downloads` by 1.
   - **Options**:
     - Set `new: true` to return the updated document instead of the original version before modification.

   ```javascript
   db.podcasts.findAndModify({
     query: { "_id": ObjectId("examplePodcastId123") },
     update: { $inc: { downloads: 1 } },
     new: true
   });
   ```

   - **Explanation**:
     - `query`: Finds the podcast document by `_id`.
     - `update`: Increments the `downloads` field by 1 using `$inc`.
     - `new: true`: Ensures the returned document reflects the update.

5. **Result of `findAndModify()`**:
   - The modified document is returned, showing the updated `downloads` field. For instance, if the original value was 6012, the output will display `downloads: 6013`.

6. __Using `upsert`__
	```json
	db.zips.findAndModify({
	  query: { zip: 87571 },
	  update: { $set: { city: "TAOS", state: "NM", pop: 40000 } },
	  upsert: true,
	  new: true,
	})
	```

---

#### Summary
- **Use `findAndModify()`** to update a document and retrieve the modified version in one atomic operation.
- Setting `new: true` in the options ensures the modified document is returned.
- This method is ideal when data consistency and reduced latency are critical, especially in cases where multiple users might access and modify documents concurrently.

___

## **Note**

`findAndModify()` is deprecated in MongoDB. MongoDB now recommends using more specialized methods for modifying and querying documents. These alternatives include `findOneAndUpdate()`, `findOneAndDelete()`, and `findOneAndReplace()`. Each of these methods comes with various options to control their behavior. Let's dive into each one with examples.

### 1. `findOneAndUpdate()`

#### Purpose:
Updates a single document and returns either the original or the updated document.

#### Syntax:
```javascript
db.collection.findOneAndUpdate(
  filter,
  update,
  options
)
```

#### Options:
- `projection`: Specifies the fields to return in the document.
- `sort`: Determines which document to modify if the query matches multiple documents.
- `upsert`: Creates a new document if no documents match the query filter.
- `returnDocument`: Specifies whether to return the document before or after the update (values: "before" or "after").

#### Example:
```javascript
db.collection.findOneAndUpdate(
  { name: "Alice" },
  { $set: { age: 29 } },
  {
    projection: { name: 1, age: 1 },
    sort: { age: -1 },
    upsert: true,
    returnDocument: "after"
  }
)
```
In this example, if no document with `name: "Alice"` exists, it creates one. It returns the document after the update with only the `name` and `age` fields.

### 2. `findOneAndDelete()`

#### Purpose:
Deletes a single document and returns the deleted document.

#### Syntax:
```javascript
db.collection.findOneAndDelete(
  filter,
  options
)
```

#### Options:
- `projection`: Specifies the fields to return in the document.
- `sort`: Determines which document to delete if the query matches multiple documents.

#### Example:
```javascript
db.collection.findOneAndDelete(
  { name: "Bob" },
  {
    projection: { name: 1, email: 1 },
    sort: { age: 1 }
  }
)
```
This example deletes the document where `name: "Bob"`, returns the deleted document with only `name` and `email` fields, and sorts by `age` if there are multiple matches.

### 3. `findOneAndReplace()`

#### Purpose:
Replaces a single document and returns either the original or the replaced document.

#### Syntax:
```javascript
db.collection.findOneAndReplace(
  filter,
  replacement,
  options
)
```

#### Options:
- `projection`: Specifies the fields to return in the document.
- `sort`: Determines which document to replace if the query matches multiple documents.
- `upsert`: Creates a new document if no documents match the query filter.
- `returnDocument`: Specifies whether to return the document before or after the replacement (values: "before" or "after").

#### Example:
```javascript
db.collection.findOneAndReplace(
  { name: "Charlie" },
  { name: "Charlie", age: 35, city: "New York" },
  {
    projection: { name: 1, city: 1 },
    sort: { name: 1 },
    upsert: false,
    returnDocument: "after"
  }
)
```
In this example, it replaces the document with `name: "Charlie"` and returns the document after replacement with only the `name` and `city` fields.

### Summary
The deprecation of `findAndModify()` in MongoDB has led to clearer, more specific methods for updating, deleting, and replacing documents. These methods (`findOneAndUpdate()`, `findOneAndDelete()`, `findOneAndReplace()`) offer a range of options to control their behavior, making MongoDB operations more efficient and easier to understand.

___
### Lesson 04: Updating MongoDB Documents by Using `updateMany()`

#### Key Concepts

1. **Purpose of `updateMany()`**:
   - The `updateMany()` method in MongoDB allows us to update multiple documents that match a specified condition within a single collection. It's especially useful for batch updates where multiple records meet the same criteria.

2. **Syntax of `updateMany()`**:
   ```javascript
   db.collection.updateMany(
     { /* filter criteria */ },
     { /* update document */ }
   );
   ```
   - This method takes three parameters:
     - **Filter Document**: Specifies which documents to update based on certain criteria.
     - **Update Document**: Defines the changes to apply to all matching documents.
     - **Options Object**: An optional object that provides additional options (not covered here).

3. **Example: Updating Books Published Before 2019**:
   - Imagine a `books` collection where we want to mark all books published before 2019 with a `status` of `"legacy"`.
   - **Filter Document**: Specifies that we want books with a `publish_date` before January 1, 2019, using the `$lt` operator.
   - **Update Document**: Uses the `$set` operator to set `status` to `"legacy"` for all matching documents.

   ```javascript
   db.books.updateMany(
     { publish_date: { $lt: new Date("2019-01-01") } },
     { $set: { status: "legacy" } }
   );
   ```
   - **Explanation**:
     - `publish_date: { $lt: new Date("2019-01-01") }`: Finds documents where `publish_date` is earlier than January 1, 2019.
     - `$set: { status: "legacy" }`: Sets the `status` field to `"legacy"` for all documents that match the filter criteria.

4. **Result of `updateMany()`**:
   - After executing `updateMany()`, MongoDB returns an output showing the `matchedCount` (number of documents that met the filter criteria) and the `modifiedCount` (number of documents actually updated). These values should match if the update is successful for all matched documents.

5. **Considerations When Using `updateMany()`**:
   - **No Rollback on Failure**: If an `updateMany()` operation fails mid-way, MongoDB does not roll back changes to documents already updated. You would need to rerun `updateMany()` to update any remaining documents.
   - **Lacks Isolation**: Updates made by `updateMany()` are visible immediately. This means that it is not ideal for transactions or cases requiring isolated updates, such as financial transactions, where data consistency across multiple records is critical.

---

#### Summary
- **Use `updateMany()`** to apply updates across multiple documents in a collection.
- Provides an efficient way to batch update records, but does not support rollback on partial failures.
- This method is ideal for general updates but should be used cautiously in scenarios requiring strict data isolation.

___

### Lesson 05: Deleting Documents in MongoDB

To delete documents, use the `deleteOne()` or `deleteMany()` methods. Both methods accept a filter document and an options object.

## Delete One Document

The following code shows an example of the `deleteOne()` method:

```js
db.podcasts.deleteOne({ _id: Objectid("6282c9862acb966e76bbf20a") })
```

## Delete Many Documents

The following code shows an example of the `deleteMany()` method:

```js
db.podcasts.deleteMany({category: “crime”})
```

___

## Unit 7: MongoDB CRUD Operations: Modifying Query Results

### Lesson 01: Sorting and Limiting Query Results in MongoDB

#### Key Concepts

1. **MongoDB Cursors**:
   - In MongoDB, a cursor is a pointer to the result set of a query. 
   - The `find()` method returns a cursor pointing to all documents that match the query.
   - We can use cursor methods, such as `.sort()` and `.limit()`, to modify the results.

2. **Sorting Results Using `sort()`**:
   - The `sort()` method arranges the documents returned by a query based on the values in specified fields.
   - **Syntax**:
     ```javascript
     db.collection.find({ /* query */ }).sort({ field: 1 or -1 });
     ```
   - **Sort Order**:
     - `1`: Ascending order.
     - `-1`: Descending order.
   - **Example**:
     - Suppose we want to list all music companies in alphabetical order by name. Here’s how:
       ```javascript
       db.companies.find({ category_code: "music" }).sort({ name: 1 });
       ```
     - Here, we filter companies where `category_code` is `"music"` and sort them by `name` in ascending (alphabetical) order.

   - **Sorting on Multiple Fields**:
     - You can sort by multiple fields by passing additional field-value pairs to `sort()`.
     - **Example**:
       ```javascript
       db.collection.find().sort({ field1: 1, field2: -1 });
       ```
     - This example sorts primarily by `field1` in ascending order and then by `field2` in descending order if there are ties.

3. **Sorting Behavior**:
   - MongoDB sorts capital letters before lowercase letters, grouping uppercase entries first and then lowercase entries. 
   - You can adjust sorting options for more advanced alphabetization needs.

		#### Collation in MongoDB for Case-Insensitive Sorting
		
		**Default Behavior:**
		- MongoDB sorts capital letters before lowercase letters.
		
		**Solution:**
		- Use **collation** to adjust sorting behavior for more intuitive alphabetization.
		
		### Steps to Implement Collation:
		
		1. **Create Collection with Collation:** You can create a collection with a collation setting that is case-insensitive.
		   ```javascript
		   db.createCollection("customers", { collation: { locale: 'en', strength: 2 } });
		   ```
		   - `locale`: Specifies the language (e.g., 'en' for English).
		   - `strength: 2`: Ignores case differences.
		
		2. **Query with Collation:** If the collection is already created, you can specify collation directly in your queries:
		   ```javascript
		   db.customers.find().sort({ name: 1 }).collation({ locale: 'en', strength: 2 });
		   ```
		   - Sorts the `name` field in a case-insensitive manner.
		
		### Example:
		- **Without Collation:** Uppercase letters are sorted before lowercase letters.
		- **With Collation:** Sorted in a case-insensitive way, making the order more intuitive.
		
		Using collation in MongoDB helps achieve advanced alphabetization needs by making sorting case-insensitive. Give it a try in your MongoDB collection!

4. **Limiting Results Using `limit()`**:
   - The `limit()` method restricts the number of documents returned in a query, which helps enhance performance by reducing data load.
   - **Syntax**:
     ```javascript
     db.collection.find().limit(number);
     ```
   - **Example**:
     - To find the three music companies with the highest number of employees:
       ```javascript
       db.companies.find({ category_code: "music" }).sort({ employees: -1 }).limit(3);
       ```
     - Here, we sort the companies by `employees` in descending order (largest to smallest) and limit the results to the top three.

5. **Example Result**:
   - In our example of music companies by employee count, the sorted and limited query might return:
     - Spotify: 5,000 employees
     - Rhapsody: 150 employees
     - Official VirtualDJ: 102 employees

6. **Using Projections with `sort()` and `limit()`**:
   - When documents contain many fields, you can use **projections** to select only specific fields, simplifying the results. 
   - Example projection:
     ```javascript
     db.companies.find({ category_code: "music" }, {name: 1, employees: 1})
       .sort({ employees: -1 })
       .limit(3)
     ```
   - Here, only the `name` and `employees` fields are returned for each document.

	The `.$` operator in a projection is used to include only the **first matched array element** from a query that includes an array field and a condition.

---

#### Summary
- **`sort()`**: Organizes results by specified fields and order (ascending or descending).
- **`limit()`**: Restricts the number of documents returned, helping improve app performance by reducing unnecessary data.
- **Projections**: Used with these methods to streamline data by only including specific fields in the final output. 

This approach helps you control the data shown to clients, making MongoDB queries more efficient and relevant.

___

### Lesson 02: Returning Specific Data from a Query in MongoDB

#### Key Concepts

1. **Projections in MongoDB**:
   - By default, MongoDB queries return all fields in matching documents.
   - Projections allow you to specify which fields you want returned, which can reduce data transfer and improve application performance.
   - **Projection Document**: Used as the second argument in `find()` queries to select specific fields.

2. **Basic Projection Syntax**:
   - **Inclusion**: To include specific fields, set their values to `1` in the projection document.
   - **Exclusion**: To exclude specific fields, set their values to `0` in the projection document.
   - **Example**:
     ```javascript
     db.collection.find({ /* query */ }, { field1: 1, field2: 0 });
     ```
   - **Inclusion/Exclusion Rules**:
     - You cannot mix inclusion and exclusion in the same projection document, except for the `_id` field.

3. **Inclusion Example**:
   - Suppose we want to only see the `business_name` and `result` fields in an inspections collection:
     ```javascript
     db.inspections.find({ sector: "restaurant 818" }, { business_name: 1, result: 1 });
     ```
   - This returns only `business_name` and `result` for documents matching the query.

4. **Excluding `_id`**:
   - MongoDB includes the `_id` field by default in query results.
   - To exclude `_id`, set it to `0` in the projection document:
     ```javascript
     db.inspections.find({ sector: "restaurant 818" }, { business_name: 1, result: 1, _id: 0 });
     ```
   - This will return `business_name` and `result` without the `_id` field.

5. **Exclusion Example**:
   - To exclude specific fields, set them to `0`.
   - **Example**: Find documents where `result` is either "Pass" or "Warning" but exclude `date` and `address.zip`.
     ```javascript
     db.inspections.find(
       { result: { $in: ["Pass", "Warning"] } },
       { date: 0, "address.zip": 0 }
     );
     ```
   - Here, we use dot notation (`"address.zip"`) to exclude `zip` within the `address` subdocument. Note the quotation marks around `"address.zip"`.

6. **Dot Notation in Projections**:
   - **Dot Notation**: Used for accessing nested fields in subdocuments.
   - **Syntax**:
     - Include quotation marks around fields when using dot notation (e.g., `"address.zip"`).

---

#### Key Takeaways

- **Projection**: The process of selecting specific fields in MongoDB queries.
- **Performance**: Reduces data returned from the database, enhancing performance.
- **Syntax Recap**:
  - Include fields by setting them to `1`, exclude by setting to `0`.
  - `_id` is included by default; exclude it explicitly if desired.
  - **Inclusion/Exclusion Limitation**: Generally, do not mix `1` and `0` in the same projection, except with `_id`.

---

With this knowledge, you’re set to optimize your MongoDB queries by retrieving only the necessary data for your application.

___

### Lesson 03: Counting Documents in a MongoDB Collection

Review the following code, which demonstrates how to count the number of documents that match a query.

## Count Documents

Use `db.collection.countDocuments()` to count the number of documents that match a query. `countDocuments()` takes two parameters: a query document and an options document.

Syntax:

```js
db.collection.countDocuments( <query>, <options> )
```

The query selects the documents to be counted.

Examples:

```js
// Count number of docs in trip collection
db.trips.countDocuments({})
```

```js
// Count number of trips over 120 minutes by subscribers
db.trips.countDocuments({ tripduration: { $gt: 120 }, usertype: "Subscriber" })
```


---
---

## Unit 8: MongoDB Aggregation

### Lesson 01: Introduction to MongoDB Aggregation

---

#### Aggregation Framework Overview

- **Aggregation**: Analyzing and summarizing data without permanently altering the source data.
- **Aggregation Pipeline**: A series of stages where each stage performs an operation on the data. Each stage’s output becomes the input for the next stage, allowing for complex data transformations.

---

#### Components of an Aggregation Pipeline

1. **Stages**:
   - Each stage represents an operation.
   - Common stages:
     - **$match**: Filters documents by criteria, similar to the `find` method.
     - **$group**: Groups documents by a specified field or criteria.
     - **$sort**: Orders documents based on specified fields.

2. **Syntax**:
   - **Dot Notation**: Use `db.collection.aggregate([ /* stages */ ])` to define a pipeline.
   - **Field Paths**: Fields are often prefixed with `$` to reference their values (e.g., `$firstName`).
   - **Expressions**: Many stages use expressions and operators to transform data.
   - 
	```JS
	db.collection.aggregate([
	    {
	        $stage1: {
	            { expression1 },
	            { expression2 }...
	        },
	        $stage2: {
	            { expression1 }...
	        }
	    }
	])
	```
3. **MongoDB Atlas Visual Editor**:
   - Allows users to create and test aggregation pipelines with a visual interface.
   - Displays results at each stage, making it helpful for beginners.

---

These basics form the foundation for creating aggregation pipelines, where data can be filtered, grouped, and transformed across multiple stages for in-depth analysis.

___

### Lesson 2: Using `$match` and `$group` Stages in a MongoDB Aggregation Pipeline

Review the following sections, which show the code for the `$match` and `$group` aggregation stages.

## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/02-match-and-group/lesson-text/2-lesson-text.md#match)`$match`

The `$match` stage filters for documents that match specified conditions. Here's the code for `$match`:

```js
{
  $match: {
     "field_name": "value"
  }
}
```

- **Argument**: Takes a query, functioning the same way as a `find` command.
- **Optimization Tip**: Place `$match` early in the pipeline to leverage indexes, reducing the number of documents and improving performance.
## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/02-match-and-group/lesson-text/2-lesson-text.md#group)`$group`

The `$group` stage groups documents by a group key.

```js
{
  $group:
    {
      _id: <expression>, // Group key
      <field>: { <accumulator> : <expression> }
    }
 }
```

- **Purpose**: Groups documents by a specified key, creating one document per unique group key.
- **Group Key** (`_id`): Defines how documents are grouped.
- **Accumulators**: An expression that specifies how to aggregate information for each of the groups. For example:
    - `$count` for counting documents within each group.
## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/02-match-and-group/lesson-text/2-lesson-text.md#match-and-group-in-an-aggregation-pipeline)`$match` and `$group` in an Aggregation Pipeline

The following aggregation pipeline finds the documents with a field named "state" that matches a value "CA" and then groups those documents by the group key "$city" and shows the total number of zip codes in the state of California.

```js
db.zips.aggregate([
{   
   $match: { 
      state: "CA"
    }
},
{
   $group: {
      _id: "$city",
      totalZips: { $count : { } }
   }
}
])
```

___
### Lesson 3:  Using `$sort` and `$limit` Stages in a MongoDB Aggregation Pipeline

Review the following sections, which show the code for the `$sort` and `$limit` aggregation stages.

## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/03-sort-and-limit/lesson-text/3-lesson-text.md#sort)`$sort`

The `$sort` stage sorts all input documents and returns them to the pipeline in sorted order. We use 1 to represent ascending order, and -1 to represent descending order.

```js
{
    $sort: {
        "field_name": 1
    }
}
```

  

## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/03-sort-and-limit/lesson-text/3-lesson-text.md#limit)`$limit`

The `$limit` stage returns only a specified number of records.

```js
{
  $limit: 5
}
```

  

## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/03-sort-and-limit/lesson-text/3-lesson-text.md#sort-and-limit-in-an-aggregation-pipeline)`$sort` and `$limit` in an Aggregation Pipeline

The following aggregation pipeline sorts the documents in descending order, so the documents with the greatest `pop` value appear first, and limits the output to only the first five documents after sorting.

```js
db.zips.aggregate([
{
  $sort: {
    pop: -1
  }
},
{
  $limit:  5
}
])
```

___

### Lesson 4: Using `$project`, `$count`, and `$set` Stages in a MongoDB Aggregation Pipeline

Review the following sections, which show the code for the `$project`, `$set`, and `$count` aggregation stages.

## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/04-project-count-set/lesson-text/4-lesson-text.md#project)`$project`

The `$project` stage specifies the fields of the output documents. 1 means that the field should be included, and 0 means that the field should be supressed. The field can also be assigned a new value.

It should be the last stage to format the output
MongoDB already works out which fields are needed and reads only the fields that are required in the pipeline, so there's usually no reason to use project earlier in the pipeline

![Pasted image 20241107032511.png](assets/Pasted%20image%2020241107032511.png)

```js
{
    $project: {
        state:1, 
        zip:1,
        population:"$pop",
        _id:0
    }
}
```

## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/04-project-count-set/lesson-text/4-lesson-text.md#set)`$set`

The `$set` stage creates new fields or changes the value of existing fields, and then outputs the documents with the new fields.

**Purpose**: Adds or modifies fields in the pipeline without specifying the entire output structure.
`$set` is an alias for `$addField`
`$set` and `$project` can both create and assign values to fields, but only `$project` can be used to reshape the data.

```js
{
	$set: {
		pop_2022: {$round: {$multiply: [1.0031, '$pop']}}
	}
}
```

```js
{
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }
```


## [](https://github.com/10gen/curriculum/blob/develop/Application-Developer/07-Aggregation/Lessons/04-project-count-set/lesson-text/4-lesson-text.md#count)`$count`

The `$count` stage creates a new document, with the number of documents at that stage in the aggregation pipeline assigned to the specified field name.

```js
{
  $count: "total_zips"
}
```



Use the `sightings` collection in this lab.
1. Create an aggregation pipeline on the `sightings` collection. _(Forgot the command or aggregation stages? Check the hint below!)_
2. Create a stage that finds matches where `species_common` is "Eastern Bluebird" and where the `date` field is a value between January 1, 2022 0:00, and January 1, 2023 0:00.
3. Create a stage to count how many sightings that includes.
4. Run your aggregation pipeline, and see how many sightings of Eastern Bluebirds took place in 2022.

```js
db.sightings.aggregate([
{
  $match: {
    date: {
      $gt: ISODate('2022-01-01T00:00:00.000Z'),
      $lt: ISODate('2023-01-01T00:00:00.000Z')
    },
    species_common: 'Eastern Bluebird'
  }
}, {
  $count: 'bluebird_sightings_2022'
}
])
``````



___

### Lesson 5: Using the `$out` Stage in a MongoDB Aggregation Pipeline

#### `$out` Stage

- **Purpose**: Writes the output of an aggregation pipeline directly into a collection.
- **Usage Constraints**: 
  - Must be the **last stage** in the pipeline.
  - **Creates** a new collection if it doesn't exist.
  - **Overwrites** an existing collection with the same name if it exists, so be cautious when naming.

- **Parameters**:
  - `coll`: Specifies the name of the collection for the output.
  - `db`: Specifies the database for the output collection (optional if using the same database as the aggregation).
```js
{ $out: { db: "<output-db>", coll: "<output-collection>" } }
```

```js
{ $out: "<output-collection>" } // Output collection is in the same database
```

**Example Pipeline Using `$out`**:
Suppose we want to group zip codes by state, calculate the total population for each state, then keep only states with a population under 1,000,000, and store this result in a new collection named `small_states`.

```javascript
db.zips.aggregate([
  { $group: { _id: "$state", total_population: { $sum: "$pop" } } },
  { $match: { total_population: { $lt: 1000000 } } },
  { $out: "small_states" }
])
```

- **Outcome**:
  - Running this pipeline will **not return results** in the terminal. Instead, the output is written directly to the `small_states` collection.
  - You can verify the output by querying the new collection:

    ```javascript
    db.small_states.find()
    ```

This approach is efficient for saving aggregated results, making them easily accessible for further queries or applications without needing to recompute the aggregation each time.


Aggregation is a way to filter, sort, group, reshape, and analyze data without changing any data in your collection.
You can use aggregation to answer questions and gain insights from the data in your collections.

___

## Unit 8: MongoDB Indexes

### Lesson 1: Using MongoDB Indexes in Collections

#### **1. Purpose and Benefits of Indexes**
   - **Definition**: Indexes are data structures that improve the speed of data retrieval by storing a small, ordered portion of the collection's data.
   - **Performance Improvement**: 
     - Reduces the need for **collection scans** (reading each document to match queries).
     - Supports **equality and range-based queries**, allowing for faster lookups and sorted outputs.
 ![Screenshot (66).png](assets/Screenshot%20(66).png)
 
   - **Efficiency Gains**:
     - Lowers **disk I/O** and **resource usage**.
     - Avoids reading full documents if the index covers all query fields.

![Screenshot (99).png](assets/Screenshot%20(99).png)
![Screenshot (100).png](assets/Screenshot%20(100).png)

#### **2. Default and Additional Indexes**
   - **Default Index**: Every collection has a default index on the `_id` field.
   - **Additional Indexes**: Custom indexes can be created on frequently queried fields to optimize performance for specific queries.

#### **3. Cost of Indexes**
   - **Write-Performance Impact**: 
     - Adding or updating documents in indexed fields requires additional updates to the index structure.
     - Multiple indexes can slow down write operations, so keep only necessary indexes to avoid performance degradation.

#### **4. Common Index Types**
   - **Single-Field Index**: An index on a single field, used for simple queries and sort on one attribute.
   - **Compound Index**: An index on multiple fields. The order of the fields in the index is important because it determines the order in which the documents are returned when querying the collection. The **index prefix** (starting fields | Index prefixes are the beginning subsets of indexed fields) can be used individually in queries.
   - **Multikey Index**: Used on fields that contain arrays. Each array entry has a separate entry in the index, making it suitable for array-based queries.

#### **Example Use Case: Compound and Multikey Index**
   - **Example Scenario**: Searching for customers who are `active` and have a specific `account`.
   - **Solution**: A **compound index** on `active` and `accounts` fields will improve query speed.
   - Since `accounts` is an array field, this index is a **multikey compound index**.
	![Screenshot (105).png](assets/Screenshot%20(105).png)
	![Screenshot (106).png](assets/Screenshot%20(106).png)
#### **Recap**
   - Indexes store ordered fields from a collection to support faster data retrieval, equality, and range-based operations, and they allow sorting without reading all documents.
   - **Types**: Single-field, compound, and multikey indexes.
   - **Considerations**: Indexes come with a write cost and should be used selectively for optimal performance.
___

### Lesson 2: Creating a Single Field Index in MongoDB

#### **1. Creating a Single Field Index**
   - **Definition**: Single field indexes are indexes on a single attribute of a document.
   - **Use Cases**: They support efficient querying and sorting on that specific field.
   - **Command**: The `createIndex` command is used to create single field indexes.
     - **Example**: To create an index on the `birthdate` field in ascending order:
       ```javascript
       db.customers.createIndex({ birthdate: 1 });
       ```
     - **Ascending Order** (`1`): While the sort order must be specified for compound indexes, a single ascending index works for both ascending and descending queries.

#### **2. Enforcing Uniqueness with Indexes**
   - **Unique Constraint**: Use the `unique` constraint to ensure no duplicate values exist for a field.
   - **Example**: To create a unique index on the `email` field:
     ```javascript
     db.customers.createIndex({ email: 1 }, { unique: true });
     ```
   - **Duplicate Key Error**: If an insert operation attempts to add a document with an existing email, MongoDB will return a duplicate key error, enforcing data integrity.
   - MongoDB only creates the unique index if there is no duplication in the field values for the index field/s.

#### **3. Viewing Existing Indexes**
   - **Command**: Use `getIndexes` to list all indexes on a collection.
     - **Example**:
       ```javascript
       db.customers.getIndexes();
       ```
   - **Atlas UI**: You can also view indexes on the MongoDB Atlas UI by navigating to the collection’s “Indexes” section.

#### **4. Using `explain` to Analyze Query Index Usage**
   - Use `explain()` in a collection when running a query to see the Execution plan. This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.).
   - There are three main verbosity modes that provide different levels of detail in the output:
	   - **`"queryPlanner"` (default)**: Provides basic information about the query plan and index usage.
	   - **`"executionStats"`**: Includes detailed statistics on execution, such as the number of documents examined and time taken `executionTimeMillis`.
		   - **`totalKeysExamined`**: Represents the number of index keys MongoDB scanned during the query. This is relevant if an index is used and indicates the number of entries checked within the index.
		   - **`totalDocsExamined`**: Represents the number of documents MongoDB actually retrieved and examined from the collection after looking up the index keys.
		   - Ideally, you want `totalDocsExamined` to be as low as possible relative to `totalKeysExamined` for efficiency, meaning MongoDB can quickly locate the relevant documents without needing to scan many of them directly.
	   - **`"allPlansExecution"`**: Provides detailed information on all candidate plans evaluated, including stats for each one.
   - **Command**:
     ```javascript
     db.customers.find({ birthdate: { $gt: ISODate("1995-08-01") } }).explain();
     ```
     ![Screenshot (107).png](assets/Screenshot%20(107).png)
   - **Understanding the Query Plan**:
    - **Winning Plan**: This section of the `explain` output shows the most efficient way MongoDB executes the query.
    - A `winningPlan` is a document that contains information about the query and the method that was used to execute the query.
    - The `IXSCAN` stage indicates the query is using an index and what index is being selected.
	- The `COLLSCAN` stage indicates a collection scan is perform, not using any indexes.
	- The `FETCH` stage indicates documents are being read from the collection.
		- Here if we return only the birthdate field, this fetch stage won't be necessary, and the index covers the query.
	- The `SORT` stage indicates documents are being sorted in memory.
	- The `PROJECTION` stage:
		- Applies the specified projection to limit fields in the output.
		- Reduces the size of documents returned, which can improve performance.
	- **`LIMIT` and `SKIP`**:
		- Limits the number of documents returned or skips a number of documents.
		- These stages are typically seen in paginated queries.

#### **5. Index Optimization Example**
   - **Query with Sorting**: If a query sorts by `email` and filters by `birthdate`, MongoDB may perform an in-memory sort if an index does not cover both fields.
		```js
db.customers.find({birthdate: {$lt: ISODate("1966-08-01")}}).sort({email: 1}).explain()
		```
		   
	    ![Screenshot (109).png](assets/Screenshot%20(109).png)
	    ![Screenshot (111).png](assets/Screenshot%20(111).png)
     - **Optimization Tip**: Creating a compound index on `birthdate` and `email` would allow MongoDB to optimize both the filter and the sort, improving query performance.
     - The query optimizer considered using email_1 index, which would support the sort but not the query filter, but it was rejected
     
	    ```js
    db.customers.explain().find({accounts: 871666})
	```
![Pasted image 20241109174430.png](assets/Pasted%20image%2020241109174430.png)
The `winningPlan` is a collection scan `COLLSCAN` because there are no indexes that can be used for the accounts field
#### **6. Collection Scans and Index Creation Needs**
   - **Collection Scan**: If no index is available, MongoDB performs a full scan on the collection, which is slower.
   - **Example**: If frequent queries use a specific field (e.g., `accounts`), create an index on that field to avoid collection scans.

#### **Summary**
   - In this lesson, we covered how to create single field indexes with `createIndex`, enforce uniqueness using `unique`, and identify indexes with `getIndexes`.
   - **Analyze Query Plans**: Use `explain` to check if queries are optimized and see how indexes affect query performance.

___
### Lesson 3: Creating a Multikey Index in MongoDB

#### **1. Defining Multikey Indexes**
   - **Definition**: A multikey index is an index on a field that contains an array. MongoDB indexes each unique element within the array as an individual index entry.
   - **Supported Data Types**: Multikey indexes can be created on arrays that contain primitive values, subdocuments, or nested arrays.

#### **2. Creating Multikey Indexes**
   - **Single Field Multikey Index**: An index created on a single array field is automatically treated as a multikey index.
     - **Example**: To create a multikey index on the `accounts` array field in the `customers` collection:
       ```javascript
       db.customers.createIndex({ accounts: 1 });
       ```
   - **Compound Multikey Index**: Multikey indexes can also be part of a compound index, but only **one array field** is allowed per index.
     - **Example**:
       ```javascript
       db.customers.createIndex({ accounts: 1, email: 1 });
       ```
   - **Limitation**: An index can have multiple fields, but **only one of these fields can be an array**. MongoDB will not create an index if more than one field in the index specification is an array.

#### **3. Index Decomposition Process**
   - **Decomposition**: When creating a multikey index, MongoDB "decomposes" the array field into separate entries, creating an index entry for each unique value within the array.
   - **Example**: If a document in the `accounts` array has `[101, 202, 303]`, MongoDB will create three individual index entries, one for each account number.

#### **4. Use Case: Indexing Array Fields**
   - **Purpose**: Multikey indexes improve query performance for array fields, allowing efficient lookups even when querying against individual elements within an array.
   - **Example**: If we want to find customers with a specific account number, creating a multikey index on the `accounts` field speeds up such queries and avoids a full collection scan.

#### **5. Checking for Existing Indexes**
   - **Command**: Use the `getIndexes` command to list all indexes on a collection.
     ```javascript
     db.customers.getIndexes();
     ```
   - This command lists all the indexes, including any multikey indexes.

#### **6. Analyzing Index Usage with `explain`**
   - **Command**: Use `explain` to analyze whether a query uses an index or performs a collection scan.
     - **Example**:
       ```javascript
       db.customers.find({ accounts: 627788 }).explain();
       ```
       ![Pasted image 20241109181106.png](assets/Pasted%20image%2020241109181106.png)
   - **Index Scan (IXSCAN)**: When the index is used, the `IXSCAN` stage appears in the `winningPlan`, showing that MongoDB scans the index for matching values.
   - **Fetch Stage**: After identifying relevant documents with `IXSCAN`, MongoDB typically performs a `FETCH` stage to retrieve the complete documents, as each array value is stored as a separate index entry. (multikey indexes need to fetch the documents after the IXSCAN stage because the index entries have each of the array values stored separately)

#### **7. Summary**
   - **Multikey Index**: Any index on an array field is a multikey index. It enables efficient querying and supports both single field and compound indexes, with the restriction of one array field per index.
   - **Execution Plan**: Use `explain` to verify that the index is applied in the `IXSCAN` stage for optimized queries.

___
### Lesson 4: Working with Compound Indexes in MongoDB

#### **1. Overview of Compound Indexes**

- **Definition**: An index that spans multiple fields in a document.
- **Multikey Support**: Compound indexes can include an array field but are limited to **one array field** per index.

---

#### **2. Benefits of Compound Indexes**

- Efficiently supports queries on multiple fields.
- Enhances query performance by reducing document scans and sorting.
- Can **cover queries** to avoid fetching entire documents if all required fields are in the index.

---

#### **3. Field Ordering in Compound Indexes**

- **Importance of Order**: MongoDB indexes are ordered, and query fields must match the prefix order of the index for optimal usage.
- **Recommended Order** ([The ESR (Equality, Sort, Range) Rule](https://www.mongodb.com/docs/manual/tutorial/equality-sort-range-rule/#the-esr--equality--sort--range--rule "Permalink to this heading")):
	- The recommended order of indexed fields in a compound index is Equality, Sort, and Range. Optimized queries use the first field in the index, Equality, to determine which documents match the query. The second field in the index, Sort, is used to determine the order of the documents. The third field, Range, is used to determine which documents to include in the result set.
    1. **Equality Filters**: Fields tested for equality (`field = value`) should appear first.
		![Screenshot (119).png](assets/Screenshot%20(119).png)
    2. **Sort Fields**: Fields used for sorting come next, following the query's sort order.
	    ![Screenshot (121).png](assets/Screenshot%20(121).png)
    3. **Range Filters**: Fields used in range queries (`>=`, `<=`, etc.) should come last.
	
	In general, it's recommended that range and sort should be placed after equality in the index to avoid in-memory sort or filtering.

---
#### **4. Example: Query Optimization**

- **Scenario**:
    - Query: Find active customers born after `1977-01-01`.
    - Sort results by `birthdate` (descending) and `name` (ascending).
- **Before Optimization**:
    - MongoDB uses a single-field index (e.g., on `birthdate`) for scanning.
    - Performs a **fetch stage** to filter by `active`.
    - Sorts results **in-memory**, which is inefficient.
- **Optimized Index**:
    
    ```js
db.customers.createIndex({ active: 1, birthdate: -1, name: 1 });
```
    
    - Places `active` first (equality).
    - Includes `birthdate` and `name` in the sort order specified in the query.
    
    We can also use:
    ```js
db.customers.createIndex({ active: 1, birthdate: -1, name: 1 });
```
    
```js
    db.customers.find(
  { active: true, birthdate: { $gte: new Date("1977-01-01") } }
).sort({ birthdate: -1, name: 1 });
```
- **Outcome**:
    - Index scan retrieves filtered and sorted results efficiently.
    - Reduced in-memory operations.
---

#### **5. Covering Queries**

- **Definition**: A query is "covered" if all fields it accesses are included in the index.
- **Benefits**:
    - Eliminates the fetch stage, speeding up execution.
- **Example**:
    
    ```javascript
    db.customers.find(
      { active: true, birthdate: { $gte: new Date("1977-01-01") } },
      { name: 1, birthdate: 1, _id: 0 }
    ).explain();
    ```
    
    - Query projects only `name` and `birthdate`, both of which are in the index.
    - No fetch stage is performed, as the index fully satisfies the query.
    - when we run the `explain()` command, the execution plan shows only two stages:
		- IXSCAN - Index scan using the compound index
		- PROJECTION_COVERED - All the information needed is returned by the index, no need to fetch from memory

- If a query projects additional fields that we don't use to filter or sort by, we can include those fields at the end of the field list when we create the index. This way we use the index to cover the query and avoid fetching the documents.
	![Pasted image 20241121200357.png](assets/Pasted%20image%2020241121200357.png)

---

#### **6. Verification with `explain`**

- **Command**: Use `explain` to confirm index usage and identify optimization opportunities.
- **Key Stages**:
    - **`IXSCAN`**: Shows the index scan being used.
    - **`FETCH`**: Indicates document retrieval from the collection (avoid if possible).
- **Optimized Example**:
    - After creating the compound index, the query uses the index scan (`IXSCAN`) and avoids unnecessary fetch operations.

---
Consider this compound index:

```js
{ "item": 1, "location": 1, "stock": 1 }
```

The index has these index prefixes:
- `{ item: 1 }`
- `{ item: 1, location: 1 }`

MongoDB can use the compound index to support queries on these field combinations:
- `item`
- `item` and `location`
- `item`, `location`, and `stock`

MongoDB can also use the index to support a query on the `item` and `stock` fields, since the `item` field corresponds to a prefix. However, only the `item` field in the index can support this query. The query cannot use the `stock` field which follows `location`.

___

```js
db.leaderboard.createIndex( { score: -1, username: 1 } )
```

Compound indexes support sort operations that match either the sort order of the index, or the reverse sort order of the index as MongoDB can traverse a compound index in either direction.
```js
db.leaderboard.find().sort( { score: 1, username: -1 } )
```

### Unsupported Queries

Compound indexes cannot support queries where the sort order does not match the index or the reverse of the index. As a result, the `{ score: -1, username: 1 }` index **cannot** support sorting by ascending `score` values and then by ascending `username` values, such as this query:

```js
db.leaderboard.find().sort( { score: 1, username: 1 } )
```

Additionally, for a sort operation to use an index, the fields specified in the sort must appear in the same order that they appear in an index. As a result, the above index cannot support this query:

```js
db.leaderboard.find().sort( { username: 1, score: -1, } )
``````

___
#### **7. Summary**

- **Compound Index Essentials**:
    - Define with equality, sort, and range fields in that order.
    - Use `createIndex` to implement and `explain` to verify.
- **Performance Gains**:
    - Faster query execution.
    - Reduced in-memory sorting and filtering.
    - Covered queries eliminate fetch stages for further speed improvements.

___

A summary of the key points about MongoDB index usage for sorting and filtering, along with examples:

### Key Points

1. **Index Usage for Sorting**:
    
    - MongoDB can efficiently use an index for sorting if the query includes equality conditions for all the prefix keys that precede the sort keys in the index.
2. **Range Filters and Sorting**:
    
    - MongoDB cannot perform an index sort on the results of a range filter. To optimize performance, place the sort predicate before the range filter so MongoDB can use a non-blocking index sort.
3. **Order of Equality Matches**:
    
    - The order of equality match fields in the query does not need to match the order in the index. However, all equality match fields must come before any range conditions or sort operations for MongoDB to use the index efficiently.
4. **Aggregation Pipeline Optimization**:
    
    - Use `$match`, `$limit`, and `$skip` stages to filter documents early in the pipeline.
    - Place `$match` at the beginning of the pipeline to leverage indexes.
    - `$match` followed by `$sort` at the start of the pipeline can use an index efficiently.

### Examples

#### Example 1: Index Usage for Sorting

Consider an index on the `data` collection:

```js
db.data.createIndex({ a: 1, b: 1, c: 1, d: 1 })
```

- **Supported Sort Operation**:
    
    ```js
    db.data.find({ a: 5 }).sort({ b: 1, c: 1 })
    ```
    
    - The query includes an equality condition on `a`, which is the prefix key preceding the sort keys `b` and `c`. MongoDB can use the index efficiently.
- **Unsupported Sort Operation**:
    
    ```js
    db.data.find({ a: { $gt: 2 } }).sort({ c: 1 })
    ```
    
    - The query does not include an equality condition on `a` and does not include any condition on `b`, which are the prefix keys preceding the sort key `c`. MongoDB cannot use the index efficiently for sorting.

#### Example 2: Range Filters and Sorting

Consider an index on the `cars` collection:

```js
db.cars.createIndex({ price: 1, model: 1 })
```

- **Inefficient Query with Range Filter Before Sort**:
    
    ```js
    db.cars.find({ price: { $gte: 15000 } }).sort({ model: 1 })
    ```
    
    - MongoDB cannot use the index to sort the results by `model` because the range filter on `price` is applied first, resulting in a blocking sort operation.
- **Efficient Query with Sort Before Range Filter**:
    
    ```js
    db.cars.find().sort({ model: 1 }).find({ price: { $gte: 15000 } })
    ```
    
    - MongoDB can use the index to sort the documents by `model` first and then apply the range filter on `price`, resulting in a non-blocking sort operation.

#### Example 3: Order of Equality Matches

Consider the following index on the `data` collection:

```js
db.data.createIndex({ a: 1, b: 1, c: 1, d: 1 })
```

- **Supported Query**:
    
    ```js
    db.data.find({ a: 5, b: 3 }).sort({ c: 1 })
    ```
    
    - The equality matches on `a` and `b` come before the sort on `c`, allowing MongoDB to use the index efficiently.
- **Order of Equality Matches**:
    
    ```js
    db.data.find({ b: 3, a: 5 }).sort({ c: 1 })
    ```
    
    - Even though the order of `a` and `b` is reversed in the query, MongoDB can still use the index efficiently because all equality matches (`a` and `b`) come before the sort key (`c`).

#### Example 4: Aggregation Pipeline Optimization

Consider a collection `sales` with the following documents:

```js
db.sales.insertMany([  { _id: 0, items: [{ item_id: 43, quantity: 2, price: 10, name: "pen" }, { item_id: 2, quantity: 1, price: 240, name: "briefcase" }] },  { _id: 1, items: [{ item_id: 23, quantity: 3, price: 110, name: "notebook" }, { item_id: 103, quantity: 4, price: 5, name: "pen" }, { item_id: 38, quantity: 1
```

## Example

The following query searches the `cars` collection for vehicles manufactured by Ford that cost more than $15,000 dollars. The results are sorted by model:

```js
db.cars.find(   
	{       
		manufacturer: 'Ford',
		cost: { $gt: 15000 }
	}).sort( { model: 1 } )
```

The query contains all the elements of the ESR Rule:

- `manufacturer: 'Ford'` is an equality based match
    
- `cost: { $gt: 15000 }` is a range based match, and
    
- `model` is used for sorting
    

Following the ESR rule, the optimal index for the example query is:

```js
{ manufacturer: 1, model: 1, cost: 1 }
```

___

### Lesson 5: Deleting MongoDB Indexes

#### **1. Overview of Index Deletion**
   - **Indexes and Performance**: Indexes speed up queries but come with a write cost:
     - Updating index keys for every insert or update operation.
     - Too many indexes can degrade system performance.
   - **Reasons to Delete Indexes**:
     - Index is unused or redundant.
     - To improve overall system efficiency.

---

#### **2. Risks and Precautions Before Deletion**
   - **Query Impact**: Deleting an index used by queries forces MongoDB to perform a **collection scan**, significantly reducing query performance.
   - **Default Index**: The `_id` index cannot be deleted.
   - **Recreating an Index**: Rebuilding a deleted index takes time and resources.
   - **Best Practice**: Use `hideIndex()` before deleting an index to test its necessity without permanent removal.

---

#### **3. Hiding and Unhiding Indexes**
   - **Hide an Index**:
     ```javascript
     db.collection.hideIndex("index_name");
     ```
     - Hidden indexes are ignored by queries but continue to update keys.
   - **Unhide an Index**:
     ```javascript
     db.collection.unhideIndex("index_name");
     ```
     - Unhiding is faster than recreating the index.

---

#### **4. Identifying Redundant Indexes**
   - **Example**: A compound index (`{username: 1, active: 1}`) can support queries for both:
     - `{username: 1}`
     - `{username: 1, active: 1}`
   - In this case, a single-field index on `username` becomes **redundant**.

---

#### **5. Deleting Indexes**
   - **Retrieve Indexes**:
     ```javascript
     db.collection.getIndexes();
     ```
     - Displays all indexes in the collection.
     - Each index has a name and a key.
     - In a production environment, it's best practice to hide the index before deleting it. This avoids having to recreate it later if we realize the index was needed for a query.
   - **Delete an Index**:
     ```javascript
     // Deleting by name
     db.collection.dropIndex("active_1_birthdate_-1_name_1");
     // Deleting by key
     db.collection.dropIndex({active: 1, birthdate: -1, name: 1});
     ```
     - Deletes a specific index by name or key.
   - **Delete Multiple Indexes**:
     ```javascript
     db.collection.dropIndexes();
     ```
     - Deletes all indexes except the `_id` index.
     - To delete specific indexes, provide one index or an array of index names.

---

#### **6. Deleting Indexes in MongoDB Atlas UI**
   - Navigate to the collection's **Indexes** section.
   - Click **Drop Index** next to the desired index.
   - For multiple indexes, use the `dropIndexes()` method in the command line.

---

#### **7. Example Workflow**
   - **Scenario**: Deleting the compound index `{active: 1, birthdate: -1, name: 1}`.
   - **Steps**:
     1. **View Indexes**:
        ```javascript
        db.customers.getIndexes();
        ```
     2. **Hide the Index** (optional):
        ```javascript
        db.customers.hideIndex("active_1_birthdate_-1_name_1");
        ```
     3. **Delete the Index**:
        ```javascript
        db.customers.dropIndex("active_1_birthdate_-1_name_1");
        ```

---

#### **8. Key Commands**
   - **Delete One Index**:
     ```javascript
     db.collection.dropIndex("index_name");
     ```
   - **Delete Multiple Indexes**:
     ```javascript
     db.collection.dropIndexes();
     ```
   - **Hide an Index**:
     ```javascript
     db.collection.hideIndex("index_name");
     ```
   - **Unhide an Index**:
     ```javascript
     db.collection.unhideIndex("index_name");
     ```

---

#### **9. Summary**
   - **Deleting an Index**:
     - Use `dropIndex()` for a single index and `dropIndexes()` for multiple indexes.
   - **Precaution**: Hide indexes with `hideIndex()` to assess their necessity before deletion.
   - **MongoDB Atlas**: Use the **Drop Index** option for a graphical interface approach.

___
___

## Unit 10: MongoDB Atlas Search

### Lesson 1: Using Relevance-Based Search and Search Indexes

### **Introduction to Atlas Search**

Atlas Search is a powerful, relevance-based search tool built directly into MongoDB Atlas. Unlike traditional database searches that focus on exact matches, Atlas Search enables users to retrieve records ranked by relevance to a search term. This eliminates the need for separate search software, simplifying development and reducing system complexity.

---

### **Use Cases for Atlas Search**

Atlas Search is ideal for applications where users need to find information based on vague or broad search terms. For example, searching for "Atlas Search" in MongoDB documentation retrieves results ranked by their relevance to the term. Such functionality is critical for applications prioritizing user-centric search experiences.

---

### **Key Differences: Database Search vs. Relevance-Based Search**

- **Database Search**:
    - Used by developers or database administrators.
    - Focuses on retrieving specific records through efficient queries (exact match efficiency).
    - Relies on database indexes (e.g., B-trees or hashed indexes).
- **Relevance-Based Search**:
    - Designed for end-users searching for information based on keywords or phrases.
    - Returns results ranked by relevance.
    - Powered by search indexes configured for specific search requirements.
    - Prioritizes a richer search experience.

---

### **Challenges with Search in Applications**

Building relevance-based search functionality in applications is challenging because it requires:

- Complex computations to rank results based on relevance.
- Often relying on external search tools (e.g., Elasticsearch) separate from the database, which adds overhead.
- Maintaining synchronization between the database and the search system.

**Atlas Search Solution**: MongoDB Atlas integrates **Apache Lucene**, an open-source search library, to provide relevance-based search directly within the database. This removes the need for separate systems, reducing maintenance and accelerating development.

---

### **Search Index vs. Database Index**

1. **Database Index**:
    - Speeds up frequent database queries by efficiently locating specific records.
    - Used by developers and admins for performance optimization.
2. **Search Index**:
    - Configures how records are processed and ranked for relevance-based search.
    - Designed to enhance the search experience for end-users by providing accurate and prioritized results.

---

### **Components of an Atlas Search Index**

1. **Analyzers**:
    - Determine how text is processed (e.g., tokenizing text into searchable units).
    - Default analyzer: `lucene.standard`.
2. **Mapping Type**:
    - Specifies whether fields are indexed dynamically or require explicit definitions.
3. **Memory Management**:
    - Option to store entire documents in memory for improved performance during post-aggregation.
4. **Field Mappings**:
    - Define which fields in a document are indexed and how they are processed for search.

---

### **Key Takeaways**

1. **When to Use Atlas Search**:
    - When your application needs advanced, relevance-based search capabilities.
    - To avoid the complexity of managing separate search systems outside the database.
2. **Advantages**:
    - Simplifies development by integrating search into MongoDB Atlas.
    - Leverages industry-standard search technology (Apache Lucene).
    - Eliminates synchronization issues between database and search systems.
3. **Next Steps**:
    - Create and configure a search index using the discussed components.
    - Explore ways to enhance your application with relevance-based search functionality.

---

### **Conclusion**

Atlas Search integrates relevance-based search seamlessly into MongoDB Atlas, leveraging Apache Lucene to enhance user experience. By understanding the differences between relevance-based and database searches and configuring search indexes effectively, developers can build applications with robust, intuitive search functionality.

___

### Lesson 2: Creating a Search Index with Dynamic Mapping
### **Creating a Search Index with MongoDB Atlas**

In this lesson, we will walk through the process of creating a **search index** in MongoDB Atlas, using **dynamic mapping** to allow for quick and efficient relevance-based searches. By the end, we’ll query the data and explore the results to see how the search index works in action.

---

### **Search Index Overview**

A **search index** defines how a relevance-based search should be performed on your data. It is different from a database index, which is used to optimize database queries.

---

### **Steps to Create a Search Index**

1. **Access the Search Tab**:
    - In the MongoDB Atlas interface, navigate to the **Search tab**.
    - This section displays all existing search indexes for the current database.
2. **Create a New Search Index**:
    - Click the **Create Search Index** button.
    - Use the **Visual Editor** for a guided setup process.
3. **Set Database and Collection**:
    - Select the appropriate database and collection for the index.  
        Example: Using the **birds** collection in the **bird guide** database.
4. **Configure the Search Index**:
    - **Dynamic Mapping**:
        - Enable this option (default setting) to index all fields automatically, except for fields such as Booleans, ObjectIDs, and timestamps.
        - This allows the search algorithm to work across all fields without manual configuration.
    - **Analyzers**:
        - The default analyzer is `lucene.standard`, part of the open-source **Lucene** search engine library.
        - This analyzer tokenizes text into searchable chunks and is ideal for general-purpose searches.
        - MongoDB offers over 40 analyzers for multilingual and specific use cases.
    - Save the configuration and click **Create Search Index**.
        
        > _Note: Index creation may take some time, and you’ll be notified once it’s ready._
     
![Pasted image 20241122190611.png](assets/Pasted%20image%2020241122190611.png)




---

### **Running a Query**

Once the search index is created, you can run a query to test its functionality:

1. **Perform a Search**:
    - Example: Query the term **"blue"** to find all bird species with blue plumage.
    - Click **Search** after entering the term.
2. **Understanding the Results**:
    - Results are ranked using a **relevance score**, calculated by Lucene's scoring logic.
    - The score depends on how prevalent the query term is across the indexed fields.
    - Higher scores indicate more relevant matches, which are displayed at the top.

---

### **Advanced Features**

As you advance with Atlas Search, you can:
- **Adjust Scoring**: Assign weights to specific fields, making certain fields more influential in the relevance calculation.
- **Custom Field Mappings**: Configure specific fields to refine search results further.

---

### **Key Takeaways**

1. **Dynamic Mapping**:
    - Indexes all fields automatically, making it the fastest way to set up a search index.
2. **Lucene Integration**:
    - MongoDB Atlas leverages the **Apache Lucene** library for powerful relevance-based search capabilities.
3. **Relevance Scoring**:
    - Ensures that search results are ranked based on how well they match the query.

---

### **Conclusion**

You’ve successfully created your first search index with dynamic mapping, queried data, and understood how relevance scoring works. With this foundation, you can now explore Atlas Search further by experimenting with custom configurations, analyzers, and field mappings to build advanced search functionalities for your applications.

Start by creating a search index on your own dataset and testing it with real queries!

___
#### **The Lab**

```json
{
    "name": "sample_supplies-sales-dynamic",
    "searchAnalyzer": "lucene.standard",
    "analyzer": "lucene.standard",
    "collectionName": "sales",
    "database": "sample_supplies",
    "mappings": {
        "dynamic": true
    }
}
```
 
**Explanation:**
The `/app/search_index.json` file contains the Atlas search index definition. Here is an overview of the fields defined:
- The `name` field specifies the name of the index as it will appear in MongoDB Atlas.
- The `searchAnalyzer` specifies the analyzer to apply to query text before searching with it. In this example, we use the default `lucene.standard` analyzer.
- The `analyzer` defines how Atlas search will turn a string field's contents into searchable terms. It defines the tokenizer used to extract tokens from the > text and filters to use. In this example, we use the default `lucene.standard` analyzer.
- The `collectionName` and `database` define the collection and database on which to create the index.
- The `mappings` object is used to define the mapping to use, `dynamic` or `static`. In this example, we are using `dynamic` which means that we want Atlas > Search to automatically index all supported field types.

Navigate to the [terminal](https://play.instruqt.com/embed/mongodb/tracks/creating-a-search-index-with-dynamic-mapping/challenges/creating-a-search-index-with-dynamic-mapping/assignment#tab-1) tab at the top of the screen and run the following command to create the Atlas Search index:
`atlas clusters search indexes create --clusterName myAtlasClusterEDU -f /app/search_index.json`

To verify that the index creation has started, please run the following command in the terminal:
`atlas clusters search indexes list --clusterName myAtlasClusterEDU --db sample_supplies --collection sales`

If the index has been created (or is in the process of being created) you should see output similar to the following in your terminal, the ID field value will differ:

```js
ID                         NAME                            DATABASE     COLLECTION   
659c4d8278800d64abe1d3e5   sample_supplies-sales-dynamic   sample_supplies   sales 
TYPE
search
```


**Important**

If you get see the following error `MAXIMUM_INDEXES_FOR_TENANT_EXCEEDED`, wait for 15-20 seconds and retry the command to create the search index. If this fails again, you should login in to your Atlas account and review the existing Atlas Search Indexes. You will need to delete one or more before you can complete this exercise. This is due to the limitation of a maximum of three (3) Atlas Search Indexes on M0 free tier clusters.

`Error: POST https://cloud.mongodb.com/api/atlas/v1.0/groups/67283b5b8db03b7272b42bcc/clusters/myAtlasClusterEDU/fts/indexes: 400 (request "MAXIMUM_INDEXES_FOR_TENANT_EXCEEDED") The maximum number of FTS indexes has been reached for this instance size.`


In the [mongosh](https://play.instruqt.com/embed/mongodb/tracks/creating-a-search-index-with-dynamic-mapping/challenges/using-a-search-index-with-dynamic-mapping/assignment#tab-0) tab, modify the `$search` in the pipeline variable below and run the aggregation pipeline. You need to set the `text.query` value to `notepad` and the `text.path.wildcard` to `*`.

```js
var pipeline = [ 
{ 
	$search: { 
		index: "sample_supplies-sales-dynamic", 
		text: { 
			query: "notepad", 
			path: { "wildcard": "*" } 
	} } }, 
	{ 
	$set: { 
		score: { 
			$meta: "searchScore" } 
			} 
} 
] 
db.sales.aggregate(pipeline)
```

**Explanation:**
- `$search` is an aggregation stage used to perform a full-text search on the specified field(s) by using an Atlas Search Index.
- `index` is the name of the index we've created in the previous challenge.
- `text` is the search operator that the `$search` stage will use. It performs a full-text search using the analyzer we have specified in the index > configuration. For your convenience, you can review the index configuration in the `/app/search_index.json` file available under the [Index > Configuration](https://play.instruqt.com/embed/mongodb/tracks/creating-a-search-index-with-dynamic-mapping/challenges/using-a-search-index-with-dynamic-mapping/assignment#tab-2) tab.
- `query` specifies the string to search for.
- `path` specifies the fields to search. In our example, we are using a `wildcard` path so all the fields will be searched.
- The `$set` stage is used to add the `score` field to the result set.
- `$meta` is an aggregation operator to return the metadata associated to a document. In this case, we are interested in the `searchScore` of a search query. The score is based on how much that record matches the search term and on the rules described in the index. In this case, with a dynamic index, all of the fields were treated equally.


___

### Lesson 3: Creating a Search Index with Static Field Mapping

### **Using Static Mapping for Atlas Search**

In this lesson, we will learn how to use **static mapping** for Atlas Search to optimize search performance by indexing only specific fields. While the default **dynamic mapping** indexes all fields, static mapping focuses on the fields most relevant to the application’s users, improving efficiency and accuracy.[]()

---

### **What is Static Mapping?**

- **Dynamic Mapping**: Automatically indexes all fields in a collection.
    - Great for quick setup but can be resource-intensive for collections with many fields.
- **Static Mapping**: Allows developers to define specific fields to be indexed.
    - Ideal for optimizing performance and ensuring relevant search results.
    - It's called static because the fields being queried are always the same.

---

### **When to Use Static Mapping**

Static mapping is useful when:

- Your data has numerous fields, but only a subset is relevant to user searches.
- You want to reduce the processing overhead of indexing unnecessary fields.
- You aim to deliver more focused and efficient search results.

---

### **Steps to Create a Static Search Index**

1. **Access the Search Tab**:
    
    - Navigate to the **Search tab** in the MongoDB Atlas interface.
    - Click on the **Create Search Index** button.
2. **Select Database and Collection**:
    - Choose the database and collection for your search index.  
        Example: The **birds** collection in the **bird guide** database.
3. **Configure Field Mappings**:
	Let's imagine we're creating a bird guidebook application. In this case, the most important fields that contain information that our user might search by are common name, scientific name, habitat, and diet. Dynamic mapping indexes all of the fields present in a collection. In order to save time and processing power, we'll toggle off dynamic mapping and only index the fields that we anticipate will contain the search terms that our end user might query
	![Pasted image 20241124130545.png](assets/Pasted%20image%2020241124130545.png)
	![Pasted image 20241124130549.png](assets/Pasted%20image%2020241124130549.png)
    - Toggle **off** dynamic mapping to disable automatic indexing of all fields.
	     ![Pasted image 20241124130646.png](assets/Pasted%20image%2020241124130646.png)
    - Use the **Add Field** option to manually select specific fields to index.
        - Example: For a bird guide application, select fields like:
            - `common name`
            - `scientific name`
            - `habitat`
            - `diet`
    - Set the **data type** for each field (e.g., `string` for text fields).
5. **Save and Create the Index**:
    
    - After defining all field mappings, click **Save Changes**.
    - Finalize by clicking the **Create Search Index** button.
        
        > _Note: It may take some time for the index to be generated, and you’ll be notified upon completion._
        

---

### **Testing the Static Search Index**

1. **Run a Query**:
    
    - Example: Search for **"bluebird"**.  
        The results will show matches where **bluebird** appears in the `common name` field.
2. **Understand the Limitations**:
    
    - Searching for terms in fields not indexed (e.g., `wingspan`) will return no results.  
        This is because static mapping restricts the search scope to predefined fields.

---

### **Advantages of Static Mapping**

- **Efficiency**: Reduces resource consumption by focusing only on relevant fields.
- **Precision**: Improves the quality of search results by limiting indexing to fields users care about.
- **Customizability**: Allows developers to tailor search functionality to specific application requirements.

---

### **Key Takeaways**

- **Static Mapping**:
    - Best for collections with many fields where only a subset is relevant to the user.
    - Improves search performance by indexing only selected fields.
- **Dynamic Mapping**:
    - Good for general-purpose searches but less efficient for large collections.
- **Index Limitations**:
    - Only indexed fields are searchable, so it’s important to carefully choose the fields based on user needs.

---

### **Conclusion**

Static mapping in Atlas Search empowers developers to create targeted, efficient search experiences by focusing on the most relevant fields. By understanding how to configure and use static mappings, you can deliver optimized search functionality tailored to your application’s needs.

Try creating a static search index for your own data and see how it enhances both performance and user experience!

___

**The Lab**
We have a dataset that contains records from an office supply company. The records contain information about orders, products, and customers. Imagine we are working on an application with a dashboard for administrators keeping track of sales at our office supply company. We want to add search functionality so that administrators can easily find sales matching their criteria. First we need to create a search index for the `sales` collection!

```json
{
    "name": "sample_supplies-sales-static",
    "searchAnalyzer": "lucene.standard",
    "analyzer": "lucene.standard",
    "collectionName": "sales",
    "database": "sample_supplies",
    "mappings": {
        "dynamic": false,
        "fields": {
            "storeLocation": {
                "type": "string"
            }
        }
    }
}
```

## Lab Instructions

You will be connected to your Atlas cluster and to the `sample_supplies` database. Use the `sales` collection in this lab.

1. Copy and paste the following command to use `mongosh` to connect to your Atlas cluster. For simplicity, we have included your connection string in the command by using the bash variable `MY_ATLAS_CONNECTION_STRING` and appended the correct database for this lab.
    
    ```shell
    mongosh -u myAtlasDBUser -p myatlas-001 $MY_ATLAS_CONNECTION_STRING/sample_supplies
    ```
    
2. Next, create an aggregation pipeline, which will contain two stages. (Forgot the method for aggregation? Check the hint below!)
    
3. Create a `$search` stage that uses the index `sample_supplies-sales-static` with query value of `London` and a path wildcard of `*`.
    
4. Create a `$set` stage that adds a new field `score` to the document to represent the Atlas Search score `{ $meta: "searchScore" }`.
    
5. Review the documents outputted and note they now include a score, the `object_id`, and the fields that matched the search term. The score is based on how much that record matches the search term and on the rules described in the index. In this case, with a static mapping for the index, the field `storeLocation` is what is used for the score calculation


The following code is a snippet from a search index. What type of field mapping does this search index use? (Select one.)

```json
{
    "mappings": {
        "dynamic": false,
        "fields": {
            "common_name": [
            {
                "dynamic": true,
                "type": "document"
            },
            {
                "type": "string"
            }
            ]
        }
    }
}
```

![Pasted image 20241124132455.png](assets/Pasted%20image%2020241124132455.png)

___

### Lesson 4: Using $search and Compound Operators

The `$compound` operator within the `$search` aggregation stage allows us to give weight to different field and also filter our results without having to create additional aggregation stages. The four options for the compound operator are "must", "mustNot, "should", and "filter".
- **must**: Includes only results that meet specific conditions.
- **mustNot**: Excludes results that match the condition.
- **should**: Assigns a score to results that meet the condition, influencing their ranking.
- **filter**: Excludes results that don't match the condition but doesn’t affect the score.

---

### **Steps to Implement a `$compound` Operator**

1. **Create a Search Index**:
    - Ensure a search index (e.g., dynamic mapping) is already configured.
2. **Set up the Aggregation Pipeline**:
    - Navigate to the **Aggregation tab** in MongoDB Atlas.
    - Select **$search** from the dropdown.
3. **Write the `$search` Stage**:
    - Define the `$compound` operator within the `$search` stage.
    - Specify the search logic using clauses like `must` and `should`.

---

### **Example: Bird Search Application**

Let’s create an application for bird enthusiasts where users can search for birds commonly found in **grasslands** with a **wingspan greater than 75 cm**.

#### **Logic**:

- Results **must** include birds inhabiting grasslands.
- Results **should** prioritize birds with a wingspan greater than 75 cm by assigning a higher score.

#### **Pipeline Configuration**:

Here’s how the pipeline might look:

```json
{
  "$search": {
    "index": "default",
    "compound": {
      "must": [
        {
          "text": {
            "query": "grasslands",
            "path": "habitat"
          }
        }
      ],
      "should": [
        {
          "range": {
            "path": "wingspan",
            "gt": 75,
            "score": { "boost": { "value": 5 } }
          }
        }
      ]
    }
  }
}
```

---

### **Testing and Results**

- **Execute the Query**:
    - Run the pipeline in the **Aggregation Pipeline Tester** in MongoDB Atlas.
    - The results will display records ordered by their **relevance score**.
- **Understand the Scoring**:
    - Birds inhabiting grasslands and having a wingspan > 75 cm receive the highest scores.
    - The `boost` value of 5 increases the priority of the wingspan condition in ranking.

#### **Example Output**

- **Bird A**: Grasslands, wingspan 80 cm → High score.
- **Bird B**: Grasslands, wingspan 70 cm → Lower score.
- **Bird C**: Not in grasslands → Excluded.

---

### **Advantages of Field Weighting with `$compound`**

1. **Customization**:
    - Tailor search results to user preferences or application needs.
2. **Efficiency**:
    - Rank results meaningfully, reducing noise in large datasets.
3. **Flexibility**:
    - Combine multiple conditions (e.g., geography, size, or user behavior).

---

### **Key Takeaways**

- The `$compound` operator in Atlas Search enables advanced search customization through weighted clauses.
- By combining `must`, `mustNot`, `should`, and `filter` clauses, you can design search experiences that prioritize the most relevant results.
- Scoring can be fine-tuned to suit the application’s context, improving user experience and engagement.

---

### **Next Steps**

Try implementing a `$compound` operator in your own database! Experiment with different clauses and scoring strategies to see how it transforms your search capabilities. With MongoDB’s powerful tools, you can create truly dynamic and intuitive search functionalities tailored to your application's users.

___

## A summary for `$search` stage from ChatGPT

The `$search` stage in MongoDB’s **Atlas Search** is a powerful feature used to implement advanced search capabilities. It allows you to search text, numeric, and other types of data within documents, with functionality such as fuzzy matching, ranking, and custom scoring.

Here's a deep dive into the `$search` stage, including key components, operators, and examples.

---

### **Key Components of `$search`**

1. **`index`**
    
    - Specifies the name of the search index to use. If not specified, MongoDB uses the default index.
    - Example:
        
        ```javascript
        { $search: { index: "customIndexName", ... } }
        ```
        
2. **`text` Operator**
    
    - Performs text-based search.
    - Allows matching with options like fuzzy search or phrase matching.
    - Example:
        
        ```javascript
        { 
          $search: { 
            text: { 
              query: "pizza", 
              path: "name", 
              fuzzy: { maxEdits: 2 } 
            } 
          } 
        }
        ```
        
3. **`compound` Operator**
    
    - Combines multiple conditions (`must`, `mustNot`, `filter`, etc.).
    - Useful for complex queries.
    - Example:
        
        ```javascript
        { 
          $search: { 
            compound: { 
              must: [
                { text: { query: "pizza", path: "name" } },
                { range: { path: "price", gte: 10, lte: 50 } }
              ],
              filter: [
                { range: { path: "rating", gte: 4 } }
              ]
            } 
          } 
        }
        ```
        
4. **`range` Operator**
    
    - Finds numeric or date values within a specified range.
    - Example:
        
        ```javascript
        {
          $search: {
            range: {
              path: "price",
              gte: 10,
              lte: 30
            }
          }
        }
        ```
        
5. **`autocomplete` Operator**
    
    - Used for implementing type-ahead functionality.
    - Example:
        
        ```javascript
        {
          $search: {
            autocomplete: {
              query: "piz",
              path: "name",
              fuzzy: { maxEdits: 1 }
            }
          }
        }
        ```
        
6. **`highlight`**
    
    - Highlights matched text in the search results.
    - Example:
        
        ```javascript
        {
          $search: {
            text: {
              query: "pizza",
              path: "name"
            },
            highlight: { path: "name" }
          }
        }
        ```
        

---

### **Detailed Examples**

#### **1. Simple Text Search**

Search for documents where the `name` field contains "pizza".

```javascript
db.collection.aggregate([
  {
    $search: {
      text: {
        query: "pizza",
        path: "name"
      }
    }
  }
]);
```

- **Result**: Returns all documents with "pizza" in the `name` field.

---

#### **2. Fuzzy Text Search**

Search for names similar to "pizza" with a maximum of 2 character edits.

```javascript
db.collection.aggregate([
  {
    $search: {
      text: {
        query: "piza",
        path: "name",
        fuzzy: {
          maxEdits: 2
        }
      }
    }
  }
]);
```

- **Use Case**: Handles user typos in search queries.

---

#### **3. Using Multiple Fields**

Search for a query in multiple fields, such as `name` and `categories`.

```javascript
db.collection.aggregate([
  {
    $search: {
      text: {
        query: "pizza",
        path: ["name", "categories"]
      }
    }
  }
]);
```

- **Result**: Returns documents where "pizza" is found in either the `name` or `categories` fields.

---

#### **4. Combining Queries with `compound`**

Search for documents that meet multiple criteria.

```javascript
db.collection.aggregate([
  {
    $search: {
      compound: {
        must: [
          { text: { query: "pizza", path: "name" } },
          { range: { path: "price", gte: 10, lte: 30 } }
        ],
        filter: [
          { range: { path: "rating", gte: 4 } }
        ]
      }
    }
  }
]);
```

- **Explanation**:
    - `must`: The name must contain "pizza" and the price must be between 10 and 30.
    - `filter`: Only includes documents with a rating of 4 or higher but does not affect the ranking.

---

#### **5. Autocomplete Search**

Implement a type-ahead search for restaurant names.

```javascript
db.collection.aggregate([
  {
    $search: {
      autocomplete: {
        query: "piz",
        path: "name",
        fuzzy: { maxEdits: 1 }
      }
    }
  }
]);
```

- **Result**: Matches names that start with "piz" and handles typos.

---

#### **6. Sorting Results**

Sort results by relevance and add additional sorting criteria.

```javascript
db.collection.aggregate([
  {
    $search: {
      text: {
        query: "pizza",
        path: "name"
      }
    }
  },
  { $sort: { rating: -1 } }
]);
```

- **Explanation**: Results are sorted by relevance first, then by the `rating` field in descending order.

---

#### **7. Highlighting Matched Text**

Highlight matches in the `name` field.

```javascript
db.collection.aggregate([
  {
    $search: {
      text: {
        query: "pizza",
        path: "name"
      },
      highlight: {
        path: "name"
      }
    }
  }
]);
```

- **Result**: Adds a `highlight` field in the output, showing matched text segments.

---

### **When to Use `$search`**

- **Full-text search**: When you need to search textual data with relevance scoring.
- **Typo handling**: To account for user input errors.
- **Sorting by relevance**: For ranked search results.
- **Autocomplete**: For type-ahead search functionality.
- **Custom filters**: For applying strict conditions while still using advanced search features.

---

### **Comparison with Standard MongoDB Queries**

|Feature|`$search`|Standard MongoDB Queries|
|---|---|---|
|**Text Matching**|Supports advanced matching|Limited to exact matches|
|**Relevance Scoring**|Yes|No|
|**Fuzzy Matching**|Yes|No|
|**Index Requirements**|Search indexes|B-tree indexes|
|**Autocomplete**|Yes|No|

___

### Lesson 5: Group Search Results by Using Facets
### **Using Facets and `$searchMeta` for Categorized Search Results**

In this lesson, we learn how to use **facets** and the **`$searchMeta` stage** in MongoDB Atlas to categorize search results, enhancing usability and search experience for end users.

---

### **What are Facets?**

Facets are **buckets** that group search results into categories, helping users navigate and find relevant data more efficiently. For example, in a social media app, search results can be categorized into **people**, **posts**, **events**, etc.

#### **Example Use Case**:

For a bird sighting application, we might categorize search results by the **week of sighting** to help users identify patterns in bird sightings.

---

### **Steps to Implement Faceted Search**

#### **1. Prepare the Search Index**

- **Field Mapping**:
    - Identify the fields to use for categorization (e.g., date, numbers, or strings).
    - Turn **off Dynamic Mapping** to define field mappings explicitly.
    - Add the fields to the search index with appropriate data types:
        - Example: Map the `date` field to the `DateFacet` data type for week-based categorization.
        - Additional searchable fields like `species common` and `species scientific` can be mapped as `string`.

---

#### **2. Use the `$searchMeta` Aggregation Stage**

The `$searchMeta` stage provides metadata about search results, including:

- **Facets**: Buckets with categorized results.
	- The buckets that the search will be filtered into are not part of the search result themselves, they're part of the search metadata-- information about how the search was carried out, As of now, there are two pieces of metadata for atlas search, the facets and the count, which is the number of results returned in each of the facets.
- **Count**: The number of results in each bucket.

---

#### **3. Configure the `$searchMeta` Query**

Below is an example query to group bird sightings by **weeks**:

```json
{
  "$searchMeta": {
    "index": "default",
    "facet": {
      "operator": {
        "text": {
          "query": ["Northern Cardinal"],
          "path": "species_common"
        }
      },
      "facets": {
        "sightingWeekFacet": {
          "type": "date",
          "path": "date",
          "boundaries": [
            "2024-01-01T00:00:00Z",
            "2024-01-08T00:00:00Z",
            "2024-01-15T00:00:00Z"
          ],
          "default": "other"
        }
      }
    }
  }
}
```

#### **Explanation**:

- **Facets Definition**:
    
    - `sightingWeekFacet`: The facet name, grouping results by `date` field.
    - `type`: `date` (to create weekly groupings).
    - `boundaries`: Array of dates representing the starting day of each week.
    - `default`: A bucket named `other` for results outside the specified weeks.
- **Query**:
    
    - Searches for the term `"Northern Cardinal"` in the `species_common` field.

---

#### **4. Results**

The query generates metadata with categorized results:

- Four buckets are created for sightings grouped by weeks.
- The **count** of sightings for each week is provided.
- The categorized results can now be displayed to end users in a user-friendly format.

---

### **Advantages of Faceted Search**

1. **Improved User Experience**:
    - Users can quickly narrow down results based on categories.
2. **Efficient Navigation**:
    - Categorization helps users find specific types of information faster.
3. **Customizable**:
    - You can define facets based on your application’s context (e.g., dates, price ranges, or tags).

---

### **Key Takeaways**

- Faceted search categorizes results into buckets, improving search usability.
- The `$searchMeta` stage provides metadata for grouping results without affecting the search results themselves.
- By defining **facets** in the `$searchMeta` stage, you can create highly customized and user-friendly search experiences.

---

### **Next Steps**

- Experiment with different facets in your application, such as price ranges, locations, or tags.
- Use MongoDB Atlas documentation to explore driver-specific implementations for integrating `$searchMeta` and facets into your code.

___

### **The Lab**

We have a dataset that contains records from an office supply company. The records contain information about orders, products, and customers. Imagine we are working on an application with a dashboard for administrators keeping track of sales at our office supply company. We want to add search functionality so that administrators can easily find sales matching their criteria. The administrators like to look at the sales by store and would like to organize search results by the store's location. We can accomplish that using facets!

## Lab Instructions

1. Open the `search_index.json` file in the [editor](https://play.instruqt.com/embed/mongodb/tracks/grouping-search-results-by-using-facets/challenges/creating-a-search-index-with-dynamic-mapping/assignment#tab-0) tab by clicking the file name in the file explorer to the left.
    
2. Edit the JSON file in the IDE as follows: changing the value for the second field 'type' for 'purchaseMethod' in 'fields' to `string` and change the second field 'type' for 'storeLocation' to `stringFacet` to reflect the sample below.

```json
{
    "name": "sample_supplies-sales-facets",
    "searchAnalyzer": "lucene.standard",
    "analyzer": "lucene.standard",
    "collectionName": "sales",
    "database": "sample_supplies",
    "mappings": {
        "dynamic": true,
        "fields": {
        "purchaseMethod": [
            {
            "dynamic": true,
            "type": "document"
            },
            {
            "type": "string"
            }
        ],
        "storeLocation": [
            {
            "dynamic": true,
            "type": "document"
            },
            {
            "type": "stringFacet"
            }
        ]
        }
    }
}
```


1. After you edit the `search_index.json` file, it will be autosaved. You can also use the keyboard commands such as `⌘+S` or `Ctrl+S` to save explicitly. Then, navigate to the [terminal](https://play.instruqt.com/embed/mongodb/tracks/grouping-search-results-by-using-facets/challenges/creating-a-search-index-with-dynamic-mapping/assignment#tab-1) tab at the top of the screen and run the following command: `atlas clusters search indexes create`:
    
    ```shell
    atlas clusters search indexes create --clusterName myAtlasClusterEDU -f /app/search_index.json
    ```
    
2. To verify that the index creation has started, please run the following command in the terminal `atlas clusters search indexes list --clusterName myAtlasClusterEDU --db sample_supplies --collection sales`:
    
    ```shell
    atlas clusters search indexes list --clusterName myAtlasClusterEDU --db sample_supplies --collection sales
    ```
    
    If the index has been created you should see output similar to the following in your terminal, the ID field value will differ:
    
    ```js
    ID                         NAME                            DATABASE          COLLECTION
    63930e5f9533f04f9cea282f   sample_supplies-sales-facets   sample_supplies   sales
    ```


```js
db.sales.aggregate([ { $searchMeta: { index: 'sample_supplies-sales-facets', "facet": { "operator": { "text": { "query": "In store", "path": "purchaseMethod" } }, "facets": { "locationFacet": { "type": "string", "path": "storeLocation" } } } } }] )

db.sales.aggregate([{$searchMeta: {index: 'sample_supplies-sales-facets'}, 'facet': {'operator': {'text': {'query': 'In Store', 'path': 'purchaseMethod'}}, 'facets': {'locationFacet': {'type': 'string', 'path': 'storeLocation'}}}}])
```

___

# MongoDB Atlas Search

Congratulations on learning about Atlas Search, an incredibly powerful tool for adding search functionality to your apps. In this unit, we covered the basics of Atlas Search, including the following:
- What a search index is.
- How to use a dynamically mapped search index to find results from any field in your data.
- How to use a statically mapped search index to find results from the most relevant parts of your data.
- How to use the aggregation pipeline to complete a `$search` operation.
- How to use a compound operator to search based on multiple operators.
- How to use a compound operator to give more weight to certain fields and give the user more relevant results.
- How to use `$searchMeta` and `$facet` to categorize search results to help your app's users find what they need more quickly

___

## Unit 11 : MongoDB Data Modeling Intro
### Lesson 1: Introduction to Data Modeling

### **Understanding Data Modeling in MongoDB**

This lesson introduces the concept of **data modeling** and explores how it is implemented in MongoDB compared to traditional relational databases. It highlights the advantages of MongoDB's flexible document model and how to approach data modeling effectively.

---

### **What is Data Modeling?**

Data modeling is the process of defining:

- **How data is stored**.
- **The relationships** between various entities in the data.

The structure or organization of this data within a database is called a **schema**.

---

### **Key Questions for Data Modeling**

When designing a schema in MongoDB, focus on the application rather than the database. Consider:
1. **What does my application do?**
2. **What data will I store?**
3. **How will users access this data?**
4. **What data will be most valuable?**

Answering these questions helps identify:
- User and application tasks.
- Data relationships.
- Access patterns and tools required.

---

### **Why is Proper Data Modeling Important?**

A good data model:
- **Simplifies data management**.
- **Optimizes query efficiency**.
- **Reduces memory and CPU usage**.
- **Minimizes costs**.

In MongoDB, the principle is simple:
> **Data accessed together should be stored together.**

---

### **MongoDB Document Data Model**

MongoDB provides a **flexible document data model**, characterized by:

#### 1. **Schema Flexibility**:

- Collections do not enforce a strict structure.
- Documents in the same collection can have different structures (**polymorphism**).

#### 2. **Schema Validation**:

- MongoDB allows schema definition and validation for structured storage.

#### 3. **Nested or Embedded Documents**:

- Documents can include other documents, enabling complex relationships.

#### 4. **Normalization and Embedding**:

- Normalize data using references when necessary.
- Embed data when it will be accessed together frequently.

---

### **MongoDB vs. Relational Databases**

|Aspect|Relational Model|Document Model|
|---|---|---|
|**Approach**|Focus on database and data requirements.|Focus on application and access patterns.|
|**Schema Structure**|Rigid, predefined schema.|Flexible, schema-optional.|
|**Storage Techniques**|Normalize all data with references.|Embed, normalize, or combine approaches.|
|**Developer Involvement**|Data passed to developers for use.|Schema designed based on application needs.|

---

### **Steps in Data Modeling with MongoDB**

1. **Understand Application Requirements**:
    - Define access patterns and relationships.
2. **Optimize for Queries**:
    - Structure data to reduce resource usage.
3. **Decide on Relationships**:
    - Embed or normalize based on application needs.

---

### **Takeaway**

The goal of data modeling is to:

- Design a schema that supports efficient storage and querying.
- Ensure optimal use of database resources.
- Enhance application performance by aligning the schema with user and application needs.

With these principles, you can build a robust, scalable data model tailored to your application's requirements.

___
### Lesson 2: Types of Data Relationships

### **Understanding Data Relationships in MongoDB**

This lesson introduces the common types of data relationships and the two primary methods for modeling them in MongoDB. The focus is on creating efficient schemas that align with how the application queries and updates data.

---

### **Key Principle**

> **Data that is accessed together should be stored together.**

By adhering to this principle, you can reduce query complexity, optimize performance, and minimize resource usage.

---

### **Types of Data Relationships**

1. **One-to-One**:
    
    - A single entity in one dataset is associated with a single entity in another.
    - Example: A movie linked to its director.
    - **Modeling in MongoDB**:
        
        - Embed the director's information directly in the movie document.
        
        ```json
        {
	      "_id": ObjectId ("573a139@f29313caabcd413b") ,
          "title": "Star Wars: Episode IV - A New Hope",
          "director": "George Lucas"
        }
        ```
        
2. **One-to-Many**:
    
    - A single entity in one dataset is linked to multiple entities in another.
    - Example: A movie with multiple cast members.
    - **Modeling in MongoDB**:
        
        - Use an array to embed the cast members.
        
        ```json
        {
          "movie": "Star Wars",
          "cast": [
	          {"actor": "Mark Hamill", "character": "Luke Skywalker"},
	          {"actor": "Harrison Ford", "character": "Han Solo"},
	          {"actor": "Carrie Fisher", "character": "Princess Leia Organa"},
        }
        ```
        
3. **Many-to-Many**:
    
    - Multiple entities in one dataset are linked to multiple entities in another.
    - Example: Movies filmed at various locations and locations used by multiple movies.
    - **Modeling in MongoDB**:
        - Use a combination of embedding and referencing, depending on access patterns.

---

### **Methods of Modeling Relationships**

1. **Embedding**:
    
    - **Definition**: Related data is stored directly within the same document.
    - **Benefits**:
        - All related data is retrieved with a single query.
        - Simplifies the schema for closely associated data.
    - **Example**:
        
        ```json
        {
          "movie": "Star Wars",
          "cast": ["Mark Hamill", "Carrie Fisher"],
          "details": {
            "director": "George Lucas",
            "release_year": 1977
          }
        }
        ```
        
2. **Referencing**:
    
    - **Definition**: A document includes references (e.g., object IDs) to related documents in another collection.
    - **Benefits**:
        - Useful for data that is not frequently accessed together or shared across entities.
    - **Example**:
        
        ```json
        {
          "movie": "Star Wars",
          "filming_locations": [
            ObjectId("64a7f03e2e0001"),
            ObjectId("64a7f03e2e0002")
          ]
        }
        ```

![Pasted image 20241125164017.png](assets/Pasted%20image%2020241125164017.png)

---

### **When to Use Each Method**

- **Embedding**:
    
    - When related data is accessed together frequently.
    - When the data has a one-to-one or one-to-many relationship.
    - When the size of the embedded data is predictable and manageable.
- **Referencing**:
    
    - When related data is shared between entities.
    - When related data is infrequently accessed together.
    - When the related data size is large or dynamic.

---

### **Takeaways**

1. **Data Access Patterns**:
    - Structure data according to how the application queries and updates it.
2. **Efficient Relationships**:
    - MongoDB supports one-to-one, one-to-many, and many-to-many relationships.
3. **Modeling Methods**:
    - Use **embedding** for frequently accessed related data.
    - Use **referencing** for shared or less frequently accessed related data.

By understanding these principles, you can create a schema that is efficient, scalable, and tailored to your application's needs.

___

### Lesson 3: Modeling Data Relationships

### **Detailed Example of Data Modeling**

This lesson provides a practical example of how to model data efficiently in MongoDB by using a student record as a case study. It focuses on handling one-to-many relationships and demonstrates the flexibility of MongoDB’s document model.

---

### **Key Recap**

1. **Relationships**:
    - One-to-One
    - One-to-Many
    - Many-to-Many
2. **Modeling Methods**:
    - Embedding
    - Referencing

---

### **Case Study: Student Record**

When a student starts school, their profile is created in the database. This profile includes:
- Personal details
- Contact phone numbers
- Information about classes and grades

---

### **Handling One-to-Many Relationships**

#### **Contact Phone Numbers**

- **Original Representation**: Each phone number is stored as a separate item.
	![Pasted image 20241125165811.png](assets/Pasted%20image%2020241125165811.png)
    - **Issue**: This creates redundancy and lacks organization.
- **Improved Representation**: Use an array to represent a one-to-many relationship.
    
    ```json
    {
      "name": "John Doe",
      "phone_numbers": [
        "123-456-7890",
        "987-654-3210",
        "555-123-4567"
      ]
    }
    ```
    
- **Enhanced Representation with Context**: To maintain context (e.g., home, cell, or emergency contact), store additional metadata alongside each number.
    
    ```json
    {
      "name": "John Doe",
      "phone_numbers": [
        { "number": "123-456-7890", "type": "home" },
        { "number": "987-654-3210", "type": "cell" },
        { "number": "555-123-4567", "type": "emergency" }
      ]
    }
    ```
    

---

### **Handling Large and Related Data**

#### **Class Information**

- If a student takes many classes, embedding all class details in the student document could make it bloated and inefficient to manage.
- Instead, reference another collection that contains class details.

**Example: Student Document**

```json
{
  "name": "John Doe",
  "phone_numbers": [
    { "type": "home", "number": "123-456-7890" },
    { "type": "cell", "number": "987-654-3210" }
  ],
  "classes": [
    { "course_id": "CS101", "course_name": "Intro to Computer Science" },
    { "course_id": "MATH203", "course_name": "Linear Algebra" }
  ]
}
```

**Example: Courses Collection**

```json
[
  { "course_id": "CS101", "course_name": "Intro to Computer Science", "instructor": "Dr. Smith" },
  { "course_id": "MATH203", "course_name": "Linear Algebra", "instructor": "Prof. Lee" }
]
```

By referencing the `course_id` and `course_name`, we avoid duplicating course details and maintain a clean structure.

---

### **Key Takeaways**

1. **Relationships in MongoDB**:
    
    - Can be represented using embedding, referencing, or a combination of both.
    - Example: Contact numbers (embedding), course details (referencing).
2. **Embedding**:
    
    - Use for tightly related data or when all information is accessed together.
    - Example: Contact numbers with metadata.
3. **Referencing**:
    
    - Use for shared or frequently updated data.
    - Example: Linking students to courses.
4. **Flexibility**:
    
    - MongoDB's document model allows for efficient and context-driven schemas.

By understanding the principles demonstrated in this case study, you can create a flexible, scalable, and application-friendly data model.

___

### Lesson 4: Embedding Data in Documents

### **Modeling Data Using Embedding in MongoDB**

This lesson explains the concept of **embedding** in MongoDB, highlighting its benefits, use cases, and potential pitfalls.

---

### **What is Embedding?**

Embedding involves nesting one document inside another. It is particularly useful for **one-to-many** or **many-to-many** relationships, as it simplifies queries and enhances performance.

#### **Key Features:**

- Simplifies data access by storing related data together.
- Improves query performance by avoiding joins.
- Allows for single-operation updates of related data.

---

### **Examples of Embedding**

#### **Basic Example:**

**Single-level embedding** for simple relationships like name:

```json
{
  "name": { 
    "firstName": "Sarah", 
    "lastName": "Davis" 
  }
}
```

#### **Complex Example:**

**Multi-level embedding** for a one-to-many relationship, such as a customer with multiple addresses:

![Pasted image 20241125170857.png](assets/Pasted%20image%2020241125170857.png)
In this case:

- The `Address` field is an array of embedded documents.

---

### **Advantages of Embedding**

1. **Improved Performance**:
    - Embedding adheres to MongoDB’s principle: _“Data that is accessed together should be stored together.”_
    - Reduces the number of queries by consolidating data into one document.
    - Avoids costly application-level joins.
    
1. **Simplified Updates**:
    - Related data can be modified in a single operation.
    
1. **Efficient Reads**:
    - Retrieves all necessary data with one query, speeding up operations.

---

### **Potential Pitfalls of Embedding**

1. **Large Documents**:
    - Embedding can make documents grow excessively large, leading to:
        - Increased memory usage.
	        - Large documents have to be read into memory in full, which can result in a slow application performance for your end user.
        - Slower read operations.
    - Example: Continuously adding history or logs to an embedded array.
    
1. **Unbounded Documents**:
    - A document that grows indefinitely can exceed MongoDB’s maximum document size of **16 MB**, causing performance degradation.
    
1. **Schema Anti-Patterns**:
    - Overuse of embedding can result in poor schema design.
    - Example: Embedding rarely accessed or unrelated data in the same document.

---

### **Best Practices for Embedding**

- Use **embedding** for related data that:
    - Is frequently accessed together.
    - Has a manageable and predictable size.
    
- Avoid **embedding** when:
    - The data is unbounded or grows indefinitely.
    - The embedded data is rarely queried alongside the parent document.

---

### **Key Takeaways**

1. **Purpose**:
    - Embedding is a powerful method to model relationships in MongoDB.
    - It is suitable for **one-to-many** and **many-to-many** relationships.

2. **Benefits**:    
    - Simplifies queries and reduces their frequency.
    - Improves application performance for reads and writes.

3. **Challenges**:    
    - Watch out for large or unbounded documents.
    - Avoid schema anti-patterns to maintain optimal performance.

4. **Recommendation**:
    - MongoDB encourages embedding for related data, as long as it fits your application's access patterns and size constraints.

By following these guidelines, you can effectively use embedding to create efficient and scalable MongoDB schemas.

___

### Lesson 5: Referencing Data in Documents

### **Modeling Data Using References in MongoDB**

This lesson introduces **references** (also called linking or data normalization) as a method for modeling relationships in MongoDB. It provides insights into when and why to use referencing instead of embedding.

---

### **What are References?**

References establish a link between documents stored in different collections. They store the `_id` of one document as a field in another document, ensuring the relationship is maintained without duplicating data.
Using references sometimes is called linking or data normalization.

#### **Key Principle:**

_"Data that is accessed together should be stored together."_

While embedding aligns with this principle, referencing is useful when:

1. The data is large and infrequently accessed together.
2. You want to avoid duplication and redundancy.
3. The relationship is more complex or spans multiple collections.

---

### **Example of Referencing**

#### **Scenario:**

A student record that references the courses they are enrolled in.

![Pasted image 20241125172746.png](assets/Pasted%20image%2020241125172746.png)

**Student Document (in `students` collection):**

```json
{
  "_id": "student123",
  "name": "Sarah Davis",
  "enrolledCourses": ["course456", "course789"]
}
```

**Course Documents (in `courses` collection):**

```json
{
  "_id": "course456",
  "name": "Introduction to MongoDB",
  "instructor": "John Doe"
}
```

```json
{
  "_id": "course789",
  "name": "Advanced JavaScript",
  "instructor": "Jane Smith"
}
```

Here:

- The `enrolledCourses` field in the student document holds an array of references (`_id` values) to course documents in the `courses` collection.
- Each referenced document stores detailed course information.

---

### **Advantages of Using References**

1. **Avoids Data Duplication:**
    - Ensures that shared data, like course details, is stored in a single location.
    - Reduces storage requirements.
2. **Smaller Documents:**
    - Keeps documents lightweight by storing only references instead of full datasets.
3. **Consistency:**
    - Updates to referenced documents automatically reflect for all related documents.

---

### **Disadvantages of Using References**

1. **Multiple Queries:**
    - Retrieving related data requires querying multiple collections, increasing resource usage and latency.
2. **Increased Complexity:**
    - Application logic must handle fetching and combining data from multiple sources.
3. **Performance Cost:**
    - Joining data at the application level (instead of within a single document) may slow down reads.

---

![Pasted image 20241125172552.png](assets/Pasted%20image%2020241125172552.png)

___
### **When to Use References**

- **Use References When:**
    
    1. The data is large and rarely accessed together.
    2. Relationships are complex or deeply nested.
    3. You want to enforce consistency and avoid data duplication.
- **Use Embedding When:**
    
    1. The data is small and frequently accessed together.
    2. You want to reduce query complexity and optimize read performance.

---

### **Key Takeaways**

1. **Purpose:**
    - References link documents across collections, maintaining relationships without duplicating data.
2. **Benefits:**
    - Saves storage space and enforces consistency.
    - Suitable for large, independent datasets or complex relationships.
3. **Challenges:**
    - Requires additional queries to retrieve linked data.
    - Can impact read performance due to the need for application-level joins.
4. **Guideline for Choosing:**
    - Use **embedding** for frequent, tightly coupled data.
    - Use **referencing** for larger, less-frequently accessed, or shared data.

By understanding when to use references, you can design efficient and scalable schemas in MongoDB.

___

### Lesson 6: Scaling a Data Model

### **Avoiding Scalability Issues in MongoDB Data Models**

This lesson highlights common problems with non-scalable data models and strategies to design efficient and scalable schemas in MongoDB. The key focus is on **unbounded documents** and how to avoid them.

---

### **Key Principle:**

_"Data that is accessed together should be stored together."_

Your data model must align with your query patterns to achieve:

- Optimal **query performance**.
- Efficient **memory usage**.
- Reduced **CPU overhead**.
- Scalable **storage management**.

---

### **Understanding Unbounded Documents**

Unbounded documents are those that grow infinitely as new data is added. These are problematic because they:

1. Exceed the **16 MB document size limit** in MongoDB.
2. Consume excessive memory during read operations.
3. Slow down **write performance** as the entire document must be rewritten with every update.
4. Make tasks like filtering or pagination inefficient.

---

### **Example: Blog Post with Embedded Comments**

#### **Initial Model (Embedding Comments):**

![Pasted image 20241125190324.png](assets/Pasted%20image%2020241125190324.png)

```json
{
  "_id": "post123",
  "title": "Understanding MongoDB",
  "content": "MongoDB is a NoSQL database...",
  "comments": [
    { "author": "User1", "text": "Great post!" },
    { "author": "User2", "text": "Thanks for sharing." }
  ]
}
```

This model:

- Embeds all comments within the blog post document.
- Works well for a small number of comments.
- Fails when there are thousands of comments, as:
    - The document grows too large.
    - Query performance declines.
    - Filtering and pagination require retrieving all comments first.

---

### **Optimized Model (Using References):**

To scale efficiently, separate comments into their own collection and use references.

![Pasted image 20241125190334.png](assets/Pasted%20image%2020241125190334.png)
#### **Blog Post Collection:**

```json
{
  "_id": "post123",
  "title": "Understanding MongoDB",
  "content": "MongoDB is a NoSQL database..."
}
```

#### **Comments Collection:**

```json
{
  "_id": "comment456",
  "postId": "post123",
  "author": "User1",
  "text": "Great post!"
}
```

```json
{
  "_id": "comment789",
  "postId": "post123",
  "author": "User2",
  "text": "Thanks for sharing."
}
```

**Advantages:**

1. Each document remains small and efficient.
2. Comments can be retrieved or paginated independently.
3. Avoids hitting the **16 MB size limit**.
4. Allows efficient queries like filtering comments for specific blog posts.

---

### **Key Considerations for Scalable Data Models**

1. **Embed vs. Reference:**
    - Embed data for **small, frequently accessed** relationships.
    - Use references for **large, growing** or **complex relationships**.
2. **Optimize for Query Patterns:**
    - Store data together when it's accessed together.
    - Avoid over-embedding, which leads to unbounded documents.
3. **Plan for Scalability:**
    - Design your schema to handle growth in data size and complexity.
    - Use appropriate indexes to improve query performance.
4. **Avoid Schema Anti-Patterns:**
    - Large and unbounded documents.
    - Inefficient joins or queries at the application level.

---

### **Key Takeaways**

1. Avoid unbounded documents, as they lead to performance issues and can hit the document size limit.
2. Use **embedding** for tightly coupled, small data sets.
3. Use **referencing** for large, frequently growing data sets.
4. Design your schema to align with your application's query patterns to ensure scalability and efficiency.

With these strategies, you can build scalable, efficient, and robust data models in MongoDB.

___

### Lesson 7: Using Atlas Tools for Schema Help

### **Schema Anti-Patterns and Tools in MongoDB Atlas**

This lesson explores **schema anti-patterns**—common pitfalls in data modeling—and demonstrates how to identify and resolve them using **MongoDB Atlas tools**, specifically the **Data Explorer** and **Performance Advisor**.

---

### **What Are Schema Anti-Patterns?**

Schema anti-patterns occur when data modeling doesn't follow best practices, leading to:

- Suboptimal query performance.
- Non-scalable or inefficient database operations.

**Common Anti-Patterns:**

1. **Massive Arrays**: Unbounded arrays that grow infinitely.
2. **Excessive Collections**: Too many collections for small, unrelated data.
3. **Bloated Documents**: Large documents with unnecessary or redundant fields.
4. **Unnecessary Indexes**: Indexes that aren't used by queries, consuming storage and memory.
5. **Queries Without Indexes**: Resulting in slow scans of large data sets.
6. **Scattered Data**: Data accessed together but stored in separate collections.

---

### **Tools for Identifying Anti-Patterns**

MongoDB Atlas provides two main tools to help identify and fix schema anti-patterns: **Data Explorer** and **Performance Advisor**.

#### **1. Data Explorer (Available in Free Tier):**

- Accessible via **Browse Collections** in the Atlas interface.
- Features:
    - View **storage size**, **document count**, and **index size** for collections.
    - **Indexes Tab**: Displays indexes and their stats to identify unused or redundant ones.
	    ![Pasted image 20241125192235.png](assets/Pasted%20image%2020241125192235.png)
    - **Schema Anti-Pattern Tab**:
        - Highlights potential schema design issues.
        - Provides detailed recommendations for resolving them.
    - Example Action: Dropping unused indexes by sorting them based on usage.
	    ![Pasted image 20241125192316.png](assets/Pasted%20image%2020241125192316.png)

#### **2. Performance Advisor (Available in M10 Tier and Above):**

- Analyzes **active collections** and queries to suggest performance improvements.
- Features:
    - Recommends **new indexes** for slow or frequent queries.
    - Identifies **redundant indexes** that can be dropped.
    - Highlights **schema issues** impacting performance.
	    ![Pasted image 20241125192400.png](assets/Pasted%20image%2020241125192400.png)

---

### **Steps to Fix Anti-Patterns:**

1. **Using Data Explorer:**
    
    - Review the **Schema Anti-Pattern Tab** for flagged issues.
    - Use the **Indexes Tab** to analyze unused indexes.
    - Drop unnecessary indexes to optimize storage and memory.
2. **Using Performance Advisor:**
    
    - Analyze the most active collections and slow queries.
    - Implement suggested **index recommendations**.
    - Resolve flagged schema issues to improve query performance.

---

### **Key Takeaways**

1. **Schema Anti-Patterns**:
    - Avoid massive arrays, excessive collections, bloated documents, and unused indexes.
    - Ensure data frequently accessed together is stored together.
2. **Data Explorer**:
    - Helps identify schema issues and manage collections and indexes.
3. **Performance Advisor**:
    - Provides recommendations for schema and index optimization.

By leveraging these tools, you can ensure scalable and efficient MongoDB schemas, enhancing database performance and resource usage.

___

## Introduction to MongoDB Data Modeling  

In this unit, you learned how to:

- Explain the purpose of data modeling.
- Identify the types of data relationships (one to one, one to many, many to many).
- Model data relationships.
- Identify the differences between embedded and referenced data models.
- Scale a data model.
- Use Atlas Tools for schema help.

___

### MongoDB Schema Validation (ChatGPT)

Schema validation in MongoDB allows you to enforce rules on the structure, content, and data types of documents in a collection. While MongoDB is schema-less by default, schema validation offers flexibility to enforce specific structures when needed.

---

### **Key Concepts**

- **Validation Level**: Defines how strictly validation rules are applied to documents.
    - **strict**: Documents must fully comply with validation rules.
    - **moderate**: Existing documents bypass validation unless modified.
- **Validation Action**: Determines what happens when a document violates the rules.
    - **error**: Rejects documents that fail validation.
    - **warn**: Logs warnings for invalid documents but still allows insertion.

---

### **Defining Schema Validation**

Schema validation is set using the `$jsonSchema` keyword when creating or updating a collection:

```javascript
db.createCollection("products", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "price"],
      properties: {
        name: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        price: {
          bsonType: "number",
          minimum: 0,
          description: "must be a non-negative number and is required"
        },
        category: {
          bsonType: "string",
          description: "must be a string"
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
});
```

---

### **Key Validation Features**

1. **Field Type Validation**: Ensures fields are specific BSON types, like `string`, `number`, `array`, or `date`.
    
2. **Required Fields**: Specifies mandatory fields in a document.
    
3. **Field Constraints**:
    
    - Minimum and maximum values for numbers.
    - Length constraints for strings.
    - Matching patterns using regular expressions.
4. **Array Validation**: Validates elements in an array using `items`.
    
5. **Nested Validation**: Validates embedded documents or arrays of documents.
    

---

### **Examples**

#### 1. **Validating String Fields**

```javascript
"status": {
  bsonType: "string",
  enum: ["new", "in progress", "completed"],
  description: "must be one of the enum values"
}
```

```js
"name": {
  bsonType: 'string',
  maxLength: 100,
  description: 'Maximum length for name is 100 chars'
}
```
#### 2. **Validating Number Ranges**

```javascript
"age": {
  bsonType: "int",
  minimum: 18,
  maximum: 65,
  description: "must be an integer between 18 and 65"
}
```

#### 3. **Validating Arrays**

```javascript
"tags": {
  bsonType: "array",
  items: {
    bsonType: "string"
  },
  description: "must be an array of strings"
}
```

#### 4. **Validating Nested Documents**

```javascript
"address": {
  bsonType: "object",
  required: ["city", "zip"],
  properties: {
    city: {
      bsonType: "string",
      description: "must be a string"
    },
    zip: {
      bsonType: "string",
      pattern: "^[0-9]{5}$",
      description: "must be a 5-digit zip code"
    }
  }
}
```

#### 5. **Additional Properties**

- Control whether extra fields outside the defined schema are allowed. Example:
```js
{ 
  "additionalProperties": false 
}
```
        
#### 6. **Conditional Validation**:
- Specify rules based on the presence or value of other fields. Example:
```js
{   
  "if": { "properties": { "status": { "enum": ["active"] } } },   
  "then": {  "required": ["lastActive"] } 
}
```
---

### **Updating Schema Validation**

You can update the schema validation rules using the `collMod` command:

```javascript
db.runCommand({
  collMod: "products",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "price"],
      properties: {
        name: { bsonType: "string" },
        price: { bsonType: "number", minimum: 0 }
      }
    }
  },
  validationLevel: "moderate",
  validationAction: "warn"
});
```

---

### **Advantages of Schema Validation**

- Ensures data integrity and consistency.
- Reduces application-side validation overhead.
- Supports gradual adoption by adjusting `validationLevel`.

---

### **Caveats**

- Validation applies only to inserts and updates; existing documents are unaffected unless modified.
- Validation adds some overhead, especially for complex rules.
- MongoDB's schema validation is not as strict or comprehensive as relational database schemas.

Schema validation in MongoDB is a powerful tool for balancing the flexibility of a NoSQL database with the need for structured data integrity.

___
## Unit 12: MongoDB Transactions
### Lesson 1: Introduction to ACID Transactions
### **ACID Transactions: Ensuring Database Integrity**

This lesson introduces **ACID transactions**, explaining their properties, use cases, and why they are essential for maintaining data integrity and consistency during complex database operations.

---

### **What Are ACID Transactions?**

An **ACID transaction** is a group of database operations that:

- **Execute as a single unit** (all succeed or fail together).
- Maintain the integrity and consistency of data.

**Problem Example:**

- Two friends split a dinner bill, transferring money between accounts using a mobile payment app.
- Without ACID transactions, if one operation (deducting money) succeeds but the other (adding money) fails, this results in **data inconsistency** or **loss of value**.

---

### **ACID Properties**

ACID is an acronym for the properties that ensure database safety during transactions:

1. **Atomicity (A)**:
    
    - **All or Nothing**: Either all operations in the transaction succeed, or none of them are applied.
    - Example: Deducting money from one account and adding it to another must both succeed or fail.
2. **Consistency (C)**:
    
    - Ensures all changes conform to the database's constraints and rules.
    - Example: A transaction fails if an operation violates a rule, like an account balance going below zero.
3. **Isolation (I)**:
    
    - Ensures multiple transactions run simultaneously without interfering with each other.
    - Guarantees that each transaction that is run concurrently leaves the database in the same state as if the transactions were run sequentially. In other words, multiple transactions can happen at the same time without affecting the outcome of the other transactions.
    - Example: While one transaction transfers money, another querying the account sees consistent data.
4. **Durability (D)**:
    
    - Ensures changes are permanently saved, even in the event of a power failure or crash.
    - Guarantees that data is never lost. Data is saved to non-volatile memory, so any modifications made to data by a successful transaction will persist, even in the event of a power or hardware failure.
    - Example: Once a transfer is marked successful, the data remains intact despite system failures.

---

### **Use Cases for ACID Transactions**

ACID transactions are crucial in scenarios where **value transfer or integrity** is at stake, such as:

- **Banking and Finance**: Transferring money between accounts.
- **E-Commerce**: Moving an item from inventory to a shopping cart.
- **Payroll Systems**: Tracking payments made to employees.
- **Stock Trading**: Buying or selling shares in real-time.

---

### **Key Takeaways**

1. **Definition**: ACID transactions ensure that all database operations in a transaction execute as a single, reliable unit.
2. **Properties**:
    - **Atomicity**: All or nothing.
    - **Consistency**: Adheres to rules.
    - **Isolation**: Prevents interference.
    - **Durability**: Persists changes.
3. **When to Use**:
    - Value transfer between records.
	    - ACID transactions in MongoDB are typically used only by applications where values are exchanged between different parties, such as banking or business applications.
    - Situations requiring guaranteed data consistency and integrity.

By applying ACID principles, developers can build reliable applications that handle critical operations seamlessly, even under failure conditions.

___

### Lesson 2: ACID Transactions in MongoDB

### **ACID Transactions in MongoDB: Single vs. Multidocument Operations**

This lesson explains how **ACID transactions** work in MongoDB's document model, focusing on the distinction between **single-document** and **multidocument** transactions and their use cases.

---

### **Single-Document Transactions**

- In MongoDB, **operations on a single document** are **inherently atomic**.
- Example: Using the `updateOne` method to modify multiple fields in a single document ensures that either all changes are applied or none are.
- **Key Properties**:
    - Changes are visible simultaneously to all clients.
    - No additional steps are required to ensure ACID compliance.

![Pasted image 20241126121044.png](assets/Pasted%20image%2020241126121044.png)

---

### **Multidocument Transactions**

- Operations affecting **multiple documents** are **not inherently atomic**. To make them atomic, they must be wrapped in a **multidocument ACID transaction**.
- Example: In an e-commerce application:
    - Adding an item to a shopping cart (updates a document in the `shoppingCart` collection).
	    ![Pasted image 20241126121350.png](assets/Pasted%20image%2020241126121350.png)
    - Deducting the same item from the inventory (updates a document in the `inventory` collection).  
	    ![Pasted image 20241126121416.png](assets/Pasted%20image%2020241126121416.png)
        Both operations must succeed or fail together to ensure data consistency.

---

### **When to Use Multidocument Transactions**

- Use multidocument transactions only when **ACID properties** are absolutely necessary for:
    
    1. **Maintaining Consistency**: Ensuring the database remains consistent after complex, interrelated operations.
    2. **Critical Operations**: Scenarios like financial transactions, inventory management, or any interdependent updates across multiple collections.
- **Performance Consideration**:
    
    - Multidocument transactions **lock all involved documents** during the operation.
    - This can introduce significant **performance overhead**, increasing latency in your application.

---

### **Key Takeaways**

1. **Single-Document Operations**:
    - Inherently atomic.
    - No additional steps required for ACID compliance.
2. **Multidocument Operations**:
    - Not inherently atomic.
    - Require a multidocument ACID transaction for consistency.
3. **Use Multidocument Transactions Sparingly**:
    - They are a precise tool for special cases.
    - Avoid overuse due to performance trade-offs.

By understanding the differences between single and multidocument transactions, developers can design efficient and scalable data models that prioritize performance while maintaining consistency where it matters most.

___

### Lesson 3: Using Transactions in MongoDB

### **How to Use Multi-Document Transactions in MongoDB**

This video explains how to execute, commit, and abort **multi-document transactions** in MongoDB using the shell, which mirrors the process used in MongoDB drivers. The example focuses on transferring funds between two accounts.

---

### **Steps for Multi-Document Transactions**

1. **Start a Session**:
    
    - Begin by creating a session, which groups related database operations.
    - A session is a group of database operations that are related to each other and should be run together, similar to a transaction.
    - Use the `startSession` method:
	    - `db.getMongo()` allows us to access the Mongo session.
        
        ```javascript
        const session = db.getMongo().startSession();
        ```
        
2. **Start the Transaction**:
    
    - Begin a transaction within the session:
        
        ```javascript
        session.startTransaction();
        ```
        
3. **Access the Relevant Collection**:
    
    - Reference the collection involved in the transaction. This will make it easier to write the subsequent queries:
        
        ```javascript
        const accounts = session.getDatabase("bank").getCollection("accounts");
        ```
        
4. **Perform Database Operations**:
    
    - Execute updates as part of the transaction using `updateOne`.  
        Example: Deduct $30 from one account and add it to another.
        
        ```javascript
        accounts.updateOne(
          { account_id: "MDB740836066" },
          { $inc: { balance: -30 } }
        );
        ```
    
	- Verify that `modifiedCount` is `1` for each update to confirm successful operation.
		```js
		{
			acknowledged: true,
			insertedld: null,
			matchedCount: 1
			modifiedCount; 1,
			upsertedCount: a
		}
		```
		This shows us that this particular operation has been conducted on the database. However, in the events that this transaction fails, this operation can be reverted back to its original state.
		
		```js        
        accounts.updateOne(
          { account_id: "4500" },
          { $inc: { balance: 30 } }
        );
        ```
        If you get an error at this point, it's likely because you've taken too long with your transaction. In MongoDB, all transactions have to execute within 60 seconds of the first write.    
1. **Commit the Transaction**:
    
    - Finalize the transaction to apply all changes:
        
        ```javascript
        session.commitTransaction();
        ```
        
6. **Abort the Transaction (Optional)**:
    
    - If needed, cancel the transaction to revert all operations:
        
        ```javascript
        session.abortTransaction();
        ```
        
7. **Verify the Results**:
    
    - Use `find` queries to check the updated balances:
        
        ```javascript
        db.accounts.find({ account_id: "MDB740836066" });
        db.accounts.find({ account_id: "4500" });
        ```
        

---

### **Example Scenario**

**Initial Balances**:

- Naja Petersen (Account `6066`): $2891
- Florence Taylor (Account `4500`): $2598

**Transaction**:

- Deduct $30 from `6066` and add it to `4500`.

**Post-Transaction Balances**:

- Naja Petersen: $2861
- Florence Taylor: $2628

---

### **Key Considerations**

- **Timeout**: Transactions must complete within **60 seconds** of the first write. If timed out, restart the transaction.
	- MongoServerError: Transation 1 has been aborted
- **Abort Transactions**: Use `abortTransaction` to revert all changes if the transaction cannot or should not proceed.
- **Performance**: Transactions lock all involved documents, which can impact performance.

---

### **Key Takeaways**

- **Start a Session**: Use `startSession` to initiate a session for the transaction.
- **Transaction Control**: Use `startTransaction`, `commitTransaction`, and `abortTransaction` to manage transaction flow.
- **Atomicity**: Multi-document transactions ensure all operations succeed together or fail together, preserving database integrity.

___

# MongoDB Transactions

In this unit, you learned that ACID transactions ensure that database operations, such as transferring funds from one account to another, happen together or not at all. You also explored how ACID transactions work with the document model in MongoDB. Finally, you learned how to create and use multi-document transactions by using the `startTransaction()` and `commitTransaction()` commands, and how to cancel multi-document transactions by using the `abortTransaction()` command.

___
# References
https://www.mongodb.com/docs/manual/tutorial/equality-sort-range-rule/
https://medium.com/@akyas.naushad/mongo-indexing-best-practices-simplified-dfee73fa08b
