# django-migrate-sql
Django Migrations support for raw SQL

[![Build Status](https://travis-ci.org/klichukb/django-migrate-sql.svg?branch=master)](https://travis-ci.org/klichukb/django-migrate-sql)
[![codecov.io](https://img.shields.io/codecov/c/github/klichukb/django-migrate-sql/master.svg)](https://codecov.io/github/klichukb/django-migrate-sql?branch=master)

## What it does
* Makes maintaining your SQL functions, custom composite types, indices and triggers easier.
* Structures SQL into configuration of **SQL items**, that are identified by names and divided among apps, just like models.
* Automatically gathers and persists changes of your custom SQL into migrations using `makemigrations`.
* Properly executes backwards/forwards keeping integrity of database.
* Create -> Drop -> Recreate approach for changes to items that do not support altering and require dropping and recreating.
* Dependencies system for SQL items, which solves the problem of updating items, that rely on others (for example custom types/functions that use other custom types), and require dropping all dependency tree previously with further recreation.

## What it does not
* Does not create `ALTER` queries for items that support this, for example `ALTER TYPE` in Postgre SQL, because is database-agnostic. In case your tools allow rolling all the changes through `ALTER` queries, you can consider not using this app **or** restructure migrations manually after creation by nesting generated operations into `state_operations` of basic `RunSQL` that does `ALTER`s.
* Does not validate SQL nor preprocesses queries during `makemigrations` or `migrate` because is database-agnostic.
* Does not check whether SQL config dependencies do match database relations/dependencies because guess why.
* (**TODO**)(During `./manage.py migrate` does not restore full state of items for analysis, thus does not notify about existing changes to schema that are not migrated **and** does not recognize circular dependencies during migration execution.
* (
