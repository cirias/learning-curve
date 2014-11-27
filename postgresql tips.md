* [TeamPostgreSQL](http://54.191.127.175:8082/teampostgresql/#)


* Trigger
  * [Overview of Trigger Behavior](http://www.postgresql.org/docs/9.1/static/trigger-definition.html)
  * [Trigger Procedures](http://www.postgresql.org/docs/9.3/static/plpgsql-trigger.html)
  * [Create Trigger](http://www.postgresql.org/docs/9.3/static/sql-createtrigger.html)
  * [Create Constraint Trigger](http://www.postgresql.org/docs/9.0/interactive/sql-createconstraint.html)


* Functions
  * [Data Type Formatting Functions](http://www.postgresql.org/docs/9.3/static/functions-formatting.html)
  * [String Functions and Operators](http://www.postgresql.org/docs/9.3/static/functions-string.html)
  * [JSON Functions and Operators](http://www.postgresql.org/docs/9.3/static/functions-json.html)
  * [Array Functions and Operators](http://www.postgresql.org/docs/9.1/static/functions-array.html)


* Transcations
  * [Transactions](http://www.postgresql.org/docs/8.3/static/tutorial-transactions.html)
  * [Transaction Isolation](http://www.postgresql.org/docs/9.1/static/transaction-iso.html)


* [Basic Statements](http://www.postgresql.org/docs/9.3/static/plpgsql-statements.html)
* [Declarations](http://www.postgresql.org/docs/9.1/static/plpgsql-declarations.html)
* [Data Types](http://www.postgresql.org/docs/8.4/static/datatype.html)
* [Create Function](https://github.com/brianc/node-postgres/wiki/Client#method-query-parameterized)

```
-- Check Deadlock
SELECT * FROM pg_stat_activity WHERE datname='database name';
-- WHERE waiting = 't'

-- Kill the Dead Process
SELECT pg_terminate_backend('pid');
```
