+++
date = '2026-03-15T14:00:00+01:00'
draft = false
title = 'Security - Protecting the Backend'
tags = ["java", "security", "jwt", "authentication", "backend"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Security - Protecting the Backend

This week focused on something that is easy to overlook early in development, but absolutely critical in real systems: **security**.

While the previous week introduced authentication and testing, this week was about understanding how to properly protect the backend.

## From functionality to responsibility

Up until this point, my focus had mostly been:
- making endpoints work
- returning correct data
- structuring the backend properly

But this week introduced a different perspective:

> Just because something works does not mean it is safe.

This changed how I approached the system.

## Authentication with JWT

Authentication in the system is implemented using **JWT (JSON Web Tokens)**.

The flow works as follows:
1. A user logs in with email and password
2. The backend validates the credentials
3. A JWT token is generated
4. The client includes the token in future requests

This allows the backend to:
- remain stateless
- avoid storing session data
- validate requests efficiently

Working with JWT made the request flow much more structured and realistic.

## Password security

Passwords are never stored in plain text.

Instead, they are hashed before being stored in the database using a password service.

This ensures that:
- passwords are not exposed in case of a database leak
- sensitive user data is protected

Implementing hashing was one of the most important improvements to the system.

## Protecting endpoints

With authentication in place, certain endpoints now require a valid token.

This means:
- users must log in before accessing protected resources
- requests without a valid token are rejected

This introduced a clear separation between:
- public endpoints (such as login)
- protected endpoints (such as accessing data)

## Environment variables and secrets

Another important part of security was handling sensitive configuration.

Instead of hardcoding values, the system uses environment variables for:
- database credentials
- JWT secret keys
- configuration values

This became especially important when deploying the application using Docker and CI/CD.

## Real-world issues during development

This week included several practical challenges:

- login failing due to password mismatches
- configuration not loading correctly
- environment variables missing in CI/CD
- differences between local and Docker environments

These issues made it clear how fragile security-related features can be if not handled carefully.

## Thinking about system boundaries

One important shift this week was starting to think about how the system behaves from the outside.

Questions I began asking:
- What happens if a request has no token?
- What if the token is invalid?
- What data is exposed through the API?

Even simple endpoints required more thought when viewed from a security perspective.

## Security is an ongoing process

One of the biggest takeaways this week was realizing that security is not something you “finish”.

It is something that must be:
- considered during development
- tested continuously
- improved over time

The current system includes:
- JWT-based authentication
- password hashing
- protected endpoints

But security can always be improved further as the system evolves.

## Key takeaways

This week taught me that:
- security is about protecting both data and access
- authentication is a core part of backend design
- passwords must always be stored securely
- environment variables are essential for handling secrets
- even small systems require careful consideration of security

This was the week where I started thinking about the backend not just as functionality,  
but as something that needs to be **protected and trusted**.