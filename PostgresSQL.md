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
