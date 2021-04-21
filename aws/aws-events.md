# AWS Cloud Servers

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Video: Decouple and Scale Apps Using SNS and SQS](#decouple-and-scale-apps)

[Medium: Difference Between SQS and SNS](#sqs-vs-sns)

## Bookmarks

[AWS: SNS SDK Docs](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html)

[AWS: SQS SDK Docs](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html)


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
Serverless API | Serverless API is an API hosted in a way where it is only active when requests are being made against it, and these functions themselves are only available and run when they are called
Triggers | Triggers are emitting functions that cause a listening object to do some action
Dynamo vs Mongo | Dynamo is the Amazon hosted NoSQL database, Mongo is hosted by Atlas, both are cloud services
Dynamoose vs Mongoose | Dynamoose is used to communicate with Dynamo, Mongoose is used to communicate with Mongo, they are both Object Relational Models (ORMs) 

### [Decouple and Scale Apps](https://www.youtube.com/watch?v=UesxWuZMZqI)

### [SQS vs SNS](https://medium.com/awesome-cloud/aws-difference-between-sqs-and-sns-61a397bf76c5)
