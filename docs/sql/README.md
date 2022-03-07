# SQL Guidelines

These guidelines apply to relational databases, specifically Microsoft SqlServer.

## Tables

Tables should contain the following:

* Table names should be singular, i.e. Customer, not Customers
* Unique `PRIMARY KEY` column that following the naming pattern of {table}Id, i.e. CustomerId.  This should be numeric and incrementing, ideally an identity column.
* When table will be used in or part of a REST resource, a `uniqueidentier` column that has a `UNIQUE` constraint on it following the naming pattern of {table}ResourceId, i.e. CustomerResourceId.
* Excepting in case of a pure join table, all tables should have a set of audit stamp column for keeping track of when a row was created or last modified
    * CreatedDate
    * CreateSubjectId
    * LastModifiedDate
    * LastModifiedSubjectId
