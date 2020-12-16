---
layout: default
title: Setters
nav_order: 1
parent: Generator
---

Setters are annotations which define initial variables.
Setters can get variables from request/config/container/etc.

### Reference

In all annotations "value" property is the name of variable in source, "name" - the name of variable in the scope to create.

```
@SetFromField(name="name", value="value")
```

Sets "value" from request body.

```
@SetFromAllFields(name="name")
```

Sets to $name array with all request fields.

```
@SetFromStringField(name="name", value="value")
```

Sets "value" from request body. Casts numbers to string. $name is null, if "value" is not string or number.

```
@SetFromIntField(name="name", value="value")
```

Sets "value" from request body. Casts string to int. $name is null, if "value" is not string or int.

```
@SetFromBoolField(name="name", value="value")
```

Sets "value" from request body. $name is null, if "value" is not bool.

```
@SetFromArrayField(name="name", value="value")
```

Sets "value" from request body. $name is null, if "value" is not array.

```
@SetFromParam(name="name", value="value")
```

Sets "value" from config.

```
@SetFromRepository(name="name", value="value")
```

Sets "value"-repository class instance to $name.

```
@SetFromDomain(name="name", value="value")
```

Sets "value"-domain class instance to $name.

```
@SetFromFacade(name="name", value="value")
```

Sets "value"-facade class instance to $name.

```
@SetFromService(name="name", value="value")
```

Sets "value"-service class instance to $name.

```
@SetFromAuth(name="name")
```

Sets "auth" service instance to $name.

```
@SetFromAuthUser(name="name")
```

Sets authorised user object to $name.

```
@SetFromAuthUserId(name="name")
```

Sets authorised user objects ID to $name.