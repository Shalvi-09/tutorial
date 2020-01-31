
http://thedeveloperspace.com/postgresql-finally-gets-stored-procedures/


##  PostgreSQL finally gets Stored Procedures 
PostgreSQL has traditionally supported User Defined Functions since a long time. Functions allow us to store and execute procedural code repeatedly. The downside with Functions is its lack of support for Transactions in the Function body, the entire Function body is executed within an implicit transaction.

## The Problem
Lack of transaction support in Functions meant that if multiple sets of statements had to be executed, each within a transaction, you had to write one Function for each.

When migrating RDBMS like Oracle or SQL Server to PostgreSQL 10.x or older, Stored Procedures had to be re-written as PostgreSQL Functions. If the Stored Procedures used Transaction logic within them, they had to be rewritten as multiple PostgreSQL Functions, one for each transaction. This was a very difficult and error-prone task since it involved manual rewriting of code and thorough Unit testing covering all use-cases. It was difficult to ensure that the PostgreSQL Functions did exactly the same thing as the Stored Procedures they were rewritten from.

## Stored Procedures
__`PostgreSQL 11.5. released on 10/18/2018`__. includes PROCEDURE as a new Schema object. A stored procedure is created using the `CREATE PROCEDURE` statement. You can also use the` CREATE OR REPLACE` syntax similar to Functions. You can start multiple new transactions, commit or rollback them within a Stored Procedure.

### Overloaded Stored Procedures
Stored Procedures can be overloaded similar to Functions, i.e. you can create multiple Stored Procedures with different parameters. When a Stored Procedure call is made, procedure resolution is decided on best match of parameters, as with Functions.

### Nested Stored Procedures
Stored Procedures can also be nested, i.e. you can call a Stored Procedure from another Stored Procedure, similar to Functions.

## Stored Procedure vs Function
Though Stored Procedures and Functions look very similar, there are few fundamental differences:

| Feature |	Stored Procedure |	Function |
| :--------- | ------------- | ---------- |
Use in an expression | 	No | 	Yes | 
Return a value |	No |	Yes |
Return values as OUT parameters |	Yes |	No |
Return a single result set |	Yes |	Yes (as a table function) |
Return multiple result sets |	Yes |	No |

## Why Stored Procedures?

* Stored Procedures allows transaction control through multiple COMMIT and ROLLBACK statements.
 * Makes migration of Oracle and SQL Server databases to PostgreSQL very easy since Stored Procedures can be rewritten as is without the need to rewrite them as multiple functions.
 * The syntax of CREATE PROCEDURE is very similar to CREATE FUNCTION and the learning curve is very narrow.


## Creating a new Stored Procedure
To create a new Stored Procedure, you use the CREATE PROCEDURE statement. It is very similar to CREATE FUNCTION statement except that it does not have the RETURNS clause. Here’s an example.


```
CREATE OR REPLACE PROCEDURE TestProcedure() 
 LANGUAGE plpgsql 
 AS $$
 DECLARE
 BEGIN
     CREATE TABLE table1 (id int, name text);
     INSERT INTO  table1 VALUES (1, "Andy");
     COMMIT;
     CREATE TABLE table2 (id int, name text);
     INSERT INTO  table2 VALUES (1, "Brian");
     ROLLBACK;
 END $$; 
 
```

For the complete syntax of CREATE PROCEDURE statement, check [this](https://www.postgresql.org/docs/11/sql-createprocedure.html) documentation page.


## Execute a Stored Procedure

You can execute a Stored Procedure using the CALL statement
```
CALL TestProcedure();
```











