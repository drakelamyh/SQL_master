# Conditional Expressions and Procedures

Conduct logical operations and functionality within our SQL code.

* Keywords CASE, COALESCE, NULLIF, CAST.
* Creating views
* Import and Export functionality



# CASE

Use CASE statements to execute code only when certain conditions are met (similar to IF/ELSE in other programming languages).

Can use CASE statement either as general CASE or a CASE expression.

General Syntax:

```
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE some_other_result
END
```

Example:

```
SELECT a
CASE
    WHEN a = 1 THEN 'one'
    WHEN a = 2 THEN 'two'
    ELSE 'other' AS label
END
FROM test
```

The CASE expression syntax first evaluates an expression, then compares the result with eac value in the WHEN clauses sequentially.

CASE Expression Syntax:

```
CASE Expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE some_other_result
END
```

Example:

```
SELECT a
CASE a
    WHEN 1 THEN 'one'
    WHEN 2 THEN 'two'
    ELSE 'other'
END
FROM test
```

Example:

```
SELECT customer_id
CASE
    WHEN (customer_id <= 100>) THEN 'Premium'
    WHEN (customer_id BETWEEN 100 and 200) THEN 'Plus'
    ELSE 'normal'
END AS customer_class
FROM customer
```

Example:

```
SELECT customer_id
CASE customer_id
    WHEN 2 THEN 'Winner'
    WHEN 5 THEN 'Second Place'
    ELSE 'normal'
END AS raffle_results
FROM customer
```

Example of combining SUM with aggregation:

```
SELECT
SUM(CASE rental_rate
    WHEN 0.99 THEN 1
    ELSE 0
END) AS bargains,
SUM(CASE rental_rate
    WHEN 2.99 THEN 1
    ELSE 0
END) AS regular
FROM film
```

Example:

```
SELECT
SUM(CASE rating
    WHEN 'R' THEN 1
    ELSE 0
END) AS R_SUM,
SUM(CASE rating
    WHEN 'PG' THEN 1
    ELSE 0
END) AS PG_SUM,
SUM(CASE rating
    WHEN 'PG-13' THEN 1
    ELSE 0
END) AS PG13_SUM
FROM film
```





# COALESCE

Accepts unlimited number of arguments, returns the 1st argument that is not null.

If all arguments are null, the function will then return null.

Useful when querying table that contains null values and substituting it with another value.

Basic syntax:

```
COALESCE (arg1, arg2, arg3, ..., arg_n)
```


Example:

```
SELECT item, (price - COALESCE(discount, 0))
AS final
FROM table
```





# CAST

Operator to convert one data type to another.

Note not every instance of data type can be CAST to another data type.

For example, '5' to an integer works. 'Five' to an integer will not.

General Syntax:

```
SELECT CAST('5' AS INTEGER)
```

Syntax specific to PostgreSQL:

```
SELECT '5'::INTEGER
```

Example:

```
SELECT CAST(date AS TIMESTAMP)
FROM table
```

Example:

```
SELECT CHAR_LENGTH(CAST(inventory_id AS VARCHAR)) FROM rental
```

For the previous example, if you simply try to check length of string on an integer, the operation will fail.

As such, you can cast the integer as a string first, before checking the length.






# NULLIF

