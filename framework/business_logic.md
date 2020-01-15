---
layout: default
title: Business Logic
nav_order: 5
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

Usually are located at `src/MyModule/Repository/`.
Repository must be named with the name of model which repository manages.
Repository contains methods querying database with `SELECT` operations.

##### Domains

##### Facades

##### Services