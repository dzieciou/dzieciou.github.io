---
layout: post
title: "Anti-patterns in test automation"
date: 2016-05-23 18:00:00 -0000
categories: testing 
---



As discussed by [Mike Wacker from Google](http://googletesting.blogspot.com/2015/04/just-say-no-to-more-end-to-end-tests.html), end-to-end tests are known to be flaky, run for a long time and when they fail, it is hard to isolate failure root cause. A part of those problems stems from the anti-patterns appearing in such tests and addressing those anti-patterns may make your tests more reliable, more useful in isolating root causes and cheaper to maintain.

The following list of 8 anti-patterns comes from my test automation experience. Some I found in legacy test suites me and my teams inherited. Other were committed by candidates I interviewed for testing positions. Selected ones comes from our fellow developers who helped us in test automation.

### Hardcoded test data (#1)

That usually happens when you start small and think small, without remote perspective in mind. Let's imagine you're testing authentication in your system with a sample user:

```java
String testUser = "mgawinecki@tokyo.jp";
```

When coming back to the test code a month later you might ask yourself, why you wanted to test with this particular user? Is it because you wanted to test for a user that is inactive? Or maybe for a Japanese-speaking user? Hard to guess. And when the test suite grows up to several hundred test cases, maintaning hardcoded test data becomes a nightmare. How do you handle it? Similar way you handle [magic number anti-pattern](https://en.wikipedia.org/wiki/Magic_number_(programming)).

### Hardcoded environment configuration (#2)

Imagine same checks must be run against both Firefox and Chrome; or against local and then pre-production test environments. No way to do it if you hardcoded references to browser type, server host or databases. A solution is to make your tests environment-agnostic and provide configuration to the test at runtime, e.g., by reading it from configuration file. Additionally, updating configuration will not require modifying multiple files.

### Taking environment state for granted  (#3)

Taking environment state for granted is often over optimistic. Unlike in unit tests, end-to-end setup gives little control over test environment state, particularly when it is shared with other teams. When the test starts failing it might be because of a new bug introduced or because environment is not in a state your test needs. For instance, a user you use for tests has been locked out by another team or flight schedule has changed and no longer you can use a connection from London to Los Angeles in your tests. There are a number of ways to handle such issues:

-   setting system environment in a certain state, e.g., creating a test user,
-   finding test data matching a specification in existing environment, e.g., a roundtrip flight,
-   checking environment is in required state and skipping a test if it is not.

Each of those solutions can be done manually before each test run but in case of large number of tests and dynamic environment it simply does not scale well. An alternative is to automate one of those solutions. The last one is usually the easiest to implement and saves execution time. It does not garranty test data for your test but it will skip (not fail!) the test immediately when it is clear it will provide no useful feedback. JUnit's [assumeThat](http://junit.sourceforge.net/javadoc/org/junit/Assume.html) construction can be intuitive here:

```java
assumeThat(testUser, existsInSystem());
```

### Conditional assertions (#4)

Some people are aware that environment state may change, so they try to make their test verifying different things depending on the environment state:

```java
if (existsInSystem(testUser)) {
    // test for existing user
    ...
} else {
    // test for not existing user
    ...
}
```

However, this is a shortsighted workaround as it makes your test non-deterministic: you will never be sure which path will be verified in the next pass. In extreme case, if the environment is always in the same state, only one execution path will be tested. In general, there's no reason to have one test method if you're testing two different outcomes.

### Conspiracy of silence   (#5)

When assertions in tests are failing with almost no clue why

```
Expected: true
Actual: false
```

it is hard to isolate a root case of a failure. This happens when using simple assertions like `assertTrue` or `assertEqual`. A better solution is to use [custom matchers](http://stackoverflow.com/questions/1701113/why-should-i-use-hamcrest-matcher-and-assertthat-instead-of-traditional-assert) in combination with custom messages:

```java
assertThat("Account with debit is missing", accounts, contains(expectedAccountWithDebit));\
```

### No traces left (#6)

Once a test fails you will need to understand what has happenned before. However, if you don't want to know that, follow this anti-pattern:

-   Leave no traces of what your test has done.
-   Don't report intermediary states of the system in the middle of your test execution.
-   No screenshots, no photos, no paparazzi, particularly when you use headless Web browsers like PhantomJS. It must remain headless, right!
-   No HTTP traffic recorded, no HARs, no curls to reproduce traffic, etc.

Jokes apart, the goal of addressing this issue is to ease reproducing the problem you found with the smallest cost. Running same test again and debugging the test and the system under test again and again is usually expensive and can be ineffective for intermittent bugs.

### Tests cluttered with business logic (#7)

Tests that mix details of system business logic with steps of test scenario are hard to read and maintain. A solution is to separate what the test is testing from how it is doing. In software development this separation of concerns is known as encapsulation. I found a number of ways to do it in test automation:

-   [Page Object pattern](http://stackoverflow.com/questions/18094238/what-is-the-page-object-pattern-in-selenium-webdriver), hides Web page details from a tester,
-   domain-specific assertions and matchers, hides technical details of a check from a tester, e.g., `UserExistsMatcher`,
-   Domain Specific Language (DSL), describes test steps in a language of an end-user.

I found a good introduction to the latter two approaches in the article [Writing Clean Tests -- Replace Assertions with a Domain-Specific Language](http://www.petrikainulainen.net/programming/testing/writing-clean-tests-replace-assertions-with-a-domain-specific-language/).

### Sleeping for arbitrary amount of time (#8)

Waiting in your test 4 seconds

```java
Thread.sleep(4*1000);
```

because your production system usually takes 4 seconds to go out over the network, take some data, and come back over the network with the result --- this is baaad. That's bad because your test becomes fragile to network congestion: it will start failing when network latency increases. What you actually intented is to wait until "a response is returned" or "an object appears", using [explicit and implicit waits](http://stackoverflow.com/questions/10404160/when-to-use-explicit-wait-vs-implicit-wait-in-selenium-webdriver) (in Selenium) or active pulling in general (e.g., with [Awaility](https://github.com/jayway/awaitility) library).

Obviously, there are exceptions where [explicit sleeping is fine](http://sqa.stackexchange.com/questions/18097/does-selenium-ever-need-thread-sleep).

### Take-away message

Presented anti-patterns demonstrate that writing system tests is a slightly different beast than writing unit tests. Sure, some anti-patterns like a [Wet Floor](http://stackoverflow.com/questions/333682/unit-testing-anti-patterns-catalogue) can happen both here and there. However, in this post I have focused on anti-patterns specific for end-to-end tests. I have no funny names for those an yet, so if you come up with any let me know.

I'm waiting for your feedback! Do you agree or dis-agree with some anti-patterns? Or maybe you encountered some others?
