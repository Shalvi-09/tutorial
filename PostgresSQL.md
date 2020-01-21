# PostgreSql

## PostgreSQL CREATE FUNCTION Statement

### Syntax 

```
CREATE FUNCTION function_name(p1 type, p2 type)
 RETURNS type AS
BEGIN
 -- logic
END;
LANGUAGE language_name;

```

In PostgreSQL, functions that have different parameters can share the same name. This is called function overloading, which is similar to function overloading in C++ or Java.

#### Example

```
CREATE FUNCTION inc(val integer) RETURNS integer AS $$
BEGIN
RETURN val + 1;
END; $$
LANGUAGE PLPGSQL;

```

### Creating a trigger function

A trigger function is similar to an ordinary function. However, a trigger function does not take any argument and has a return value with the type of trigger.

The following illustrates the syntax of creating trigger function:

```
CREATE FUNCTION trigger_function() 
   RETURNS trigger AS
```

A trigger function receives data about its calling environment through a special structure called `TriggerData`, which contains a set of local variables. For example,`OLD` and `NEW` represent the states of the row in the table before or after the triggering event. PostgreSQL provides other local variables starting with `TG_` as the prefix such as `TG_WHEN`, and `TG_TABLE_NAME`.

Once you define a trigger function, you can bind it to one or more trigger events such as `INSERT`, `UPDATE`, and `DELETE`.


## PostgreSQL Stored Procedures

in this tutorial, you will learn about PostgreSQL stored procedures for developing functions in PostgreSQL.

The store procedures define functions for creating triggers or custom aggregate functions. In addition, stored procedures also add many procedural features e.g., control structures and complex calculation. These allow you to develop custom functions much easier and more effective.

PostgreSQL divides the procedural languages into two main groups:

* Safe languages can be used by any users. SQL and PL/pgSQL are the safe languages.
* Sand-boxed languages are only used by superusers because sand-boxed languages provide the capability to bypass security and allow access to external sources. C is an example of a sandboxed language.

__By default, PostgreSQL supports three procedural languages: SQL, PL/pgSQL, and C.__ You can also load other procedural languages e.g., Perl, Python, and TCL into PostgreSQL using extensions.

### PL/pgSQL Block Structure

you will learn about the block structure of PL/pgSQL. You will write and execute the first PL/pgSQL block. PL/pgSQL is a block-structured language, therefore, a PL/pgSQL function or stored procedure is organized into blocks.

```

[ <<label>> ]
[ DECLARE
    declarations ]
BEGIN
    statements;
   ...
END [ label ];

```

Let’s examine the block structure in more detail:

* Each block has two sections: declaration and body. The declaration section is optional while the body section is required. The block is ended with a semicolon (;) after the END keyword.
* A block may have an optional label located at the beginning and at the end. You use the block label in case you want to specify it in the EXIT statement of the block body or if you want to qualify the names of variables declared in the block.
* The declaration section is where you declare all variables used within the body section. Each statement in the declaration section is terminated with a semicolon (;).
* The body section is where you place the code. Each statement in the body section is also terminated with a semicolon (;).

`PostgreSQL introduced the DO statement since version 9.0.  It is used to execute an anonymous block.`
#### PL/pgSQL block structure example

```
DO $$ 
<<first_block>>
DECLARE
  counter integer := 0;
BEGIN 
   counter := counter + 1;
   RAISE NOTICE 'The current value of counter is %', counter;
END first_block $$
```

Notice that the DO statement does not belong to the block. It is used to execute an anonymous block. PostgreSQL introduced the DO statement since version 9.0.

### PL/pgSQL Variables


`variable_name data_type [:= expression];`

In this syntax:

* First, specify the name of the variable. It is a good practice to assign a meaningful name to a variable. For example, instead of naming a variable i you should use index or counter.
* Second, associate a specific data type with the variable. The data type can be any valid PostgreSQL data type such as INTEGER, NUMERIC, VARCHAR and CHAR.
* Third, optionally assign a default value to a variable. If you don’t, the initial value of the variable is initialized to NULL.

#### Example
```
DO $$ 
DECLARE
   created_at time := NOW();
BEGIN 
   RAISE NOTICE '%', created_at;
   PERFORM pg_sleep(10);
   RAISE NOTICE '%', created_at;
END $$;
```

Here is the output:

```
NOTICE:  13:50:53.05891
NOTICE:  13:50:53.05891
```

#### Copying data types

PostgreSQL enables you to define a variable with a data type that references to the data type of a column of a table or the data type of another variable:

`variable_name table_name.column_name%TYPE;`

`variable_name variable%TYPE;`

For example, you can define a variable named city_name with the same data type as name the name column of the city table as follows:


```
city_name city.name%TYPE := 'San Francisco';
```

By using copying type feature, you receive the following advantages:

* First, you don’t need to care about the data type of the column. You declare a variable to just hold the values of that column in a query.
* Second, when the data type of the column changes, you don’t need to change the variable declaration in the function to adapt to the new changes.
* Third, you can refer the type of variables to data type of function arguments to create polymorphic functions since the type of internal variables can change from one call to the next.



