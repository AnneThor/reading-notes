# AWS S3 & Lambda

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Amazon S3 Docs](#amazon-s3)

[Amazon Lambda Docs](#amazon-lambda)

[Serverless: AWS Lambda](#serverless-aws-lambda)

[Cyberhoot: Content Delivery Network](#content-delivery-network)

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
Server Instances | A sever instance is a single copy of a given software running on a physical or virtual server; if you are running two or more copies of this software on the same physical or virtual server they are separate instances
Containers | a standarized unit of software that packages up code and all its dependencies so the application runs quickly and smoothly between computing environments
Cloud Services | Services that essentially let you rent server space and perhaps prove additional computing resources, software, etc.
Cloud Architecture | The various components (db, software capabilities, etc) that use cloud services to solve business problems; so practically this refers to the way the cloud service is structured to solve the problem at hand
AWS | Amazon Web Services refers to the suite of proprietary Amazon services like Elastic Beanstalk, S3, CodeCommit, etc.
EC2/Beanstalk vs Heroku | Cloud services that allow you to essentially rent server space

## [Amazon S3](https://aws.amazon.com/s3/)

Amazon Simple Storage Service is an object storage service that offers scalability, you can choose level of access/pricing, you can choose privacy settings, works with Amazon Lambda so you don't need to add additional infrastructure.

## [Amazon Lambda](https://aws.amazon.com/lambda/)

A "serverless compute service" that lets you run code without managing servers and scales automatically for you: upload your code to AWS Lambda, set up your code to trigger from other AWS services, AWS Lambda runs your code only when triggered, only pay for the compute time you use

## [Serverless AWS Lambda](https://www.serverless.com/aws-lambda)

Users of Lambda create self ocntains apps written in a supported language/runtime and upload them to AWS Lambda which manages their execution efficiently. "Serverless" does not mean no servers, it means that you aren't managing the servers yourself, it is done automatically for you.

- Each lambda function runs in it's own container, when a function is created, Lambda packages it into a new container and executes the container on a cluster of machines managed by AWS. Charges are calculates by: RAM allocated to run the function x Time required to complete the function running.
- You can have multiple server instances executed at once, that doesn't affect functionality and you will be charged by the same metric (so good for scaling cloud based solutions)

### Most common use cases for AWS Lambda

Benefits for program that

1. have short execution time on individual tasks
2. have generally self contained tasks
3. have a large differential between lowest/highest workload of the app

Best use cases

1. Scalable APIs
2. Data Processing: example, it can be configured with DynamoDB to do some work upon creation/update of a record making it useful for notification, counters, analytics
3. Task Automation: for business tasks that don't constantly require an entire server (running scheduled jobs for cleanup, processing data from forms received on website, or moving data between web stores upon request)

### Supported languages

node, python, ruby, java (JVM based languages like Clojure and Scala), Go, C#, Powershell core, (Rust and C++ are in development)

For all supported languages, Amazon provides a SDK that makes writing and integrating Lambda functions "easy"

### Advantages over maintaining own cloud servers

- Pay per use (more cost effective)
- Fully managed infrastructure (no need to manage server hardware/network layer)
- Automatic scaling
- Tight integration with AWS products (DynamoDB, S3, API Gateway, etc)

### Limitations of AWS

- Cold start time (can be small latency between trigger and event start if your function hasn't been used in past 15 minutes, but there are workarounds)
- Function limits (15 minute hard set time out, max RAM available to lambda function, max size of zipped lambda package, hard limit on concurrency per account, paylaod size limit)
- Cost consideration (can be more expensive than EC2 or other cloud providers if you're on a large scale)
- Limited number of supported runtimes

### Pricing

AWS free tier includes 1 million AWS Lambda request and 400,000 GBseconds/month (doesn't expire at 12 months like other AWS products)

### Note on usage

Lambda functions can call other lambda functions. You can use the Lambda API (invoked via the AWS SDK in your functino code) or you can use a queue service like SNS to create an SNS message from one function and use it as an event to trigger the second function

### Create AWS Lambda function from AWS GUI Console

1. Navigate to lambda
2. Click create function
3. Name it and select desired runtime, click create function
4. Now it is created and you can write it directly in the Lambda console **not recommended because you are limited to 30MB and it has limited syntax enforcement**

### Create AWS Lamba functions using "Serverless Framework"

1. Install serverless framework on your machine: ```npm install serverless -g```
2. Create a new service: ```serverless```
3. Add the resources your function needs to the ```serverless.yml``` file ([example here](https://www.serverless.com/framework/docs/providers/aws/guide/intro/))
4. Add code to your service [example here](https://www.serverless.com/framework/docs/providers/aws/guide/intro/)
5. Deploy by running the deploy step ``serverless deploy```

**Note:** Run ```serverless login``` before you deploy to your serverless application and then you can go to the serverless dashboard to see the up to date metrics and security events for the function

## [Content Delivery Network](https://cyberhoot.com/cybrary/content-delivery-network-cdn/)

A **Content Delivery Network (CDN)** is a geographically distributed network or servers that work together to speed up internet protocol transfers.

Works like this:

- CDN copies the pages of a website to a network of geographically distributed servers, caching the contents
- CDN redirects requests from the originating sites server to the server in the CDN closest to the user, delivers cached content
- CDN communicates with originating server to deliver any content that has not been cached
- This also helps protect website against some internet attacks (like Denial of Service attacks)

This is something you want to do as a larger company, smaller ones probably don't have overloads of traffic; SMB don't need to use CDN, but larger ones do. SMB can establish a relationship with a CDN protection vendor without paying for one

An indirect benefit of the CDN is that your original server location needs less capacity because it's all distributed
