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

NULLIF takes in 2 inputs and returns NULL if both equal, else it returns the 1st argument passed.

Useful when NULL value will cause error or unwanted result.

Syntax:

```
NULLIF(arg1, arg2)
```

Example where we want to measre ratio of staff in dept A to B, but B happens to be 0.

If we divide, an error where division by zero occurs.

Instead, we can use NULLIF to introduce a NULL in the event dept B is 0.

```
SELECT(
    SUM(CASE WHEN department = 'A' THEN 1 ELSE 0 END) /
    NULLIF(SUM(CASE WHEN department = 'B' THEN 1 ELSE 0 END), 0)
) AS department_ratio
FROM depts
```




# VIEWS

For common queries used often, can create a VIEW to quickly see the query.

A view is a database object that is essentially a stored query.

Can be accessed as virtual table in PostgreSQL.

Note that a view does not store data physically, it simply stores the query.

Example syntax:

```
SELECT * FROM view
```

Example:

```
CREATE VIEW cust_info AS
SELECT first_name, last_name, address FROM customer
INNER JOIN address
ON customer.address_id = address.address.id

```

To alter a view (i.e. change the underlying query info), call the CREATE OR REPLACE command.

Example:

```
CREATE OR REPLACE VIEW cust_info AS
SELECT first_name, last_name, address FROM customer
INNER JOIN address
ON customer.address_id = address.address.id
```

To drop a view, use the DROP VIEW command.

Example:

```
DROP VIEW cust_info
```

Can check whether view exist first before dropping.

Example:

```
DROP VIEW IF EXISTS cust_info
```

To rename a view:

```
ALTER VIEW cust_info RENAME to c_info
```






# Importing and Exporting Data

Note: Not every outside data file will work. Variations and things like formatting, macros embedded in the file, data types, data misalignment, etc., may prevent the input command from reading the file.

Details of compatible file types and examples: https://www.postgresql.org/docs/current/sql-copy.html

The copy command is what import uses internally when it's actually running the SQL code to import things through the PgAdmin graphical user interface.

Note: You must provide the 100% correct file path to your outside file. Otherwise, the import command will fail to find the file.

Note: The import command does not create a table for you.

It assumes table already created.

Currently no automated way within PgAdmin to create table directly from CSV file.

Tools available to combine both steps, but not part of standard PgAdmin or PostgreSQL.

Stack Overflow - How to import CSV file data into a PostgreSQL table
https://stackoverflow.com/questions/2987433/how-to-import-csv-file-data-into-a-postgresql-table

Enterprise DB - How to import and export data using CSV files in PostgreSQL
https://www.enterprisedb.com/postgres-tutorials/how-import-and-export-data-using-csv-files-postgresql

Stack Overflow - Can I automatically create a table in PostgreSQL from a csv file with headers?
https://stackoverflow.com/questions/21018256/can-i-automatically-create-a-table-in-postgresql-from-a-csv-file-with-headers

Example of creating table first:

```
CREATE TABLE simple(
a INTEGER,
b INTEGER,
c INTEGER,
)
```

Then, go to the database, select Schemas, public, Tables, and locate the table created.

Right click and select Import/Export.

Under the Options tab:
* Under Import/Export, switch to Import
* Under Filename, either type in file path or click the three dots to explore the file.
* Under Format, make sure it is correct e.g. csv
* Under Header, indicate Yes if there is a header in the CSV file
* Under Delimiter, select comma for csv file
* Under Quote, select the relevant symbol to specify quotes. Default is double-quote.
* Under Escape, select the relevant symbol for the escape character, which is used to prevent the interpretation of special characters in a string.

Under the Columns Tab
* Under Columns to import, you can remove columns if not required.

To export, select the table of interest in PostgreSQL, right click and select Import/Export.

Under the Options tab:
* Under Import/Export, switch to Export
* Under Filename, type in file path to save the new exported file
* Under Format, make sure it is correct e.g. csv
* Under Header, indicate Yes if there is a header in the CSV file
* Under Delimiter, select comma for csv file
* Under Quote, select the relevant symbol to specify quotes. Default is double-quote.
* Under Escape, select the relevant symbol for the escape character, which is used to prevent the interpretation of special characters in a string.

Under the Columns Tab
* Under Columns to column, select the columns required.

