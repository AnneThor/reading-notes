# Event Driven Programming

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Digital Ocean: Event Driven Programming in Node](#event-driven-programming-node)

[Node: Events Documentation](#node-documentation)

## Review, Research, and Discussion

### Why is access control important?

Access control is important to limit the control users have over changing the content of a webpage or accessing unauthorized information.

### Describe an application that would need access control.

Most applications that have user signup and signin capabilities require access control. For example, I have sold things on ebay since I was a teenager. In my interactions with this app I have been a simple buyer and a seller. As a user it is not desirable for me to have access to updating the listings of other users, so my role should be restricting to updating/editing records that myself have created (likely there are multiple layers of capability nested therein). At the same time, as a seller I want the sales portfolio section of my account to be limited to my private view.

### What is a role used for?

Roles are pre defined access levels that are used to determine which capabilities different users will have. Roles are generally assigned when the users are created and are meant to be inflexible (to avoid having an infinite number of hugely nuanced capability sets). The goal is to give users the minimal capabilites they need to meaningfully interact with the software.

### Why is role based access control more scalable than discretionary or mandatory access control?

It is scalable because the idea is to predefine role sets and be inflexible with them - as you scale up to a greater number of users, the roles and associated capability sets remain the same.

## Vocabulary Terms

Term | Definition
---- | ----------
Authorization | Permissions needed by the user to access certain content (Basic authorization are login credentials, bearer authorization is a token given to a user after successful login that is used to restrict/direct traffic in a website)
Role Based Access Control | Roles are assigned to users, each role has a pre defined set of minimal capabilties needed for successful interaction with the program
Capabilities | Powers such as read, create, update or delete that are assigned to various roles in RBAC; users can have multiple capabilities and multiple roles can have access to the same capabilites

### [Event Driven Programming Node](https://www.digitalocean.com/community/tutorials/nodejs-event-driven-programming)

### [Node Documentation](https://nodejs.org/api/events.html)
