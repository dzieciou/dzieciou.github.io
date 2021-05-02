---
layout: post
title: "Replacing test frameworks with test libraries"
date: 2018-09-05 18:00:00 -0000
categories: testing architecture
---


Test frameworks provide *scaffolding* for building automated tests: domain-specific vocabulary to describe your business scenarios, loggers to generate test results in a standardized format, or "glue" to talk to various services. Such already implemented elements of the framework initially speed up scripting automated tests.

One test framework may work fine for multiple teams testing similar products and working in a similar way. However, in big organizations, different teams develop and test significantly different things and work in their own ways. In my current company development culture is no different from that. In such cases, teams feel tempted to add more and more functionality to the already bloated framework, which results in an anti-pattern called [*Frankenstein's Framework*](http://thecodist.com/article/frameworks_and_frankenstein) or [*Wunder Framework*](https://www.youtube.com/watch?v=PeioFobaq94). Such frameworks illustrate needless complexity and immobility *-- *they become hard to use and maintain.

I have learned that the hard way. However, I did not want to "throw the baby out with the bathwater". I found routines common for different testing teams, but each team was still automating the same routines from scratch. For instance:

-   In many teams test scripts start from authenticating to our flagship product -- the authentication routine automated by one team can be shared via an authentication *test library* and reused by others.
-   Applications use common set of UI components provided by the UX team, e.g., tables, and testers from different teams repeat routines to access those components, e.g., sort and read values from certain column and row of the table -- Selenium/WebDriver wrappers for the UI components can be shared as a UI *test library*.
-   Communication between microservices in a distributed system is naturally sensitive to network errors and temporal outage of microservices; [request retry is a common pattern](https://medium.com/@butaji/practical-microservices-retry-circuit-breaker-and-compensation-transaction-3013943db0e1) to handle such issues; a *test library* that implements this pattern for a popular test library like REST-assured can be shared with all the teams performing REST service testing.
-   Different teams take different approach to handle tests failing because of an already known bug -- some prefer to include results of such tests in a report, others prefer to exclude them; a* test library* that enables running or running, depending on the configuration, of such tests was found useful by both types of teams.
-   Behavior of the system depends on user privileges, group membership and other properties of a user; a *test library* that finds or creates test users of certain properties can be helpful to many teams.

Teams can use one or more libraries, but it is up to each team which ones to pick up.

Do One Thing and Do It Well
===========================

A test framework does multiple things such as generating test data, mocking external systems, logging debug information, and handling interaction with Web page UI. A test library should do only one of those things and not the others. For instance, a mocking library would support mocking system X.

The concept of tools doing only one thing comes from the [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well) on how to build software. It has been present in the software engineering industry for more than 40 years and stands in opposition to building systems as monoliths.  Its benefits have been [widely discussed](https://techcrunch.com/2009/08/21/do-one-thing-and-do-it-well-40-years-of-) but only now, as I'm writing this post, they finally seem obvious to me. It has taken me much time to understand why moving from monolith frameworks to test libraries is worth the effort.

With a large system, it's hard to find one single person that knows, on his own, how all of the pieces such as authentication, UI navigation, report generation, and database population work together. A similar problem occurs with frameworks for testing such systems, it's nigh impossible for any one person to have the breadth and depth of experience required to understand how all parts of the framework work. Splitting code from the framework into common libraries lets testers *specialize in their particular strengths*. For instance, my team had enough expertise in authentication layer of our flagship product to develop an authentication library for testing purposes. Another team focused on building a library for testing UI reports related to traffic billing, their domain of expertise. Each team has focused on one thing and did it well!

With a library that does only one thing, *fixing bugs and adding new features is easier*. Rather than working through a complex monolithic test framework and worrying about complex regression testing, a maintainer of the library can focus on a single small set of functionalities. I once contributed to the test framework with a feature I needed in my tests. It took significant time to release its new version with my changes. Making sure changes didn't impact users of the framework took days and thus we released new versions infrequently. With a library, the process of *development and releasing is much faster*.

Another benefit of libraries is the *possibility to compose functionalities together*. Imagine, you would like to send an HTTP request to a protected resource requiring authentication. The request fails with an error and your colleagues want to reproduce the problem with curl command line tool. The whole functionality can be achieved by composing three different libraries: REST-assured (for sending HTTP requests), internal authentication library (for authentication and signing HTTP requests with session tokens) and curl logger (for printing curl commands).

// Authentication library
Session session = new RestAssuredAuthClient(baseUri).authenticate(user, password);

RestAssured
  .given()
  // CURL library
  .config(CurlLoggingRestAssuredConfigFactory.createConfig())
  // Authentication library
  .filter(new RestAssuredSigningFilter(session))
  .formParam("startDate", "2018-09-05")
  .post("/results")
  .then()
  .code(201)

Identifying areas for test libraries
====================================

If a library should do only one thing, then you probably ask yourselves what could be these single things in test automation. Here are a few ideas that come to my mind:

-   *generating test data* as randomized email messages or finding test users in the system,
-   *simulating sub-systems* not available for testing, for instance, a library mocking external Single Sign-On providers like Google Sign-In,
-   *simulating a user* interacting through GUI or simulating a client application communicating via API, for instance, a library authenticating a user through UI or a library booking a flight through SOAP Web service,
-   *generating diagnostic information* to help troubleshooting failing tests, for example, a library that prints HTTP requests submitted with HTTP client as curl commands,
-   *detecting certain kinds of error condition*s in a product, for example, a library with assertions checking a state of a user in a database,
-   *recording and replaying certain events*, for example, a library recording HTTP traffic from production and replaying it in a test environment.

Note that test libraries can be specific to your product or can have a wider audience. For instance, Selenium library provides a base for interacting with any Web UI, while the Luna Portal reporting library, built on the top of Selenium, provides routines for interacting with UI of our specific system.

How to find potential areas for a testing library in your *current* project? I have learned that this is an organic process. Usually, when the project starts I do not have enough knowledge about the domain and the internals of the system. Automated scripts for testing grow slowly and are frequently refactored as my initial assumptions about the system and the domain often turn out to be wrong.

It is not a bad thing if both test scripts and routines used by those scripts live in the same repository -- it eases frequent refactoring. I try to follow the *rule of three**, *that is when the same code is used three times or more, I extract it into a separate procedure or a helper class. It takes time to understand whether those classes are stable enough to be factored into a separate test library.

Sharing libraries
=================

We have built a number of such libraries in one of our teams responsible for building the authentication layer. The initial goal was to ease scripting regression tests for authentication, but ultimately the libraries proved useful for other teams. Two have been open sourced and are used by both customers building applications on top of the company infrastructure and by the online community testing REST services unrelated to my current employer.

Acknowledgements
================

The whole endevour with introducing test libraries was a joint of effort of my team at Akamai. Many thanks to, in no particular order: Anatoly Maiegov, Mariusz Jędraczka, Krzysztof Głowiński, Martin Meyer, Antonio di Maio, Bartłomiej Szczepanik, Patrizio Rullo, and Chema del Barco.
