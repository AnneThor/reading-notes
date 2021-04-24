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

3 main pillars to running services on AWS: COMPUTE, DATABASE, MSESSAGING

Messaging is following this model:
Producer --> message --> Consumer

Unlike emails, messaging is sent between software components, not between people

Messages have payload and attributes:
- Key/value pairs
- With business meaning, like CustomerID=12345 or MessageType=NewBooking
- Attributes remain unattributed
- You can have deep nested things here, but they're basically all just key/values

#### Queues

Queues are durable buffers for your messages. Consumers pull messages from the queue, process them, and then emit a message that the processing is complete which allows for the deleting of the message from the queue

#### Standard Queue

Highly scalable, scale as your traffic grows, you can expect the same level of latency at larger scale. But there is a tradoff - sometimes the messages will arrive out of order and sometimes they can be duplicated

#### SQS

FIFO Queue (limited throughput - meaning lowered ability to scale), but allows messages to be processed exactly once in an ordered fashion.

Can you use multiple works on an SQS? If it's a single stream of messages, no. The concept of "message groups" is a workaround here - so for example if multiple users are working from the same queue, you can assign users to the message groups so they are using their own message groups and getting their own messages in order. You can create these on the fly using a string tag

#### Pub/Sub Messaging

Publishers --> SNS Topic --> Lambda/SQS/HTTP

SNS and SQS are created by request, they are charged by usage

#### Message Streams (Amazon Kinesis)

When you send a message to a stream it gets appended to the end of the stream. This uses string shards or partitions. The main difference between a stream/queue, the stream persists, you are looking at a portion of the stream and you can just move the portion that you are reviewing incrementally. Stream is used to analyze the data over time or run multiple analysis on the same stream using multiple processes

#### Service to Service Communication

Internet --> Load Balancer --> Web Servers --> Load Balancer --> Booking Service --> Database

Messaging comes in handly in an overload scenario, you can replace a load balancer with the SQS queue (messages can be slowed down and processed at the rate that the end database can keep up with)

You can do the same thing if you're using Lambda functions to communicate with the DB (Lambda functions are very quick, so you'll want to slow them down in a queue in order not to hit the database with too many requests at once)

It is possible to batch requests (clump them together so that you are making fewer calls to the queue)

SQS - there is no maximum on the space that you can have in the queue, but there is a maximum on the period that the message can live in the queue (2 weeks is the maximum)

Recommended structure is to have one queue for background tasks with multiple workers

Notify everyone! Lambda(email notification), Notify Partner(External partner integration), Append to Ledger(Financial ledger queue), Schedule payment(Payment processing)
- potential issues are throttle scenarios, having to hard code retry scenarios
- one solution is to add Topics in the middle: Booking Topic that handles all these services; when a booking is made it goes into the Booking topic queue, and then asynchronously process them, only deleting from the queue when everything is checked off; if it fails, it will send a failure message

SQS:
- Long polling: you say how long you are willing to wait for the message, if the queue gets a message it will instantly push to you when it arrives (this means you don't need a super tight loop requesting it over and over which adds up fast in request costs)
- Server side encryption: can be turned on by flipping the switch
- Dead Letter Queues: lets say, a corrupted message from the producer that cannot be processed because it's in the wrong format, you can make policies on the queue that allow retrying the message up to 5x and then after 5 failures, move to a separate queue to inspect later on (you can actually see and process the failing messages)
- CloudWatch: easy monitoring and alarming (works well with Dead Letter Queues)
- Tightly integrated with other AWS services as a destination

SNS:
- Multiple transports (email, sms, messages, etc)
- Customizable delivery retries for HTTP
- Failure notifications
- Easily monitoring with CloudWatch
- Tightly integrated with other AWS services

It is very hard to make any complicated service without using a queue somewhere in your logic (especially with messaging)

### [SQS vs SNS](https://medium.com/awesome-cloud/aws-difference-between-sqs-and-sns-61a397bf76c5)

Main takeaway is:

**SNS is a distributed publish-subscribe service**
Messages are pushed to subscribes as and when they are sent by publishers to SNS

**SQS is a distributed queuing service**
Messages are not pushed to receivers, receivers have to **poll** SQS to receive messages; messages cannot be received by multiple receivers at the same time; any one receiver can receive a message, process and delete it
SQS is mainly used to decouple or integrate applications; messages can be stored for a maximum of 14 days

**Key Differences between SQS and SNS**
SQS | SNS
--- | ---
Queue | Topic (Sub/Pub System)
Pull Mechanism: consumers poll and pull messages | Push messages to consumers
Messages persist up to 14 days | No persistence, if consumers aren't present at time of message it is lost
Delivery is guaranteed | Delivery is not guaranteed
All consumers identical, process message the same way | All consumers are processing th emessage in different ways
Jobs are submitted to SQS and consumers can process the jobs asynchronously, if job freq increases, number of consumers can increase for parallel processing | Image processing, image is sent to 3 services (watermark, thumbnail, thank you) and they all receive teh same image and do their work in parallel

#### Use Cases

**SNS:**
- Publish and consume batches of messages
- Allow the same message to be processed in multiple ways
- Multiple subscribers are needed

**SQS:**
- Simple queue with no additional requirements
- Decoupling two apps and allowing parallel asynchronous processing
- Only on subscriber is needed
