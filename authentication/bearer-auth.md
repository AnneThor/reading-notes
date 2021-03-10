# Bearer Authorization

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [Video: JSON Web Tokens Explained](#json-web-tokens)
- [JWT: Introduction](#intro-to-json-web-tokens)
- [Stack Overflow: JWT Security](#jwt-security)

## Review, Research, and Discussion

### Write the following steps in the correct order

- Register your application to get a client_id and client_secret
- Ask the client if they want to sign in via a third party
- Redirect to a third party authentication endpoint
- Receive authorization code
- Make a request to the access token endpoint
- Receive access token
- Make a request to a third-party API endpoint

### What can you do with an authorization code?

The authorization code is used by the web app to create the web token that will be passed back and forth in the header.

### What can you do with an access token?

The access token is send in the header of HTTPS requests and allows the user to remain logged in (sometimes across several services) without the need to provide credentials repeatedly. You can use this as the key to request data, which may only be available to users with some credentials.

### What’s a benefit of using OAuth instead of your own basic authentication?

It provides an added layer of abstraction and checking to your authentication scheme that guarantees not only authentication but also authorization.

## Vocabulary Terms 

Term | Definition
---- | ----------
Client ID | Infomormation identifying the user/client
Client Secret | A secret string stored in an app's server used to create web tokens (part of the encoding process)
Authentication Endpoint | Where the server checks the user's credentials 
Access Token Endpoint | Where the server takes the JWT and validates it
API Endpoint | The url of a server or service where APIS can access the resources needed to carry out their function
Authorization Code |The authorization code is provided by the 3rd party authentication endpoint to the web app, which exchanges this for a woken that is made from the code, client ID and client secret
Access Token | Such as a JWT, an encoded object that does not contain any sensitive information, but has a verifiable signature that allows the user to remain signed in for a period of time  

## [JSON Web Tokens](https://www.youtube.com/watch?v=926mknSW9Lo) 

JSON Web Tokens (JWT): open standard (any can use) and securely transfer info between any 2 bodies, it is digitially signed (verified and trusted) and there is no alteration of data during the transfer, it is so compact it can be sent in the url (in the HTTP Header), fast transmission, self contained (the token itself contains data about the user and prevents querying the database more than once)

Someone can login once, then use the token for further requests (do not need to keep logging in)

JWT uses: authentication, exchanging info 

JWT Structure: aaaaaaa.bbbbbbbb.cccccc

- a part is the header (base64URlencoded)
- b part is the payload (base64URlencoded)
- c part is the signature 

Header is a JSON object that includes 2 properties: "alg"/algorithm and "typ"/

- algorithm is the algorithm used to encode it
- type is type of token (in this case "jwt"

Payload is also a JSON object that contains the claims (user details or additional metadata like the expiration time of the token)
Signature is base64Urlencoded header concatenated with the case64UrlEncoded payload + the secret

Browser sends request with credentials, server checks and generates JWT with secret and that is passed back to browser, if that is successful, the JWT will be sent in the header in further communication to the server (if JWT signature matches user info, it allows access to the protected fields; if they do not match it will not return the information)

**Because secret is created with part of the payload, if you change the payload the secret will not match and the signature will not longer be valid!**

## [Intro to JSON Web Tokens](https://jwt.io/introduction/)

JWT is an open standard, compact, self contained way to securely transmit info between parties in JSON objects; verifiable/trusted bc it is signed with a secret (HMAC algorithm) or public/private key pair. They can be encrypted, but we're looking at *signed* tokens. **Signed** tokens verify integrity, **Encrypted** tokens hide their claims. Tokens signed with public/private keys vertify only the party holding the private key as the signer.

### When to use JSON WT 
- Authorization (after user is logged in JWT will allow them to access their approved resources, Single Sign On is a feature that uses JWT due to it's small size and cross domain functionality)
- Information Exchange (signatures ensure integrity of senders and that contents haven't been tampered with)

### What is the JWT Structure
header.payload.signature 
- Header: typically consists of two parts (type of token - JWT, and signing algorithm - s.a. HMAC SHA256 or RSA)
- Payload: contains the "claims", that being user details and additional data
  - **Registered claims** are predefined (not mandatory but recommended like iss(issuer), exp(exp time), sub(subject), aud(audience), etc.
  - Claim names are 3 chars long bc JWT is meant to be compact 
  - **Public claims** 
  - **Private claims** custom claims to share info between parties that are neither registered or public
  - **Important Note** for signed tokens information is protected from tampering but it is **NOT PRIVATE** (you have to encrypt for that)
- Signature: created by taking the encoded header, encoded payload, a secret, adn the algorithm specified in the header and sign that 

**[jwo.io debugger](jwt.io)** is a good place to experiment/learn about JWT

### How do JWT work?

- When a user successfully logs in a JWT will be returned: take care with them (keep them only as lnog as necessary)
  - Do not store sensitive data in browser storage due to security
- When the user wants to access a protected route or resource, the user agent should sent the JWT
  - Typically in the **Authorization** header using the **Bearer** schema
  - ```Authorization: Bearer <token>```
- To access protected routes, server will check for this token (& it may reduce complexity of queries)
- If JWT is sent in ```Authorization``` header, CORS will not be an issue
- User request --> Authorization Server --> Sent back with JWT (or not) --> Request out to API/Resource server 
- **Signed tokens are exposed to everyone, they are just immutable, do not put secret information in them!**

### Why use JWT?

Why JWT and not SWT (Simple Web Token) or SAML (Security Assertion Markup Language Tokens)

- JSON is less verbose (than XML used in SAML)
- JWT and SAML allow for more complex signatures, SAML signatures are more prone to obscure security holes
- JSON is easier to parse

## [JWT Security](https://stackoverflow.com/questions/27301557/if-you-can-decode-jwt-how-are-they-secure)

### Question is: how are JWT secure if you can capture, deconstruct and decode the header and payload? 

- A signed, but not encrypted JWT has readable contents, but if you don't know the private key you cannot change it
- If it is encrypted you cannot read it
- If a 3rd party DOES know the secret, they could calculate the signature and update the message
- JWT should always be transmitted on a secure HTTPS server, but should also not contain sensitive information
- Stateless and therefore perfect for REST APIs
- **JWT are used for AUTHORIZATION, AUTHENTICATION is done by the server**
