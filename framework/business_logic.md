---
layout: default
title: Business Logic
nav_order: 9
parent: Framework
---

### What is Business Logic

Business Logic is collection of classes to deal with application purposes.
Actually, Business Logic is what "M" letter in "MVC" abbreviation means.
But don't be confused with terms, Business Logic is not ORM Model.
Generally, ORM Model is a subject to manage with Business Logic.

### Business Logic in Framework

There are 4 types of classes: Repository, Domain, Facade and Service.

##### Repositories

Usually are located at `src/MyPackage/Repository/`.
Repository must be named with the name of model which repository manages.
Repository contains methods querying database with `SELECT` operations.

##### Domains

Usually are located at `src/MyPackage/Domain/`.
Domain must be named with the name of model which domain manages.
Domain contains methods querying database with `INSERT`, `UPDATE`, `DELETE` operations.

##### Facades

Usually are located at `src/MyPackage/Facade/`.
Facade must be named with the name of model which facade manages.
Generally, `Facade` is what the term "facade" means in terms of programming patterns.
See [Wikipedia](https://en.wikipedia.org/wiki/Facade_pattern) or similar articles.

Facade is used when it is needed to perform method with several actions on several models.
Facade is injected by domains and repositories.

##### Services

Usually are located at `src/MyPackage/Service/`.
Services deal with functionality which is not connected with models persistence.
For example. it can be network services such as email or sms sending layers.