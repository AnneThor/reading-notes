# Access Control (ACL)

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [Video: Role Based Access Control](#role-based-access-control)
- [Five Steps to Simple Role Based Access Control](#5-step-simple-rbac)
- [Wikipedia: Role-based Access Control](#wikipedia-rbac)

## Review, Research, and Discussion

### When is Basic Authorization used vs Bearer Authorization

**Basic Authorization** is when a client enters login information: username and password. This information is checked against the database information and if it is accurate a JWT (or similar mechanism) is initiated. The client/user is allowed access to the resource that the login redirects to.

**Bearer Authorization** is when access is provided to a restricted portion of a website by checking the JWT or session data and if the information is valid they are allowed access to the resource (the client would only be aware of this process if it failed, it does not require manual input of information from the user).

### What does the JSON Web Token package do

The JSON Web Token package creates the JWT based on the input parameters you provide. You can pass it options, and then give it the body and signature and it will generate the token based on your specs. Specifically, you can tell it the encoding algorithm to use, the expiration time, and other parameters. You then give it the body of information to hold, and the secret to sign the token with.

### What considerations should we make when creating and storing a SECRET?

This should be a private string that is not easily guessed and it should be stored in a location where the user does not have access to it and it is not publicly accessible at all.

## Vocabulary Terms

Term | Definition
---- | ----------
Encryption | Encryption is a non reversible system of encoding information using a hash code and turning the plain text information into a code
Token | A token is an encoded permission that a user can carry around and use to validate their access to different web resources
Bearer | The bearer is the client who is using the token to access various resources
Secret | A secret is a private string that is incorporated into encoding and encrypting resources to increase security; secrets should not be accessible to users or publicly available
JSON Web Token | A web token that consists of 3 parts: (1) the header with the algorithm used to encode the contents and the type, (2) the payload that contains the information being shared, name and password, any other information, (3) the signature verification that is encoded with the first two parts plus a secret key that is used to ensure authenticity (if the payload or other componenents are tampered with the signature will no longer match so it will be invalidated)

## [Role Based Access Control](https://www.youtube.com/watch?v=C4NP8Eon3cA)

Roles: your function, that may vary what kind of files you can access

User -> Role -> Rights (rights are associated with the ROLE not the USER)

First: identify the roles and what resources they need access to. Users must be activated into one or more roles. In enterprise setting this is likely based on your job. Start with authentication, but then activate one or more roles.

Benefits of RBAC: policy doesn't need to be updated when users change roles (you just change their role); new employee should be able to activate the desired role; revisiting least privilege (you grant the minimum level of access to the user that they need);

## [5 Step Simple RBAC](https://www.csoonline.com/article/3060780/5-steps-to-simple-role-based-access-control.html)

RBAC = assigning access privileges based on role

### Variations on RBAC

1. ACL: Access Control Lists, defining access by user or user group (i.e. some users can edit a document, other users can only look at it)
2. ABAC: Attribute Based Access Control: sometimes known as "policy based access control" determines acces by user department, time of day, location of access, type of access, etc. 

### RBAC Implementation

1. Inventory your systems (e.g. customer database, contact management system, etc)
2. Create roles based on your scenario (keep minimal and stratified)
3. Assign people to roles
4. Never make one off changes, focus on standardization
5. Audit with the goal of keeping access minimized to essential function of roles

## [Wikipedia RBAC](https://en.wikipedia.org/wiki/Role-based_access_control)

RBAC is an approach to restricting access to authorized users and is used by majority of >500 employee enterprises. Defined around roles/privileges

Three primary rules of RBAC:

1. Role Assignment (user can only perform functions assigned to their role)
2. Role Authorization: user's active role must be authorized for the user
3. Permission Authorization: user can only exercise functions assigned to their active role 

Roles can be designed in a heierarchical way (meaning higher levels have access to all functions of lower level roles)
Does not look at message context (such as connection source) like Context Based Authorization Control

Comparison to ACL: ACL grants or denies access to a given file(s), but it wouldn't get as granular as dictating how a file could be changed by a certain user group.
