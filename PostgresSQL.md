# PostgreSql


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

### PL/pgSQL Constants

`constant_name CONSTANT data_type := expression;`

#### Example

```
DO $$ 
DECLARE
   VAT CONSTANT NUMERIC := 0.1;
   net_price    NUMERIC := 20.5;
BEGIN 
   RAISE NOTICE 'The selling price is %', net_price * ( 1 + VAT );
END $$;
```

Output is 
```
NOTICE:  The selling price is 22.55
```

### PL/pgSQL Errors and Messages

we will show you how to report messages and raise errors using the RAISE statement. In addition, we will introduce you to the ASSERT statement for inserting debugging checks into PL/pgSQL blocks.

`RAISE level format;`

Following the RAISE statement is the level option that specifies the error severity. PostgreSQL provides the following levels:

* DEBUG
* LOG
* NOTICE
* INFO
* WARNING
* EXCEPTION

If you don’t specify the level, by default, the RAISE statement will use EXCEPTION level that raises an error and stops the current transaction. We will discuss the RAISE EXCEPTION later in the next section.

The `format` is a string that specifies the message. The format uses percentage `( %)` placeholders that will be substituted by the next arguments. The number of placeholders must match the number of arguments, otherwise, PostgreSQL will report the following error message:

#### PL/pgSQL raising errors

To raise errors, you use the EXCEPTION level after the RAISE statement. Note that RAISE statement uses the EXCEPTION level by default.

`USING option = expression`

The option can be:

* MESSAGE: set error message text
* HINT: provide the hint message so that the root cause of the error is easier to be discovered.
* DETAIL:  give detailed information about the error.
* ERRCODE: identify the error code, which can be either by condition name or directly five-character SQLSTATE code. Please refer to the table of error codes and condition names.

The expression is a string-valued expression.

The following example raises a duplicate email error message:

```
DO $$ 
DECLARE
  email varchar(255) := 'info@postgresqltutorial.com';
BEGIN 
  -- check email for duplicate
  -- ...
  -- report duplicate email
  RAISE EXCEPTION 'Duplicate email: %', email 
      USING HINT = 'Check the email again';
END $$;

```

Output is 
```
[Err] ERROR:  Duplicate email: info@postgresqltutorial.com
HINT:  Check the email again

```

#### putting debugging checks using ASSERT statement

Sometimes, a PL/pgSQL function is so big that makes it more difficult to detect the bugs. To facilitate this, PostgreSQL provides you with the ASSERT statement for adding debugging checks into a PL/pgSQL function.

The following illustrates the syntax of the ASSERT statement:

`ASSERT condition [, message];`

The condition is a boolean expression. If the condition evaluates to TRUE, ASSERT statement does nothing. If the condition evaluates to FALSE or NULL, the ASSERT_FAILURE is raised.

```
DO $$ 
DECLARE 
   counter integer := -1;
BEGIN 
   ASSERT counter = 0, 'Expect counter starts with 0';
END $$;

```

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

### Function Parameters

we will introduce you to various kinds of PL/pgSQL function parameters: IN, OUT, INOUT and VARIADIC.

#### In parameter
```
CREATE OR REPLACE FUNCTION get_sum(
   a NUMERIC, 
   b NUMERIC) 
RETURNS NUMERIC AS $$
BEGIN
   RETURN a + b;
END; $$
 
LANGUAGE plpgsql;
```

The get_sum() function accepts two parameters: a, and b and returns a numeric. The data types of the two parameters are NUMERIC. By default, the parameter’s type of any parameter in PostgreSQL is IN parameter. You can pass the IN parameters to the function but you cannot get them back as a part of the result.

```
SELECT get_sum(10,20);
```
#### PL/pgSQL OUT parameters

The OUT parameters are defined as part of the function arguments list and are returned back as a part of the result. PostgreSQL supported the OUT parameters since version 8.1

To define OUT parameters, you use the OUT keyword as demonstrated in the following example:

```
CREATE OR REPLACE FUNCTION hi_lo(
   a NUMERIC, 
   b NUMERIC,
   c NUMERIC, 
        OUT hi NUMERIC,
   OUT lo NUMERIC)
AS $$
BEGIN
   hi := GREATEST(a,b,c);
   lo := LEAST(a,b,c);
END; $$
 
LANGUAGE plpgsql;
```

Because we use the OUT parameters, we don’t need to have a RETURN statement. The OUT parameters are useful in a function that needs to return multiple values without defining a custom type.

The following statement calls the hi_lo function:


`SELECT hi_lo(10,20,30);`

The output of the function is a record, which is a custom type. To make the output separated as columns, you use the following syntax:

`SELECT * FROM hi_lo(10,20,30);`

#### PL/pgSQL INOUT parameters

The INOUT parameter is the combination IN and OUT parameters. It means that the caller can pass the value to the function. The function then changes the argument and passes the value back as a part of the result.

```
CREATE OR REPLACE FUNCTION square(
   INOUT a NUMERIC)
AS $$
BEGIN
   a := a * a;
END; $$
LANGUAGE plpgsql;
```

#### PL/pgSQL Function Overloading

PostgreSQL allows more than one function to have the same name, so long as the arguments are different. If more than one function has the same name, we say those functions are overloaded. When a function is called, PostgreSQL determines the exact function is being called based on the input arguments.


### PL/pgSQL IF Statement

```
IF condition THEN
   statement;
END IF;
```

#### Example

```
DO $$
DECLARE
  a integer := 10;
  b integer := 20;
BEGIN 
  IF a > b THEN
   RAISE NOTICE 'a is greater than b';
  END IF;
 
  IF a < b THEN
   RAISE NOTICE 'a is less than b';
  END IF;
 
  IF a = b THEN
   RAISE NOTICE 'a is equal to b';
  END IF;
END $$;
```
#### if then else

```
IF condition THEN
  statements;
ELSE
  alternative-statements;
END IF;
```

#### Else if

```
IF condition-1 THEN
  if-statement;
ELSIF condition-2 THEN
  elsif-statement-2
...
ELSIF condition-n THEN
  elsif-statement-n;
ELSE
  else-statement;
END IF:

```

### Switch case

```
CASE search-expression
   WHEN expression_1 [, expression_2, ...] THEN
      when-statements
  [ ... ]
  [ELSE
      else-statements ]
END CASE;

```

#### Example
```
CREATE OR REPLACE FUNCTION get_price_segment(p_film_id integer)
   RETURNS VARCHAR(50) AS $$
DECLARE 
   rate   NUMERIC;
   price_segment VARCHAR(50);
BEGIN
     -- get the rate based on film_id
    SELECT INTO rate rental_rate 
    FROM film 
    WHERE film_id = p_film_id;
      
     CASE rate
   WHEN 0.99 THEN
            price_segment = 'Mass';
   WHEN 2.99 THEN
            price_segment = 'Mainstream';
   WHEN 4.99 THEN
            price_segment = 'High End';
   ELSE
       price_segment = 'Unspecified';
   END CASE;
   
   RETURN price_segment;
END; $$
LANGUAGE plpgsql;
```

### Loop

Sometimes, you need to execute a block of statements repeatedly until a condition becomes true. To do this, you use the PL/pgSQL LOOP statement. The following illustrates the syntax of the LOOP statement:

```
<<label>>
LOOP
   Statements;
   EXIT [<<label>>] WHEN condition;
END LOOP;
```

#### Example

```
CREATE OR REPLACE FUNCTION fibonacci (n INTEGER) 
   RETURNS INTEGER AS $$ 
DECLARE
   counter INTEGER := 0 ; 
   i INTEGER := 0 ; 
   j INTEGER := 1 ;
BEGIN
 
   IF (n < 1) THEN
      RETURN 0 ;
   END IF; 
   
   LOOP 
      EXIT WHEN counter = n ; 
      counter := counter + 1 ; 
      SELECT j, i + j INTO i,   j ;
   END LOOP ; 
   
   RETURN i ;
END ; 
$$ LANGUAGE plpgsql;
```
#### While loop and For loop
