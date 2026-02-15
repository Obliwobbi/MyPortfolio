+++
date = '2026-02-15T14:14:59+01:00'
draft = false
title = 'How we are doing now - week 2'
tags = ["java", "ORM", "JPQL", "database"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Designing the Backend Domain - JPA, Relations, and Queries

This phase of the project has focused entirely on building a solid backend foundation using **Java, JPA, Hibernate, and PostgreSQL**.

The goal was not to build features quickly, but to design a domain model and persistence layer that:
- reflects real-world relationships
- is easy to reason about
- can be extended safely as new technologies are introduced later in the semester

## Domain-driven database design with JPA

Instead of starting with SQL tables, I designed the system around **domain concepts**, letting JPA handle table creation and relationships.

The core entities in the system are:
- Company
- User
- Membership
- MembershipType
- Location
- CheckIn

Each entity maps directly to a database table using JPA annotations.

This approach helped me think in **objects and relationships**, rather than rows and columns.

## Primary keys and identity

All entities use surrogate primary keys:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

Using generated IDs simplifies relationships and avoids coupling the domain model to business-specific identifiers.

## Foreign keys through object references

Instead of storing foreign key IDs manually, relationships are represented using object references.

For example, each user belongs to a company:

```java
@ManyToOne(optional = false, fetch = FetchType.LAZY)
@JoinColumn(name = "company_id", nullable = false)
private Company company;
```

This creates a foreign key in the database (company_id) while allowing the Java code to work with objects rather than IDs.

Hibernate handles the translation automatically.

This was a key learning point: well-designed object relationships drastically reduce boilerplate code.

## Separating membership data from users

Rather than placing membership-related fields directly on the User entity, membership data is modeled as its own entity.

A Membership represents a time-bound relationship between:
- a User
- a MembershipType

It also includes:
- startDate
- endDate
- a MembershipStatus enum

This design allows:
- membership history
- cancellations and pauses
- reactivation without data loss

Separating this logic early prevents destructive updates and keeps the model flexible.

## Enums and database safety

Enums such as Role and MembershipStatus are stored using:

```java
@Enumerated(EnumType.STRING)
```

Storing enums as strings:
- improves database readability
- prevents bugs caused by enum reordering
- makes debugging and querying easier

## DAO structure and responsibility

Each entity has a corresponding DAO implementing a shared IDAO<T> interface, providing basic CRUD functionality.

More complex queries are implemented as specific methods in the DAO where they logically belong.

For example:
- UserDAO handles user-related queries
- MembershipDAO handles membership-related business logic

Even when a membership-related query returns User objects, it still belongs in MembershipDAO, because the definition of “active” depends on membership state.

This separation keeps responsibilities clear and avoids bloated DAO classes.

## JPQL and joins

JPQL was used instead of raw SQL to query the database.

JPQL operates on:
- entities
- fields
- relationships

Rather than tables and columns.
Example: finding active members of a company

```SQL
SELECT DISTINCT u
FROM Membership m
JOIN m.user u
WHERE u.company.id = :companyId
  AND m.status = :status
  ```

This query:
- joins Membership and User
- filters by company
- filters by membership status
- returns users directly

The use of DISTINCT ensures that users are not duplicated if multiple records exist.

## Managing entity state with JPA

When creating or updating entities, related objects may be detached from the persistence context.

To handle this safely, related entities are merged explicitly:

```java
Company managedCompany = em.merge(user.getCompany());
user.setCompany(managedCompany);
```

This ensures Hibernate works with managed entities and avoids persistence errors.

Understanding managed vs detached entities was one of the more important lessons in this phase.

## Manual integration testing via Main

Before introducing automated tests, a Main class is used to manually test the persistence layer.

This setup:
- creates realistic test data
- verifies entity mappings
- tests updates and deletes
- validates JPQL queries

While not a replacement for proper testing, this approach provided valuable insight into how Hibernate behaves at runtime.

## Key takeaways

Some of the most important lessons from this phase:
- JPA relationships are easier to reason about than manual foreign keys
- A clean domain model simplifies queries and future features
- JPQL encourages domain-oriented thinking
- Correct planning of relations pays off immediately

The current state of the backend provides a stable foundation for introducing service layers, validation, REST endpoints, and security in the next stages of the project.