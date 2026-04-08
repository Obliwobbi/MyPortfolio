+++
date = '2026-03-08T14:00:00+01:00'
draft = false
title = 'REST and Test - From Endpoints to Confidence'
tags = ["java", "rest", "testing", "jwt", "security"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# REST and Test - From Endpoints to Confidence

This week was where things started to get more serious.

Up until now, I had built endpoints that *worked*, but I had no real guarantee that they would continue working as the system evolved.

This week introduced:
- automated testing
- authentication
- and a much deeper understanding of how a backend should behave

## From “it works” to “it is reliable”

Before this week, my workflow was very simple:
- run the app
- test endpoints manually (often with Postman)
- fix bugs if something broke

That approach works early on, but it doesn’t scale.

This week introduced the idea that:

> If you don’t test your backend automatically, you don’t actually know if it works.

## Testing REST endpoints with Rest Assured

To test the API, I used **Rest Assured**, which allows HTTP requests to be written directly in Java tests.

Example:

```java
given()
    .contentType("application/json")
.when()
    .get("/api/v1/companies")
.then()
    .statusCode(200);
````

This made it possible to:

* test endpoints automatically
* verify status codes
* validate JSON responses

It also made the tests feel much closer to how a real client interacts with the API.

## Setting up a test environment

One of the biggest challenges was setting up a proper test environment.

The application needed:

* a database connection
* configuration values
* a running application context

This led to several issues, especially when running tests in GitHub Actions.

At one point, my tests failed with:

```
NullPointerException: inStream parameter is null
```

This turned out to be caused by missing configuration (config.properties / environment variables).

This was a key lesson:

> Tests should not depend on local configuration files.

Instead, I moved towards using environment variables and test-specific configurations.

## Authentication with JWT

This week also introduced authentication using **JWT (JSON Web Tokens)**.

The flow works as follows:

1. A user logs in with email and password
2. The backend validates credentials
3. A JWT token is generated
4. The client includes the token in future requests

This was implemented in a dedicated service.

Example responsibilities:

* generating tokens
* validating tokens
* extracting user information

## Password hashing

Another critical part of authentication was password security.

Instead of storing plain text passwords, I implemented hashing using a password service.

This ensures that:

* passwords are never stored in plain text
* even if the database is compromised, passwords are not directly exposed

This was one of the most important security improvements in the system.

## Protecting endpoints

With JWT in place, I started protecting endpoints.

This meant:

* checking for a valid token
* rejecting unauthorized requests

It introduced a new layer of thinking:

> Not all endpoints should be accessible to everyone.

For example:

* public endpoints (login)
* protected endpoints (user management)
* role-based access (admin vs user)

## Testing authenticated endpoints

Testing became more complex once authentication was introduced.

Now tests had to:

1. log in
2. extract the token
3. include the token in requests

Example flow:

```java
String token =
    given()
        .contentType("application/json")
        .body(loginRequest)
    .when()
        .post("/api/v1/login")
    .then()
        .extract()
        .path("token");

given()
    .header("Authorization", "Bearer " + token)
.when()
    .get("/api/v1/companies")
.then()
    .statusCode(200);
```

This made the tests much closer to real-world usage.

## Real-world debugging (and frustration)

This week definitely came with its fair share of frustration.

Some of the issues I ran into:

* tests failing in CI but working locally
* missing environment variables
* incorrect JWT setup
* password mismatches
* configuration not loading inside Docker

One specific issue that took time to debug was that:

* the application worked locally
* but failed in GitHub Actions due to missing config values

This forced me to rethink how configuration is handled.

## Connecting testing with CI/CD

This was also the point where testing became part of the deployment pipeline.

In GitHub Actions:

* tests run automatically on push
* the build fails if tests fail

This ensures that:

> broken code never gets deployed

That was a huge step towards making the project production-ready.

## Key takeaways

This week taught me that:

* testing is not optional in real systems
* REST APIs should be validated automatically
* authentication adds complexity, but also structure
* environment configuration must be handled carefully
* CI/CD pipelines rely heavily on reliable tests

This was the week where the backend stopped being something I *manually tested*
and became something I could actually **trust**.