+++
date = '2026-02-22T14:00:00+01:00'
draft = false
title = 'Data Integration - External APIs'
tags = ["java", "api", "integration", "http"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Data Integration - Working with External APIs

This week was my first real introduction to integrating external data into a backend system, and honestly, it was a bit of a mindset shift.

Up until this point, everything I had built was self-contained:
- my own database
- my own entities
- my own logic

But now, suddenly, my application had to **rely on someone else's system**.

## From isolated systems to connected systems

The biggest realization for me this week was:

> A backend is not just something that serves data, it also consumes data.

Instead of only working with my own PostgreSQL database, I now had to:
- send HTTP requests
- receive JSON responses
- map external data into my own system

This made the backend feel much more “real-world”.

## Using the RandomUser API

To get hands-on experience, I integrated the **RandomUser API**, which generates fake user data.

Example request:

**https://randomuser.me/api/**

This returns a JSON response with user information like:
- name
- email
- dob
- password hashes or plain text passwords

Instead of manually creating test users, I could now fetch realistic data automatically.

## Mapping external data into my system

One of the first design decisions I had to make was:

> Should I use the API’s data structure directly?

I chose **not to**.

Instead, I created DTOs to match the API response, and then mapped that data into my own domain model.

For example:
- API > RandomUserDTO
- Internal > User entity

This separation was important because:
- I don’t control the external API
- the API structure could change
- my domain model should remain stable

## Connecting it to my project

Even though I restarted my current membersystem project later, I initially used this integration in an earlier version of the system.

The idea was to:
- quickly generate users for testing
- simulate realistic data
- avoid manually inserting test records

This was especially useful when testing relationships like:
- users belonging to companies
- memberships and roles

Even though I didn’t carry this feature into the final version, it gave me a much better understanding of how external data can support development and testing.

## Handling JSON in Java

Working with JSON responses was a new challenge.

I had to:
- understand nested JSON structures
- create matching DTO classes
- deserialize responses using Jackson

This made it clear how important structure is when working with external data.

Small mismatches in field names or structure could easily break everything.

## Error handling and uncertainty

One thing that stood out immediately was how unreliable external systems can be.

Compared to my own database, external APIs can:
- be slow
- return unexpected data
- go down completely

This forced me to think about:
- null checks
- fallback behavior
- defensive programming

A key takeaway here was:
> Never assume external data is correct or even available.

## Design decisions

To keep things clean, I made sure to:
- isolate API calls in a dedicated service layer
- avoid mixing HTTP logic into controllers
- keep the rest of the system unaware of where the data came from

This separation made the system easier to understand and extend.

## Challenges

Some of the challenges I ran into:
- understanding the structure of real API responses
- mapping nested JSON correctly
- dealing with missing or null values
- debugging HTTP calls when things didn’t work

It was definitely more “messy” than working with my own controlled data.

## Key takeaways

This week taught me that:
- backend systems often depend on external data sources
- DTOs are essential when working with APIs
- separation between external and internal models is critical
- error handling becomes much more important

Even though this was a relatively small feature, it changed how I think about backend systems.

They are not just isolated applications, they are part of a larger ecosystem.