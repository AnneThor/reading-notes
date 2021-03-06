# Event Driven Architecture

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Video: SNS vs SQS](#sns-vs-sqs)

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
FIFO Queue | A queue that releases items in the order that they were received, "first in first out"
Pub/Sub | Publish/Subscribe architecture, where one object publishes an event and this is broadcast to all "subscribers" to the event; there is a distinction between this model and a message queue as the publisher does not need to know who is subscribed to the event

### [SNS vs SQS](https://www.youtube.com/watch?v=mXk0MNjlO7A)

When to use SNS vs SQS based on a concrete example

SNS | SQS
--- | ---
Simple Notification Service | Simple Queue Service
Publisher/Subscriber System (you own a topic and publish to it, subscribers get notices) | Queuing servie for message processing 
Deliver to many subscribers of various types (SQS, Lambda, Email) | Must poll the queue to discover new events, nothing automatically invoked, separate thread to poll the queue and process/delete from queue
Broadcasts to multiple parallel services | Messages processed by a single consumer/service typically 
 
How to determine which to use?

- Do other systems care about an event? **SNS**

- Does your system care about an event? **SQS**

#### Credit Card Transaction 

User making a purchase via website, entering credit card info
Making purchase request to some REST API
Payload includes transaction id, customer id, email, amount

1. Communicate with CC authority service for authorization
2. So the event is that the purchase went through and this event is important to several subsystems in your e commerce platform
3. Publish an event on this SNS topic, there may be multiple listeners with different purposes, for this example 3 in parallel that receive the save event/payload

  - Customer reminder service: email customer to confirm the order (Lambda function, maybe communicates with db for some details)
  - Analytics queue (SQS): maybe does something like analyze orders per day, etc. (a separate transaction analytics service like EC2 may poll this queue)
  - Fraud detection (SQS): something similar to the analytics queue, but entirely a separate concern (message deleted from both of those queues after processing)

SQS has the guarantee that the information will be processed before it is deleted; SNS can potentially lose events if there are errors in your Lambda/function processing
