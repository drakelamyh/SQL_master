# TIMESTAMPS AND EXTRACT


## SHOW

SHOW is used to show the value of a run-time parameter.
To show all current parameters (about 300+):

```
SHOW ALL
```


To show timezone (e.g. Asia/Kuala_Lumpur).

```
SHOW TIMEZONE
```

To see timestamp with timezone information.
(e.g. 2020-03-19 14:22:08.275279-04)

```
SELECT NOW()
```

As timestamp format may be hard to read, you can get the timestamp as a string.
(e.g. Thu May 19 14:23:26.524055 2020 EDT)
Easier to store and grab components from as it is a text string instead of timestamp data type.

```
SELECT TIMEOFDAY()
```

To get current time in a string format.
(e.g. 14:23:26.524055-04:00)

```
SELECT CURRENT_TIME
```

To get current date in a string format.
(e.g. 2020-03-19)

```
SELECT CURRENT_DATE
```

## EXTRACT

Use EXTRACT() to get sub-component of date value

* YEAR
* MONTH
* DAY
* WEEK
* QUARTER

Link: https://www.postgresql.org/docs/current/functions-datetime.html

Syntax:

```
EXTRACT(YEAR FROM date_col)
```

Example:

```
SELECT EXTRACT(MONTH FROM payment_date) AS pay_month
FROM payment
```


## AGE

Use AGE() to calculate current age given timestamp

Syntax:

```
AGE(col_date)
```

Example:

```
SELECT AGE(payment_date)
FROM payment
```

Example of result returned: 13 years 1 mon 5 days 01:34:13.003423


## TO_CHAR

Use TO_CHAR() to convert date types to text. Useful for timestamp formatting.
Can specify the text formatting you want (see documentation https://www.postgresql.org/docs/current/functions-formatting.html for different formatting).

Syntax:

```
TO_CHAR(date_col, 'mm-dd-yyyy')
```

Example:

```
SELECT TO_CHAR(payment_date, 'MONTH-dd / YYYY')
FROM payment
```

Returns: February 2007


In addition to formatting timestamps, TO_CHAR can also format int to string, etc.

Note: Sometimes it seems like TO_CHAR does not work. It actually does work, but need to cater for certain scenarios where the data are e.g. "blank padded to 9 characters", which means instead of returning 'Monday' it returns 'Monday   ' with extra spaces to fill up at least 9 spaces.



## TASKS

During which months did payments occur? Format your answer to return back the full month name.

Answer:

```
SELECT DISTINCT TO_CHAR(payment_date, 'MONTH')
FROM payment
```

How many payments occured on a Monday?

Answer:

```
SELECT COUNT(TO_CHAR(payment_date, 'DY'))
FROM payment
WHERE TO_CHAR(payment_date, 'DY') = 'MON'
```

Alternative Solution:

```
SELECT COUNT(*)
FROM payment
WHERE EXTRACT(dow FROM payment_date) = 1
```

Note: PostgreSQL considers Sunday the start of the week (indexed at 0).




# MATHEMATICAL FUNCTIONS AND OPERATORS

Link: https://www.postgresql.org/docs/current/functions-math.html

Used for applying mathematical functions and operators to columns and between columns.

Includes Mathematical Operators, Mathematical Functions, Random Functions, Trigonometric Functions, Hyperbolic Functions.


Example - What percentage of the replacement cost is a rental rate?

```
SELECT ROUND(rental_rate/replacement_cost,2)*100 AS percent_cost FROM film
```



# STRING FUNCTIONS AND OPERATORS

Link: https://www.postgresql.org/docs/current/functions-string.html


Example - What is the length of the customers' first name?

```
SELECT LENGTH(first_name) FROM customer
```

Example - How do you get the full name of the customers?

```
SELECT first_name || ' ' || last_name AS full_name
FROM customer
```

Example = If the database do not have employee email, generate a custom email address using company-approved email format.

```
SELECT LOWER(LEFT(first_name, 1)) || LOWER(last_name) || '@showbiz.com'
FROM customer
```




# SUBQUERY

A subquery allows you to construct more complex queries, essentially performing a query on the results of another query or using the results of another query.

The syntax involves the use of to select statements.

Example - How can we get a list of students who scored better than the average grade?

```
SELECT students, grade
FROM test_scores
WHERE grade > (SELECT AVG(grade) FROM test_scores)
```

The subquery inside the brackets is run first i.e. to get the average score.

Once the average score is obtained, the other query is run where the grade is compared against the average.


Example - to get films where the rental rate is higher than the average rental rate.

```
SELECT title, rental_rate
FROM film
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film)
```



## IN

If your subquery returns back multiple values, you cannot use mathematical operators directly.

Use the IN operator in conjuction with subquery to check against multiple results returned.

Example:

```
SELECT student, grade
FROM test_scores
WHERE students IN
(SELECT students
FROM honor_roll_table)
```

Example:

```
SELECT film_id, title
FROM film
WHERE film_id IN
(SELECT inventory.film_id
FROM rental
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')
ORDER BY film_id
```

* Using the rental table, we get the films returned between the 2 dates. However, as film_id is not inside, need to use a inner join on the results.
* Once you do that, the subquery returns a list of film_id.
* Can then use the IN operator to query to get the corresponding titles.
* Lastly, you can order the results based of a specified column e.g. film_id.



## EXISTS

The EXISTS operator is used to test the existence of rows / check if any rows are returned in a subquery.

Outer query is executed only if some values is returned for the subquery.


Syntax:

```
SELECT column_name
FROM table_name
WHERE EXISTS
(SELECT column_name FROM
table_name WHERE condition);
```

Example - Find customers who have at least one payment with amount more than 11. 

```
SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment as p
WHERE p.customer_id = c.customer_id
AND amount > 11)
```




# SELF-JOIN

Query in which table is joined to itself (join of two copies of same table).

Useful for comparing values in a column of rows within the same table.

No special keyword for self join, simply standard join syntax with same name in both parts.

However, necessary to use an alias for the table, otherwise table names will be ambiguous.

Syntax:

```
SELECT tableA.col, tableB.col
FROM table AS tableA
JOIN table AS tableB ON
tableA.some_col = tableB.other_col
```

Example - Given a table with employee id, name, and id of their reporting officer. To get back table with just the employee name and the name of their reporting offier. Can use self-join to match the id of their reporting officer to the name of the staff in the same table. 

```
SELECT emp.name AS staff, report.name AS ro
FROM employees AS emp
JOIN employees AS report ON
emp.emp_id = report.report_id
```

Example - Find all pairs of films that have the same duration length.

```
SELECT f1.title, f2.title, f1.length
FROM film AS f1
INNER JOIN film AS f2 ON
f1.film_id != f2.film_id
AND f1.length = f2.length
```

Notice for this query, the f1 film id and f2 film id are not equal to each other. Otherwise it will essenntially return a table where both titles are the same.

