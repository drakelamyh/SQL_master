# SQL Statement Fundamentals

SQL syntax can be applied to any major type of SQL Database (MySQL, Oracle, etc.)

Note that SQL syntax keywords in caps can be run as well in lowercase, the query will still work.

Easier to read and standardised notation in caps.

Semicolon ; denotes the end of a query. Still runs even without semicolon.


## SELECT

SELECT allows us to retrieve info from table. For multiple columns, add commas to separate column names

`SELECT column_name1, column_name2 FROM table_name`

To select entire table, use the asterisk * syntax.

`SELECT * FROM table_1`

* Not good practice to use asterisk in SELECT statement If do not need all columns.

* Automatically query everything, increases traffic between the database server and application, which slows down retrieval of results.

To change the order of the columns returned. E.g. if you want c3 before c1, just type c3, c1 in the query editor.

`SELECT first_name, last_name, email FROM customer;`




## SELECT DISTINCT

Use the DISTINCT keyword to return only distinct values in a column.

`SELECT DISTINCT column_name FROM table_name`

To clarify which column DISTINCT is being applied to, can also use paranthesis for clarity

`SELECT DISTINCT(column_name) FROM table_name`

For simple calls, paranthesis is optional. For calls with COUNT and DISTINCT together, paranthesis is necessary.


## COUNT

COUNT function returns the number of input rows that match a specific condition of query.

Can apply COUNT to specific column or `COUNT(*)`, both will return the same results.

`SELECT COUNT(column_name) FROM table_name`

* Note that COUNT requires paranthesis, it will not work without it.

Can use COUNT with DISTINCT:

`SELECT COUNT(DISTINCT column_name) FROM table_name`


## SELECT WHERE

The WHERE statement allows to specify conditions on columns for the rows to be returned.

`SELECT column1, column2 FROM table_name WHERE conditions`

* The WHERE keyword appears immediately after the FROM keyword of the SELECT statement.

* The conditions are used to filter rows returned from the SELECT statement.


**Comparison Operators:**

~~~
=			Equal
>			Greater than
<			Less than
>=			Greater than or equal to
<=			Less than or equal to
<> or !=	Not equal to
~~~

**Logical Operators:**

```
AND
OR
NOT
```


Example: What is the email address for the customer with the name Nancy Thomas.

`SELECT first_name, last_name, email FROM customer
WHERE first_name = 'Nancy' AND last_name = 'Thomas';`

Example: Can you give the description for the movie "Outlaw Hanky" to a customer?

`SELECT description FROM film
WHERE title = 'Outlaw Hanky';`

Example: A customer is late with their movie return. We have mailed them a letter at their address, but we should give them a call as well.

`SELECT phone FROM address
WHERE address = '259 Ipoh Drive';`

* Note: Realise that a table can share a same name as a column and the code still runs properly.


## ORDER BY

`SELECT column_1, column_2
FROM table_1
ORDER BY column_1 DESC;`

* Note order by is at the end, after selecting and filtering is done

* If ASC or DESC is not selected, order by uses ascending by default

* You can sort by columns that are not returned in the select statement

Can also order by multiple columns e.g.

`ORDER BY company DESC, sales ASC`


## LIMIT

Allows limiting numbers of rows returned

* Goes at the very end of query and is the last command to be executed

`SELECT column_1, column_2
FROM table_1
ORDER BY column_1 ASC
LIMIT 5;`

Example:

`SELECT * FROM payment
WHERE amount != 0.00
ORDER BY payment_date DESC
LIMIT 5;`



## BETWEEN

Used to match value against range of values:

    `value BETWEEN low AND high`

BETWEEN operator is the same as `value >= low AND value <= high`


Can also combine BETWEEN with the NOT operator:

`Value NOT BETWEEN low AND high`

Same as asking `value < low OR value > high`

Can also use with dates. Need to format dates in the ISO 8601 standard format, which is YYYY-MM-DD

`date BETWEEN '2007-01-01' AND '2007-02-01'`

Pay attention when using BETWEEN versus <= >= comparison operators for datetime, as datetime starts at 0:00

E.g. `WHERE payment_date BETWEEN '2007-02-01' AND '2007-02-15'`

Results will return only up to 14 Feb, as the cutoff is at 15 Feb 0:00.


## IN

Use the IN operator to create condition that checks if value in included list of multiple options

`value IN (option1, option2, … , option_n)`

Example:

`SELECT colour FROM table
WHERE colour IN ('red', 'blue', 'green')`

Example for NOT IN:

`SELECT colour FROM table
WHERE colour NOT IN ('red', 'blue', 'green')`


## LIKE and ILIKE

Use to match general pattern in a string

Wildcard characters

`%` matches any sequence of characters

`_` matches single characters

E.g. All names that begin with an 'A'

`WHERE name LIKE 'A%'`

E.g. All names that end with an 'a'

`WHERE name LIKE '%a'`

LIKE is case-sensitive, while ILIKE is not case-sensitive

E.g. Get all Mission Impossible films

`WHERE title LIKE 'Mission Impossible _'`

Can use multiple underscores e.g.

`WHERE value LIKE 'Version#__'`

E.g. `WHERE name LIKE '_her%'`

Returns Cheryl, Theresa, Sherri

E.g. Returns all films with truman somewhere in the film title

`SELECT COUNT(title) FROM film
WHERE title ILIKE '%Truman%';`

PostgreSQL supports full regex capabilities https://www.postgresql.org/docs/current/functions-matching.html


## GROUP BY

Aggregate function - take multiple inputs and returns single output

Documentation: https://www.postgresql.org/docs/current/functions-aggregate.html


`SELECT MIN(replacement_cost) FROM film
SELECT MAX(replacement_cost) FROM film
SELECT ROUND(AVG(replacement_cost),2) FROM film
SELECT SUM(replacement_cost) FROM film`

To return multiple values:

`SELECT MIN(replacement_cost), MAX(replacement_cost) FROM film`

Aggregating to return single value, so does not work if want to get multiple values. If need multiple values, need to use group by.

Group by - aggregate columns per some category

Choose a categorical column (not continuous, or treated as non-continuous)

Category can be numeric columns but treated as categorical for purpose of group by

Syntax:

`SELECT category_col, AGG(data_col)
FROM table
GROUP BY category_col`

`SELECT category_col, AGG(data_col)
FROM table
WHERE category_col != 'A'
GROUP BY category_col`

Note that the group by clause must appear right after a FROM or WHERE statement.

In the SELECT statement, columns must either have an aggregation function or be in the GROUP BY call.

Example:

`SELECT company, division, SUM(sales)
FROM finance_table
GROUP BY company, division`

Should not use WHERE to do filtering for the field being aggregated

Can use WHERE to filter out the fields before doing GROUP BY

If want to sort results based on aggregate, must reference the entire function

`SELECT company, SUM(sales)
FROM finance_table
GROUP BY company
ORDER BY SUM(sales)`

Example: What customers are spending the most money?

`SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC`

Example: Which dates have the highest transaction amounts?

Note that for date timestamp, since it has time as well, need to format it to a date before you can perform GROUP BY

`SELECT DATE(payment_date), SUM(amount) FROM payment
GROUP BY DATE(payment_date)
ORDER BY SUM(amount) DESC`


## HAVING

If want to filter based on aggregated field, HAVING allows us to use aggregated result as a filter along with a GROUP BY

`SELECT company, SUM(sales)
FROM finance_table
WHERE company != 'Google'
GROUP BY company
HAVING SUM(sales) > 1000`


## Assessment Test

Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2.

The answer should be customers 187 and 148.

`SELECT customer_id, SUM(amount) FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount) > 110`

How many films begin with the letter J?

The answer should be 20.

`SELECT count(*) FROM film
WHERE title LIKE 'J%'`

What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

The answer is Eddie Tomlin

`SELECT first_name,last_name FROM customer
WHERE first_name LIKE 'E%' AND address_id <500
ORDER BY customer_id DESC
LIMIT 1`

