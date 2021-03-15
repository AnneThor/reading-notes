# Access Control (ACL)

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [Video: Role Based Access Control](#role-based-access-control)
- [Five Steps to Simple Role Based Access Control](#5-step-simple-rbac)
- [Wikipedia: Role-based Access Control](#wikipedia-rbac)

## Review, Research, and Discussion

### When is Basic Authorization used via Bearer Authorization

### What does the JSON Web Token package do

### What considerations should we make when creating and storing a SECRET?

## Vocabulary Terms

Term | Definition
---- | ----------
Encryption | Encryption is taking a non reversible hash code and turning the plain text information into a code
Token | A token is an encoded permission that a user can carry around and use to validate their access to different web resources
Bearer | The bearer is the client who is using the token to access various resources
Secret | A secret is a private string that is incorporated into encoding and encrypting resources to increase security; secrets should not be accessible to users or publicly available
JSON Web Token | A web token that consists of 3 parts: (1) the header with the algorithm used to encode the contents and the type, (2) the payload that contains the information being shared, name and password, any other information, (3) the signature verification that is encoded with the first two parts plus a secret key that is used to ensure authenticity (if the payload or other componenents are tampered with the signature will no longer match so it will be invalidated)

## [Role Based Access Control](https://www.youtube.com/watch?v=C4NP8Eon3cA)

role-based-access-control

## [5 Step Simple RBAC](https://www.csoonline.com/article/3060780/5-steps-to-simple-role-based-access-control.html)

## [Wikipedia RBAC](https://en.wikipedia.org/wiki/Role-based_access_control)