# AWS Cloud Servers

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Video: What is a Virtual Machine](#virtual-machine)

[Video: Intro to Virtualization](#intro-to-virtualization)

[Amazon EC2](#amazon-ec2)

[Video: EC2 for Humans](#ec2-for-humans)

[Video: Intro to AWS Elastic Beanstalk](#elastic-beanstalk)


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
WRRC | Web Request Response Cycle, that is the cycle of sending request objects to an API and receiving a payload in the response object

### [Virtual Machine](https://www.youtube.com/watch?v=yIVXjl4SwVo)

What is a virtual machine?

- computers can perform predefined functions and are composed of hardware; users don't interface with the hardware, the software/os does
- Operating systems are software programs that control the physical components of a computer (hardware)
- A virtual machine manager (hypervisor) allows you to run more operating system inside an operating system (i.e. if you're using OSX you can install virtualbox and you can create two virtual machines, 1 with windows8 and 1 with windows10 and you can use each of them like real computers)
- Why? Testing an application, trying new OS, run old programs that require older OS

### [Intro to Virtualization](https://www.youtube.com/watch?v=l0DfHUWMjsU)

Overview of cloud concepts: the cloud, virtualization, cloud computing

#### What is virtualization?

Computer has physical resources/hardware, typically users don't use all of hardware; servers have even more computing power
- You can put hypervisor (software allowing to create virtual machines) on a server, this allows multiple virtual machines on a single computer
- Allows for fewer physical machines (lowers CAPEX - capital expenditures) and allows for centralized management (instead of managing individual machines, just manage 1 server and lower operational expenses)

#### How does this relate to cloud computing?

We have servers running virtual machines, and additional space is accrued by adding more servers, charges accrue from use
- These servers exist in data centers
- If you exist in a location and pay for the server to meet your computing needs in a seperate location, that is cloud computing

### [Amazon EC2](https://aws.amazon.com/ec2/?ec2-whats-new.sort-by=item.additionalFields.postDateTime&ec2-whats-new.sort-order=desc)

This is "Amazon Elastic Compute Cloud", secure/resizable compute capacity in teh cloud

### [Elastic Beanstalk](https://www.youtube.com/watch?v=SrwxAScdyT0)

Deploys, manages and scales web apps and services; using manage containers that allow different platforms
You choose your platform and additional resources (amazon dbs, private cloud, etc), then upload your code
AWS does the load blaancing, provisioning, application health monitoring and will do auto scaling
You retain control over AWS resources that power your app, so you can provision them yourself later if you like
Automatically load balances and manages scale; you can tweak elastic beanstalk resources
You only pay for other AWS resources you use, the EB is free

### [EC2 For Humans](https://www.youtube.com/watch?v=lZMkgOMYYIg)

Elastic Beanstalk is an app to spin up virtual servers with EC2 (elastic computing cloud)
- Each virtual server is completely isolated from others

EC2 under compute section of AWS console
- Launch instance lets you create a new virtual machine, that just sits on a cloud somewhere

Step 1: Choose the type of instance (which kind of machine you want to run)
- You can choose which os you want there from the Amazon "images"

Step 2: Choosing Instance Type (how poweful you want it to be)
- Then you decide how powerful it should be (the options all differ by the power that they offer)
- Instances are named with first letter signifying relationship between speed/storage:
- m stands for multipurpose or t (burst) groups are good for web servers
- c is computing, cpu
- storage optimized one isn't the best for web server
- gpu better for big data analysis/machine learning
You can launch it from there or go to configure details

Step 3: Configure Instance Details
- How many instances do you want to launch (only 1 is free); you can select auto scaling that would control how many you needed based on usage
- Reserve (you always pay for it and it's always there) vs. spot (you only have it on demand)

Network settings:
- This is your own network in the cloud you'll launch this instance into, you can also assign a public IP and assign subnet
- To make sure it has a public IP you need to assign it

You can assign IAM role, allows control what you'll be able to do from this instance
- If you write something here to access another AWS virtual machine, you'll need to allow it there

Shutdown behavior:
- Stop or terminate

Step 4: Storage
- Uses Elastic Block Storage, storage will also be available if you shut the instance down
- The virtual machine has some internal storage, but if you shut it down that will be lost to
- Attaching external storage makes sense if you want to keep some data after the instance is shut down

Step 5: Add Tags
- Allow you to trace instance

Step 6: Configure Security Group
You have to attach them to each instance
- What you do in a security group is decide which traffic can enter and leave the instance (basically it is a firewall)
- Choose "MyIP" and not the default setting because that will allow the entire internet to access the internet
- If your IP changes or is not static you will need to adjust this, or setup a range that will reflect the range of IP that you will potentially use
- CIDER is a tool to decode IP ranges

Step 7: Review and Launch
- You are prompted to enter a key pair or create a new one, this is necessary to log into an instance
- If you ever lose it you cannot get it again and you will lose access to your access, so you will need to download and store that

After that is saved, you can launch the instance and do whatever you want with it. You can install software, it has a public IP. It has a public DNS, if you enter that in a browser, nothing will happen (which makes sense because you have enabled only port 22 and not port 80).
- You could turn it into a web server
- You could run big data
- You could turn your vacation photos into some other format
- You will not have a computer for it though, you'll have to communicate via terminal

Open it in the terminal, navigate to where you saved the key file
- Then use ```ssh -i name-ec2.pem ec2-user@publicDNS```
- Then enter and you'll get a warning about unprotected key file
- You cannot connect as long as the pem file is unprotected; you can change the mode of the file to read only using the below command
- ```chmod 400 deco2-ec2.pem```
- Now rerun the command to connect to the database
- Now you are running the virtual machine and you can see the files that you are running in the cloud
- We were able to connect because we opened the 22 port in the setup
- Now you can do whatever you want, install software, run code, etc.
- To change to web server, you need to change some settings, etc.

In EC2 Management console you will see the running instances, there are many options on the left
- Events, tags, logging information about your instances
- Manage your block storage (EBS)
- Security groups settings where you can manage your security groups

Security groups:
- Inbound is how you manage who can access your instance (should be as narrow as possible)
- Outbound rules are very relaxed, all traffic is allowed to access anything; generally this is not as critical and you do want to give your instance access to the internet

Elastic IPS:
- Your instance does have a public IP address (you can view it when you click the instance; this changes when you shut down and restart the instance)
- As long as you stay in the AWS world you can work with dynamic IPs
- Elastic IPs are fixed IPs that you can allocate to your account and assign to instances (so even if you shut it down and bring it back up you will get back the same elastic/fixed address)

Load balance/Auto scaling allows you to distribute incoming traffic across all instances or bring up/shut down instances as traffic changes

Shut down: Right click/Instant State/Terminate or you can select it, click on actions and do Instant State/terminate
- Warning that EBS storage will be removed as well (which means it will delete all associated data)
