---
layout: default
title: Database
nav_order: 60
parent: Framework
---

### ORM

Framework uses [Propel ORM 2](http://propelorm.org/).

[XML schema](http://propelorm.org/documentation/reference/schema.html) of project usually is at `MyPackage/Resource/propel/schema` or `app/propel/schema`,
though it depends from particular project.

Generated propel models are usually at `src/MyPackage/Model`.

### How to make migrations

1. Edit schema.xml file as you need.
1. Run console command `php cli/dev framework propel/diff`.
1. The command generates migration file at `src/MyPackage/Resource/propel/migration/`. Review it.
1. In order to execute migration run `php cli/dev framework propel/migrate`.
1. Generate models with command `php cli/dev framework propel/model/build`.
1. If you need rollback migration run `php cli/dev framework propel/migration/down`.

### Migration files

Keep in mind, ALWAYS review your migration files to check if generated SQL corresponds to XML schemas changes.
Remove any changes that does not reflect to changes.
Known issues with migrations:

- if a column has "sqlType=jsonb", then every `propel/diff` generates a query like `ALTER TABLE "my_table" ALTER COLUMN "my_column" TYPE jsonb USING NULL;`
- if a column has "sqlType=cidr", then every `propel/diff` generates a query like `ALTER TABLE "my_table" ALTER COLUMN "my_column" TYPE cidr USING NULL;`
- Propel does not deal well with different `type` and `sqlType` for now

### Querying, behaviours, etc

Any documentation regarding usage of ORM you can find at official [Propel ORM site](http://propelorm.org/). 
