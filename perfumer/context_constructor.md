---
layout: default
title: Context constructor
nav_order: 5
parent: Perfumer
---

Perfumer suggests means to generate class constructor.

Suppose we want to create new service class with a number of dependencies in constructor:
MyRepository, MyDomain and MyFacade classes.

To achieve this add @Inject annotation. This will work only for interfaces.

```php
/**
 * @AnnotationExtends(class="Perfumerlabs\PerfumerFrameworkPack\Context\ServiceCall")
 * @AnnotationProperty(name="_service_name", value="my_service")
 *
 * @Inject(name="my_repository", type="MyProject\Repository\MyRepository")
 * @Inject(name="my_domain", type="MyProject\Repository\MyDomain")
 * @Inject(name="my_facade", type="MyProject\Repository\MyFacade")
 */
interface MyService
{
    public function doSomething();
}
```

After regeneration a new class MyService will appear in Service directory.

```php
class MyService extends \Generated\MyProject\Service\MyService
{
    public function doSomething()
    {
        // use injected dependencies via "get"
        
        $my_repository = $this->getMyRepository();
        
        $my_domain = $this->getMyDomain();
        
        $my_facade = $this->getMyFacade();
    }
}
```