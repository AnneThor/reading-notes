# AWS API, Dynamo and Lambda

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Serverless: API Gateway Overview](#aws-api-gateway-overview)

[Amazon: API Gateway Docs](#aws-api-gateway)

[DynamoDB Guide](#dynamodb-guide)

[Amazon: DynamoDB Docs](#aws-dynamodb)

[Dynamoose](#dynamoose)

## Review, Research, and Discussion

### What’s the difference between a FIFO and a standard queue?

Standard queue does not guarantee order and messages may be duplicated. Processing capacity is higher.

SQS (simple queue service) this means that there are other mechanisms in place to guarantee that messages are not duplicated. Messages do not necessarily get deleted after multiple failed attempts, keeps messages secure. A big advantage: can convert synchronous patterns to async. One message **CAN NOT** have multiple consumers in SQS, it gets deleted after being consumed by one customer.

### How can the server be assured a message was properly received?

The client app can emit a message back to the server that the message was received.

### What classic design pattern is best represented by event driven programming?

The observer pattern, where an object maintains a list of dependents and notifies them about any changes in state (to which they can either respond or ignore depending on their functionality).

### How do you test an event driven system?

You test that the handlers are doing what you expect them to do, you can assume that the emit/on actions perform as expected. So basically unit tests, and then you can run end to end tests by deploying the whole system against a test environment and checking expected results.

## Vocabulary Terms

Term | Definition
---- | ----------
Serverless Functions | Refers to functions that do not require any active server, they are only spun up when they are called; it should be a pure function and can theoretically serve multiple purposes
Cloud Storage | storage that is hosted on a remote server that the user can access but does not have to manage or maintain the hardware of
CDN | Content Delivery Networks are distributed systems of servers that work together to provide fast transfer of HTTP requests by locally caching information to reduce the tranmission distance from user to server

## [AWS Api Gateway Overview](https://www.serverless.com/amazon-api-gateway)

Amazon API Gateway is a service that allows developers to provide the HTTP endpoints of a REST API or WebSocket API and connect them with appropriate functionality, can handle authentication/authorization/access control and can offer detailed metrics and tracing about API request

Has a GUI, but better to create API structure in code

This is the piece that ties together Serverless functions and API definitions; can trigger serverless function directly in response to HTTP request => truely serverless construct

### Benefits of Amazon API Gateway

1. Map HTTP requests to specific functions in a serverless application via an API Gateway event
2. Map WebSocket events to Serverless functions
3. Use multiple microservices to serve same top level API: different functions can service different parts of the API so you can encapsulate functionality
4. Can integrate with other AWS services (time savings on authentication, etc)

Use case is building serverless HTTP APIs, probably not the solution for an ultra low latency HTTP request router or if you want a lot of control over how API requests are processed

### Drawbacks

- Added latency in your APIs
- It cannot be fine tuned
- There aren't many alternatives vs. running your own servers
- Gateway limitations, but they are pretty large so unlikely to be causing issues

Amazon API Gateway is a service that allows developers to provide the HTTP endpoints of a REST API or WebSocket API and connect them

### How AWS API Gateway Works

Wires up the backend/front end of your website;

## [AWS API Gateway](https://aws.amazon.com/api-gateway/)

## [DynamoDB Guide](https://www.dynamodbguide.com/what-is-dynamo-db/)

DynamoDB is a hosted NoSQL database offered by AWS
- reliable performance at any scale
- managed experience (access controls)
- small, simple API allowing for simple key/value access

Best fit for
- Apps with large amounts of data and strict latency requirements (it is fast)
- Serverless apps using AWS Lambda:
- Data sets with simple, known access patterns

### Base components of DynamoDB

Component | Function
--------- | --------
Table | grouping of data records (table in a relational db, collection in MongoDB)
Item | single data record in a table, identified by a stated **primary key** of the table
Attributes | pieces of data attached to a single item (column in relational database, field in MongoDB)
Primary Key | Must be defined at creation of table, must be provided when inserting a new item (simple or composite)

Available types:
String ```"Name": { "S": "Some Name" }```
Number ```"Age": { "N": "100" }```
Binary
Boolean ```"IsActive": { "BOOL": "false" }```
Null ``` "OrderId": { "NULL": "true" }```
List ```"Roles": { "L": ["Admin", "Use"] }```
Map
String Set, Number Set, Binary Set

Unlike a relational database, you do not have to provide all non required field (attribute) entries at the time of creation, they can be added later on

## [AWS DynamoDB](https://aws.amazon.com/dynamodb/)

## [Dynamoose](https://dynamoosejs.com/getting_started/Introduction/)
