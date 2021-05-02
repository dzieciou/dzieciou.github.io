---
layout: post
title: "Isolating bugs with REST services"
date: 2016-06-13 18:00:00 -0000
categories: testing java
---


[REST-assured](https://github.com/rest-assured/rest-assured) is a popular Java library that makes testing REST services easier. For the last few months we have used it to automate tests for a complex authentication flow. Large part of tests consisted of multiple REST calls, checking system behaviour in different states. Finding bugs was one challenge but reproducing them was another. Particularly, we struggled with:

* **Bug isolation.** You’re trying to isolate a bug you found in a long test with REST-assured but you don’t want to replay whole scenario over and over. You just want to replay one step and see a HTTP response.
* **Pair debugging.** Together with a dev you’re debugging a bug. She’s putting breakpoints in her code and asking you to replay your REST-assured test over and over because she does not have downloaded it yet.
* **Bug reporting.** A bug you have found must be reported in a bug tracking system and you need to quickly describe steps to reproduce it. While for UI it is often fairly easy to list clicks and choices, for a backend system describing API calls is harder.

All those problems can be addressed with [cURL](https://curl.haxx.se/). cURL is a popular command line tool available for many platforms and with many \*nix distributions it comes pre-installed, so a dev can easily reproduce an issue. Initially, I was crafting those curl commands manually but this scaled poorly. Other platforms for testing offer automatic generation of curl commands from HTTP requests ([Chrome Developer Tools](https://coderwall.com/p/-fdgoq/chrome-developer-tools-adds-copy-as-curl), [Postman add-on for Chrome](https://www.getpostman.com/docs/creating_curl), [Firebug add-on for Firefox](http://www.softwareishard.com/blog/planet-mozilla/firebug-tip-resend-http-request/), [Ok2Http client](https://github.com/mrmike/Ok2Curl)), but REST-assured was missing that.

So I’ve created a small library, [curl-logger](https://github.com/dzieciou/curl-logger).

# How does it work?
Imagine you’re trying to send HTTP GET request to google.com with REST-assured:

```java
given()
    .config(curlLoggingConfiguration())
    .redirects().follow(false)
.when()
    .get("http://google.com")
.then()
    .statusCode(302);
```

curl-logger will log it as the following curl command:

```bash
curl 'http://google.com/' -H 'Accept: */*' -H 'Content-Length: 0' -H 'Host: google.com' -H 'Connection: Keep-Alive' -H 'User-Agent: Apache-HttpClient/4.5.1 (Java/1.8.0_45)' –compressed –insecure –verbose
```

You will find more details how to setup a library on the [project page](https://github.com/dzieciou/curl-logger).

# Waiting for your feedback

The library generates curl commands from an actual request rather then only request specification you wrote, so if a HTTP client in your test accepted server cookies, they will be included by curl-logger as well. Currently, the library supports multiple HTTP methods, multipart/form and multipart/mixed content types and handles file attachments.

If you find it useful, encounter a bug or miss a feature, let me know.
