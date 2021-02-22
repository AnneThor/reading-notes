# NodeJS, Express, TDD, CI/CD

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## What’s the difference between PUT and PATCH? (HTTP verbs)

PATCH and PUT are both methods that apply modifications to an existing resource. The difference is that PATCH is a *partial* modification of the resource and PUT is a *total replacement* of the existing resource or *creation* of the resource. Another important difference is that calling PUT repeatedly will always yield the same result with no side effects; repeated calls to PATCH are not guaranteed to do so (ex/if there is an auto increment field in the resource, this may keep incrementing with repeated PATCH calls)

MDN Docs:

**[PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH)**

**[PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT)**

## Provide links to 3 services or tools that allow you to “mock” an API for development like json-server

The purposes of a fake API for development are manifold:

- Tests of the servie you are building must not rely on external resources
- The API could be slow or rate limited (which could incur costs)
- Often developers need to work before the API is ready to be connected

More useful to think about these as "test doubles" than "fakes"

Potentially useful mock API services for development:

- [json-server](https://www.npmjs.com/package/json-server)
- [mirage-js](https://miragejs.com/docs/getting-started/introduction/)
- [jest](https://www.npmjs.com/package/jest)
*Note with Jest you would be using ```jest.spyOn``` and ```Fetch``` in testing along with your own version of the Fetch call to evaluate the functionality*
- [JSON Schema Faker](https://github.com/json-schema-faker/json-schema-faker)
- [fake{JSON}](https://fakejson.com/)

**[How To: Rapid Development Via Mock APIS](https://www.freecodecamp.org/news/rapid-development-via-mock-apis-e559087be066/)**

[4 Ways to Fake an API](https://www.valentinog.com/blog/fake/)

## Compare and contrast Swagger and APIDoc.js 1 Which HTTP status codes should be sent with each type of (un)successful API call?

**[Swagger](https://swagger.io/docs/specification/describing-responses/)** allows you define the range of responses as 1XX, 2XX, ...5XX. If you provide the explicit code (i.e. 404) that will take precedence over the blanket request (in this case 4XX). Each response status requires a description. API specs do not necessarily need to cover all possible codes, but known error codes like 404 must be addressed.

**[APIDOC.js](https://apidocjs.com/)** you can also define the errors in groups, if you do not provide the specific code the default error ```Error 4xx``` is thrown. You can set the title and description of the errror with ```@apiDefine```

## Compare and contrast SOAP and ReST

**[SOAP (Simple Object Access Protocol)](https://en.wikipedia.org/wiki/SOAP)**
SOAP uses XML data in request/response messages, relying on XML Schema and other technology to enforce/parse structure of messages. It is platform indepedent (i.e. clients can use from disparate OS). SOAP is extensible, neutral, and independent of specific programming models.

**[REST (Representational State Transfer)](https://en.wikipedia.org/wiki/Representational_state_transfer)**
Standard software architecture for web services. This is a webservice that provides it's web services in a text form allowing them to be read and modified with a stateless protocol and predefined operations. Common, stateless protocols allow for operability amongs disparate OS.

REST has become the more popular communication protocol. REST accesses data whereas SOAP performs operations by a standardized set of messaging patterns. Each are scalable and either can be used to achieve most outcomes. Benefits to REST include: not restricted to XML formatting, JSON is faster and provides a uniform base for transferring data to browser clietns, used by major services like Google, generally faster. Since either can be used to acheive the same results, the preference is up to the developer with the ultimate end goal in mind. SOAP can be preferable due to more robust security guarantees, built in logic to handle failed communications, and a set of standardized processes that make it easier to implement across firewalls/proxies without needing to modify the protocols themselves.

## Reference Terms

Term | Definition
----- | -----------
Web Server | System of one or more computers that accepts incoming HTTP requests and sends corresponding HTTP responses; primary function is to deliver web contents/resources to the client (requestor). "Web Server" can refer to the hardware (computer storing web resources and software) or the software itself(that controls how users interact with hosted files, minimum functionality HTTP)
Node | Open source runtime environment allowing for developers to create server side applications in JavaScript
Express | Express is a Node framework that simplifies HTTP verbs and allows for concise creation of HTTP verb handlers, simple integration with different view engines (i.e. porting information into front end templates), creates common settings such as port to use for connecting, and allows for additional request processing (middleware) to be inserted into request handling. It is unopinionated, meaning that you can structure it in various ways and use many different middleware options in myriad configurations.
Routing | Routing is how the client request is connected to the code they will receive
Server Side Routing | The code served to the user is determined by the url and corresponding HTTP request. Pros: requests only needed data, search engines are optimised for webpages that come from the server. Cons: each request triggers a full page refresh (can be redundant), full page refreshing can take time (dependent on website contents)
Client Side Routing | The page is loaded and the code served to the user is determined by the JavaScript loaded on the page. Adjustments to the url cause a change in state with the JS, but do not trigger full page reload. Pros: less data is processed during client interactions resulting in generally faster UI, easier to implement smooth transitions/animations. Cons: Entire website must be loaded on first request (can be slow), potential to download unnecessary resources, requires more set up, less optimised SEO
Web Request Response Cycle (WRRC) | This is how information flows through a web app: client opens brower/enters url, request gets sent to the server which takes the url request, server follows it's routing to serve back the requested web document and passes it to the view, the view renders the information in the desired format back to the client in their browser

- [Wikipedia: Web Server](https://en.wikipedia.org/wiki/Web_server)
- [MDN: What is a web server?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
- [Medium: Server vs. Client Side Routing](https://medium.com/@wilbo/server-side-vs-client-side-routing-71d710e9227f)

## Notes for Required Readings

- [MDN: Express/Node Introduction](#mdn-introduction-to-express-node)
- [NPM Docs: About npm](#npm-docs-about-npm)
- [What is TDD?](#tdd)
- [Video: Github: Continuous Integration Continuous Delivery](#video-github-cicd)

### [MDN Introduction to Express Node](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/Introduction)

- Middleware and routing functions are called in the order of declaration (so if one depends on another observe the proper order of instantiation)
- The **only** difference between middleware and route callbacks are that middleware **require** the ```next``` function to be passed as last parameter in express
- ```static()``` is the only middleware that is *part* of Express: when you make a static path you can later refer to files RELATIVE to the static path
- error handling requires four parameters ```(err, req, res, next)```; these must be called after all other ```app.use()``` calls so they are the last middleware in the process
- Express can be setup to interact wiht database mechanisms directly or by using an ORM (Object Relational Mapper). The benefit of ORM is that it allows the developer to proceed in terms of objects rather than database semantics.
- Express allows for "view template" to be specified:

```JavaScript
//you have to require/instantiate the package containing template library first

//Set directory to contain templates ('views')
app.set('views', path.join(__dirname, 'views'))

//Set view engine to use 
app.set('view engine', 'some_template_engine_name')
```

### [npm Docs About npm](https://docs.npmjs.com/about-npm)

- World's largest software registry feat. work by open source developers from every continent
- Core components:

Name | Function
----- | ---------
**website** | browse packages, set up profiles, manage access
**Command Line Interface(CLI)** | runs from terminal, how developers interact with npm
**registry** | large public database with repositories, metadata, and docs 

- Run packages w/o downloading them with ```npx```
- Free to share packages publicly

### [TDD](https://www.agilealliance.org/glossary/tdd)
Rules for Test Driven Development (TDD hereafter):

1. Write unit tests describing an aspect of the program
2. Run the tests (at first they will all fail because you haven't written the functionality yet)
3. Write the minimal amount of code to make the tests pass
4. Refactor the code to meet simplicity criteria (no duplication, separation of concerns, comprised of a minimum number of components)
5. Repeat the process as you add functionality to the app

Benefits:

- Fewer bugs
- Initial time investment offest by reduction in final phase troubleshooting
- Improved strcuture due to forced planning at outset

Things to Avoid:

- Neglecting to run tests frequently or writing too many tests at once
- Writing either too wide or too narrowly scoped tests 
- Focusing tests on trivial asepcts
- Failure for group to adhere to development process (poor maintenance of test suites)
- Abandoned test suites

Intermediate to Advanced Implementations:

- Write a test exposing a bug/defect before fixing it
- Ability to decompose proram into several unit tests
- Ability to factor out reusable components from existing tests
- Roadmap planned tests and revise as encessary
- Ability to compare testing for design paradigms/technical domans

### [Video Github CICD](https://www.youtube.com/watch?v=xSv_m3KhUO8)

Idea is that when multiple developers are working on a single project in separate branches the workflow can diverge on each branch far from the main. 

Continuous Integration Advantages:

- Ensure everyone's changes integrate together, reduce merge conflicts
- Catch bugs

Most CI systems integrate automated testing using dedicated server or CI service, when code is added, the server will automatically test the code before integraton. CI server produces output to describe any difficulties in the code. This enables ...

Continuous Delivery:

- Developing software such that it could be released at any time
- Develop features with moudular code

Continuous Deployment:

- Deploy new features immediately with confidence

In regards to GitHub: developers push their code to GitHub, GitHub uses web hooks to manage updates to your project (you can control who is notified of events or CI server can be updated), CI server will parse the message, build the branch and run the tests, CI server then returns the status message about the commit, GH will display the results and provide verbose results allowing you to review results before they are integrated. CI server can be confiured to deploy branches as part of processes. GH also has a deployments API that works with webhooks that will allow passing versions to be passed to the environment you specify.
