# shortly-express
This is a project I completed as a student at [hackreactor](http://hackreactor.com). This project was worked on with a pair.

# SPRINT THIRTEEN: Shortly Express
Get ready for full-stack app development with Shortly! Shortly is a URL shortener service similar to Bitly - but is only partially finished. Your goal is to build out an authentication system and other features that will enable users to have their own private set of shortened URLs.

While some aspects of the authentication system have been provided, you will need to think about how to approach authentication from both a user perspective and a technical perspective. Some questions to think about:

* What additional steps will the user need to take when interacting with the application? * More specifically, what additional routes will the application need to handle?
* What strategies do I need to employ to secure existing site functionality?
* How often should the user need to enter their username + password?

**What's in this Repo**

This repo contains a functional URL shortener designed as a single page app. It's built using [Backbone.js](https://backbonejs.org/) on the client with a Node/Express-based server, which uses [EJS](https://ejs.co/) for server-side templates.

It uses [MySQL](https://www.mysql.com/), an open-source relational database management system (RDBMS).

Server side, the repo also uses [express 4](http://expressjs.com/). There are a few key differences between express 3 and 4, foremost that middleware is no longer included in the express module, but must be installed separately.

Client side, the repo includes libraries like [jQuery](https://jquery.com/), [underscore.js](https://underscorejs.org/) and [Backbone.js](https://backbonejs.org/). Templating on the client is handled via [Handlebars](http://handlebarsjs.com/).

This repo includes some basic server specs using [Mocha](https://mochajs.org/). It is your job to make all of them pass, but feel free to write additional tests to guide yourself. Enter npm test to run the tests.

Use [nodemon](https://nodemon.io/) so that the server automatically restarts when you make changes to your files. To see an example, use *npm start*, but see if you can improve on this.

**How to use the provided code**

This repo contains many lines of provided code, and spending too much time reading this code will make it hard for you to complete the sprint. You **should not** need to inspect the client side code to complete the bare minimum requirements. You **will** need to use methods in server/models/ and server/lib/. Documentation for these methods has been provided in the docs/ directory - just open docs/index.html in your browser. Note: the documentation is generated as part of the postinstall npm script - so if you haven't run npm install yet, the docs will not be available.

**Reference Material**

* Express 4 API
* Sessions and Security - this is a Rails resource, but it's a really good explanation.
* Node cypto Module
* How does a web session work?
* What is a cookie?
* How does cookie-based authentication work?
* Beginner's guide to REST
* REST and RESTful responses

# Bare Minimum Requirements

Build a simple session-based server-side authentication system - from scratch. Use the tests in test/ServerSpec.js and the following outline to guide you in your implementation.

**Usernames and passwords**

The database table, users, and it's corresponding model have been provided. Use this to store usernames and passwords. The model includes useful methods for encrypting and comparing your passwords.

* Add routes to your Express server to process incoming POST requests. These routes should enable a user to register for a new account and for users to log in to your application. Take a look at the login.ejs and signup.ejs templates in the views directory to determine which routes you need to add.
* Add the appropriate callback functions to your new routes. Add methods to your user model, as necessary, to keep your code modular (i.e., your database model methods should not receive as arguments or otherwise have access to the request or response objects).

**Sessions and cookies**

You will be writing two custom middleware functions to configure your Express server. The first will be a cookie parser and the second will be a session generator. Though Express has its own middleware packages named cookie-parser and express-session, you will be building this functionality from scratch (read as: you should not be using the cookie-parser and express-session middleware packages anywhere in your code). The database table, sessions, has been provided as a place to store your generated session hashes.

* In middleware/cookieParser.js, write a middleware function that will access the cookies on an incoming request, parse them into an object, and assign this object to a cookies property on the request.
* In middleware/auth.js, write a createSession middleware function that accesses the parsed cookies on the request, looks up the user data related to that session, and assigns an object to a session property on the request that contains relevant user information. (Ask yourself: what information about the user would you want to keep in this session object?)

Things to keep in mind:

* An incoming request with no cookies should generate a session with a unique hash and store it the sessions database. The middleware function should use this unique hash to set a cookie in the response headers. (Ask yourself: How do I set cookies using Express?).
* If an incoming request has a cookie, the middleware should verify that the cookie is valid (i.e., it is a session that is stored in your database).
* If an incoming cookie is not valid, what do you think you should do with that session and cookie?
* Mount these two middleware functions in app.js so that they are executed for all requests received by your server.

**Authenticated Routes**

* Add a verifySession helper function to all server routes that require login, redirect the user to a login page as needed. Require users to log in to see shortened links and create new ones. Do NOT require the user to login when using a previously shortened link.
* Give the user a way to log out. What will this need to do to the server session and the cookie saved to the client's browser?

**Tests**

Write at least 3 new meaningful tests inside of *test/ServerSpec.js*

# Advanced Content

Our advanced content is intended to throw you in over your head, requiring you to solve problems with very little support or oversight, much like you would as a mid or senior level engineer.

* Add error messages:
  * Let your users know when they've entered incorrect credentials or fail to properly register for a user account.
* Convert your cookies to signed cookies.
* Now that you fully understand how to roll your own server-side session-based auth system, swap out the system you built for Passport.
  * Use an [OAuth](https://en.wikipedia.org/wiki/OAuth) provider strategy; login via your GitHub account.
  * NOTE: Passport will conflict with any client-side auth system you've aleady implemented, so be ready to disable it.

# Nightmare Mode

* Build a front-end authentication system:
  * Use an OAuth provider strategy; login via your GitHub account.
  * Handle all login and signup from backbone instead of relying on server-side redirects and routes.
  * Convert your server routes into API endpoints. You will need to deliver meaningful errors on the server side, and handle them gracefully on the client side.

**Other Challenges**

The following challenges are not core to the sprint but if you have time you can give them a try:

* Add a (Backbone) router to move the user from page to page:
  * Using HTML5 pushstate, keep the URL in the address bar in sync with what page the user is viewing.
  * A user should be able to copy a url from the address bar, and then re-enter it into a browser to get back to the original page. Ensure you have a reliable strategy for handling [deep-linked](https://en.wikipedia.org/wiki/Deep_linking) routes on the server.
* Make your site prettier:
  * Find an image used on the site of the original url and use that instead of the generic icon (hint: use a regular expression or a [parser](https://stackoverflow.com/questions/7977945/html-parser-on-node-js) to analyze the HTML document). How will you store this new information?
* Create a basic stats page for each link:
  * Show clicks by time grouped into 5 min intervals (displayed as a table is ok).
  * Add additional models, routes, views, templates as needed.
* Add user-specific stats:
  * Modify the data schema to support different codes for the same url - so each user can have their own stats for a given url.
  * Change the stats page so it displays the user's clicks along with a total of all clicks in the database for the same url.
* Don't use two different templating systems:
  * Switch to using Handlebars as your template engine on both client and server.
  * Write a script that precompiles your templates on the server and loads them in the client using a single script tag.
  * Stop using EJS entirely.

**Reference Material**
* [Handlebars](http://handlebarsjs.com/)
* [HTML5 Pushstate](http://badassjs.com/post/840846392/locationhash-is-dead-long-live-html5-pushstate)
* [Backbone Router](https://backbonejs.org/#Router)
