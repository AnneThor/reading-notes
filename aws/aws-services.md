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

Amazon API Gateway is a service that allows developers to provide the HTTP endpoints of a REST API or WebSocket API and connect them with appropriate functionality, can handle authentication/authorization/access control

### How AWS API Gateway Works

Wires up the backend/front end of your website;

## [AWS API Gateway](https://aws.amazon.com/api-gateway/)

## [DynamoDB Guide](https://www.dynamodbguide.com/what-is-dynamo-db/)

## [AWS DynamoDB](https://aws.amazon.com/dynamodb/)

## [Dynamoose](https://dynamoosejs.com/getting_started/Introduction/)
