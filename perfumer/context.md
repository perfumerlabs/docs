---
layout: default
title: Context
nav_order: 3
parent: Perfumer
---

Context is any class that has methods which we want to use in contracts.
Contexts divides into 2 kinds: classes with constructor and classes without constructor (or ither dependencies).

In case context doesn't have constructor Perfumer can easily instantiate the object and use it.

But it is a bit more complicated if Context has dependencies. 
In this case we have to tell generator how to retrieve class object from container.

In Framework it is 4 types of business logic classes: Repository, Domain, Facade and Service. 
For each type of class Perfumer has annotation to manage with.

### Repository

When creating a new repository class add these annotations to class area:

```php
/**
 * @AnnotationExtends(class="Perfumerlabs\PerfumerFrameworkPack\Context\RepositoryCall")
 * @AnnotationProperty(name="_repository_name", value="my_repository")
 */
class MyRepository
{
    public function doSomething()
    {
    }
}
```

where "my_repository" is the name of this repository in service definitions (without "repository." prefix).

Same with other classes.

### Domain

```php
/**
 * @AnnotationExtends(class="Perfumerlabs\PerfumerFrameworkPack\Context\DomainCall")
 * @AnnotationProperty(name="_domain_name", value="my_domain")
 */
class MyDomain
{
    public function doSomething()
    {
    }
}
```

where "my_domain" is the name of this domain in service definitions (without "domain." prefix).

### Facade

```php
/**
 * @AnnotationExtends(class="Perfumerlabs\PerfumerFrameworkPack\Context\FacadeCall")
 * @AnnotationProperty(name="_facade_name", value="my_facade")
 */
class MyFacade
{
    public function doSomething()
    {
    }
}
```

where "my_facade" is the name of this facade in service definitions (without "facade." prefix).

### Service

```php
/**
 * @AnnotationExtends(class="Perfumerlabs\PerfumerFrameworkPack\Context\ServiceCall")
 * @AnnotationProperty(name="_service_name", value="my_service")
 */
class MyService
{
    public function doSomething()
    {
    }
}
```

where "my_service" is the name of this service in service definitions.
