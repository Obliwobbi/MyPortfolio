+++
date = '2026-03-01T14:00:00+01:00'
draft = false
title = 'REST API Introduction - From Database to Endpoints'
tags = ["java", "javalin", "rest", "http", "api"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# REST API Introduction - From Database to Endpoints

This week was where the project started to feel like an actual application instead of “just” a backend connected to a database.

Up until this point, I had mostly been working with:
- entities
- DAOs
- JPA
- database relations

But this week, I had to expose that functionality through a **REST API** using **Javalin**.

That meant turning internal Java logic into something that could be accessed through HTTP requests and JSON responses.

## Understanding what a REST API really is

Before this week, I had of course heard the words:
- API
- REST
- endpoints
- HTTP methods

But understanding it conceptually is not the same as actually building it.

What clicked for me this week was this simple way of thinking:

- **resource** = the thing I am working with  
- **HTTP method** = what I want to do with it  

So for example:
- `GET /users` > get all users  
- `GET /users/1` > get one user  
- `POST /users` > create a new user  
- `PUT /users/1` > update user 1  
- `DELETE /users/1` > delete user 1  

That distinction made REST much easier to understand.

## From Java methods to HTTP endpoints

One of the biggest transitions this week was learning how to connect my backend logic to web routes.

In Javalin, that meant defining endpoints such as:

```java
app.get("/users/{id}", ctx -> {
    Long id = Long.parseLong(ctx.pathParam("id"));
    User user = userDAO.getByIdWithCompany(id);
    ctx.json(user);
});
````

This was a big step, because now the application could respond to actual requests from outside the codebase.

Instead of just calling methods internally, I was now building an interface that a frontend, Postman, or another client could use.

## Working with the Javalin context object

A major learning point this week was the **Context object** in Javalin.

At first it felt a bit abstract, but it quickly became clear that `ctx` is basically how the route talks to the request and response.

With `ctx`, I could:

* read path parameters
* parse request bodies
* return JSON
* set status codes

This made it much easier to understand how a backend handles incoming requests in practice.

## Why DTOs became even more important

When I first started returning data from endpoints, I ran into issues with returning entities directly.

That led to a very important lesson:

> Just because an entity exists in the backend does not mean it should be exposed directly through the API.

Instead, I used DTOs to control exactly what the client sees.

This helped with:

* cleaner JSON responses
* avoiding exposure of internal fields
* preventing lazy loading problems
* separating persistence models from API models

That decision ended up being one of the most important structural improvements in the project.

## Building real endpoints in the membersystem

This week was where the membersystem started becoming a real API.

I focused on a smaller part of the domain first:

* Company
* User

That was a deliberate choice.

Instead of trying to expose the entire system at once, I chose to build a complete flow around only a few entities first.

That included:

* creating endpoints
* handling JSON request bodies
* returning response DTOs
* using proper status codes

Starting small made it much easier to understand what was going on.

## Path parameters and dynamic routes

Another thing that became much more concrete this week was **path parameters**.

Before building it myself, a route like this:

```
/users/{id}
```

looked simple enough, but it became much more meaningful when I actually had to parse it and use it.

For example:

```java
Long id = Long.parseLong(ctx.pathParam("id"));
```

This was one of those small things that suddenly made the whole request flow feel logical.

The client sends an ID, the backend extracts it, uses it in the DAO layer, and returns the matching resource.

## Learning HTTP methods through CRUD

This week also helped me connect REST to something more familiar: CRUD.

The mapping became very natural:

* `POST` > Create
* `GET` > Read
* `PUT` > Update
* `DELETE` > Delete

That made REST much less intimidating.

Instead of feeling like a new theory-heavy topic, it became a practical way to expose what I had already been building in the persistence layer.

## Status codes started to matter

Before this week, status codes were something I mostly recognized from browsing the web.

Now they became part of my own application design.

I had to think about when to return:

* `200 OK`
* `201 Created`
* `204 No Content`
* `404 Not Found`
* `500 Internal Server Error`

This forced me to think more carefully about how an API should communicate success and failure.

That was actually one of the more satisfying parts of the week, making the backend feel more professional.

## Challenges

The main challenges I ran into were:

* understanding the difference between entities and response models
* knowing where logic belongs
* handling lazy loading issues when returning data
* keeping the routes readable
* fully understanding the difference between HTTP methods and resources

At first, it was easy to think too much in “Java methods” and not enough in “REST resources”.

But once that clicked, the structure became much more natural.

## Key takeaways

This week taught me that:

* a REST API is about exposing resources in a structured way
* Javalin is simple, but still powerful enough to show the full request-response cycle
* the Context object is central to request handling
* DTOs are crucial when exposing backend data
* HTTP methods and status codes are part of the design, not just technical details

This was the week where the backend stopped being just a database project and started becoming a real application.