# Express REST API

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [Hacker News: Passwords With BCrypt Hashing](#bcrypt-password-hashing)
- [Wikipedia: Basic Access Authentication](#basic-access-authentication)
- [OWASP: Authentication Cheat Sheet](#authentication-cheat-sheet)
- [BCrypt: npm Docs](https://www.npmjs.com/package/bcrypt)

## Review, Research, and Discussion

### Explain what a “Singleton” is (in Computer Science terms)

A singleton is a design pattern set up so that the class is instantiated only once in a program anonymously. In this way it can only be accessed through it's getInstance() method and doesn't "pollute the global namespace" by adding extraneous global variables. 

In the context of this class, we have used that word "singleton" to refer to a middleware function in node that we export anonymously, but it's not making an object, it is has been a standard format middleware function taking (req, res, next) and performing some modifications on the request object.

### Explain how the Singleton pattern can be used with Node modules, specifically with classes

To use a singleton pattern in this way, you would update your module.exports from the class object to export a new instance of the class. Then when you're passing the reference around in module.exports, what you are passing around is an instantiated object reference (vs. exporting the class functionality and making new instantiations of it each time you import it in a file). Example of singlteon pattern exporting classes in node.js below:

``` JavaScript

class Singleton {
  constructor() {}
}

module.exports = new Singleton();
```

### If you were tasked with building a middleware system like Express uses, what approach might you take to construct/operate it?

What express is doing is simplifying the HTTP requests and providing syntactic sugar to make it easier and more concise to write/read routing options. So you're just abstracting the actual HTTP base requests into more simple language. If I were writing that myself, I could definitely copy that implementation of the global module.exports function, that is the trick.

### Document the following Vocabulary Terms
Term | Definition
---- | ----------
Router Middleware | Functions that modify the request object in a WRRC cycle
Dynamic Module Loading | Functions are exported to the module.exports object and are referenced only when needed in the program, allowing for modularity
Singleton Pattern | Pattern of exporting an instantiated class so that you are referring to the same instance throughout the program
CRUD -> REST Method Matches | Here are the pairs: Create/POST, Read/GET, Update/PUT or PATCH, Delete/DELETE
Mock Testing | Testing implementation where instead of running tests on your real server or real database, you create mock implementations so that you are testing while not changing the actual resource.

## [BCrypt Password Hashing](https://thehackernews.com/2014/04/securing-passwords-with-bcrypt-hashing.html)

Passwords and other sensitive information are never saved as text, they are always hashed. They are still somewhat vulnerable if they hash used is possible to reverse engineer.

### Problems with cryptographic hash algorithm

It is not possible to reverse a hash, but you can **"brute force"** solve it by trying inputs until you find one that matches the hash output (that is obviously done through automation). **Hash collision** attacks work by taking advantage of the fact that two inputs can result in the same has result.

### BCrypt solution 

Provides a slower and variable work factor to determine how slow the hash will be, making it extremely resistant to brute force attacks. Secure enough for most web apps.

## [Basic Access Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication)

**Basic access authentication** is a method for an HTTP user agent (i.e. web browser) to provide a username and password along with a request. The basic config is a header field in the form of ```Authorization: Basic <credentials>```. This is the most simple authentication method becuase it does not require cookies or anything additionally/externally to that header field (and it is also not encrypted or hashed in any way).

BA is sent in the header, so it must be cached by the browser to avoid having to constantly ask the user for re authentication. 

HTTP does not provide a logout protocol, despite offering the login protocol. Cached credentials are usually clared when the user clears browsing history. 

### Server side protocol

If the server wants authentication, it must appropriately respond to unauthenticated requests with a *HTTP 401 Unauthorized* status and a *WWW-Authenticate* field

### Client side protocol 

To send credentials to server, use the *Authorization* field constructed like this:

- username:password (username itself cannot include a colon)
- above string encoded into octet sequence; charset is unspecified by default, but can be added to charset parameter (i.e. ```charset="UTF-8"```)
- above encoded using a variance of Base64
- authorization method and a space (s.a. "Basic") is prepended to encoded string 

## [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

**Authentication** verifying user is who they claim to be by means of username, id, some other personal info
**Session Management** is how the server retains the state corresponding to the authenticated user; this should be passed back and forth in request/response and computationally very difficult to predict

### Authentication General Guidelines

**User IDs** make them case insensitive and secret (high security: usernames can be assigned and secret)
Do **NOT** allow login with sensitive accounts to front end user interface or allow same authentication solution used internally for public access
**Password Strength Controls** help prevent automated hacks

- minimum length (greater than 7 characters is general standard)
- maximum should not be set too low (common max is 64); important to set the max length 
- do not truncate passwords
- allow usage of all characters including unicode and whitespace
- ensure credential rotation when anything is leaked
- include password strength meter to guide user towards more secure password

**Other points**: implement a secure password recovery mechanism, store passwords securely, compare password hashes using safe functions, login landing page should be on secure TLS (Transport Layer Security), require re authentication for secure activities

TLS Client Authentication: requires both browser and server to provide TLS certificates during the handshake process; this sort of thing is used in intranet and non public websites generally 

### Authentication and Error Messages 

HTTP/HTML should both respond in a generic manner.
**Authentication Responses** should indicate if userid/password was incorrect, if accoutn does not exist, if account is locked or disabled; the point is that if a function differentiates in the time for a response between bad id/bad password, etc. an attacker can use this time differential to determine the kind of error occurring. This creates a bad UX though, so it needs to be balanced against the security needed for the website.

HTTP Error Codes: 200 for a positive result and 403 for a negative result

**Types of Automated Attacks**

1. Brute force: testing passwords from a dictionary or other source against a single account
2. Credential stuffing: testing username/password pairs obtained from another site
3. Password spraying: testing a single, weak password against a number of accounts 

**Multi Factor Authentication (MFA)** is the best defense against the above mentioned attack methods, shoudl be implemented where possible 
**Account Lockout** is another option and should use the number of login attempts on the account itself (not number from specific IPs) in order to prevent a hacker trying from different IPs. When designing this strategy you need to account for number of failed attempts required to lock the user (lockout threshold), time period that these must occur within (observation window), and how long the account remains frozen (lockout duration).
**CAPTCHA** can help, but they can be solved with automation, therefore should be viewed more as a device to slow down automation attempts and may be more effectively applied only after a few failed attemps than up front
**Security Questions** can be thought of also as a defense in depth mechanism rather than an actual MFA 

### Logging and Monitoring 

Enabling these on lgoin attempts ensures everything is reviewed

### Options Not Requiring Transport of Password

3rd party apps that require connecting to a web app, either from a mobile device or another website, etc. are not recommended for MFA/password scenarios because you're extending the interface that this information travels 

### OAuth

Allows an app to authenticate against a server as a user, without requiring passwords or identity provider; uses a token from the server/services tells the server which user is using the service. Recommended to use OAuth2.0, which is used by many large companies and uses HTTPS

### OpenID

HTTP based protocol that uses identity providers to validate user authenticity. This allows the user to carry their authentication across multiple websites without reentering information. Simple protocol that allows service provider initiated way for SSO (single sign on).

### SAML (Security Assertion Markup Language)

Completes with OpenID, also uses identity providers, but is XML based and more flexible. It can be initiated by a service provider or an identity provider

### FIDO (Fast Identity Online) 

Universal Authentication Framework (UAF) and Universal Second Factor (U2F) protocols, both based on public key cryptography challenge-response models; take advantage of existing security technologies present in devices and plug them into a common authentication framework; UAF works with native apps and web apps; U2F augments password based authentication using a hardware token (like a USB) that stores cryptographic authentication keys and uses them for signing (protects against phishing by using URL of website to look up stored authentication key)

### Password Managers 

Web apps using password managers should: user standard HTML forms for username/password with appropriate "type" attributes, avoid plugin pased login pages (Flash, Silverlight, etc), implement reasonable max password length, allow all printable chars in passwords, allow users to paste into username/password fields, allow users to tab between fields
