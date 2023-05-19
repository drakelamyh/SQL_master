# SQL Joins

SQL JOINS Explained with Venn Diagrams
https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/

SQL JOIN Examples
https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-joins/

Wikipedia Page on SQL JOINS
https://en.wikipedia.org/wiki/Join_(SQL)



## AS statement

Allows us to create an alias for a column or result

Syntax:

```
SELECT column_name AS new_name
FROM table_name
```

Can also use it on functions on a column. Example:

```
SELECT SUM(amount) AS net_revenue
FROM payment_table
```

* For this example, the result will show as net_revenue, which will make it easier for other users to interpret the results.

The AS operator gets executed at the very end of the query and only exists at the result.

* Cannot use the alias inside other operators.

An example that will not work:

```
SELECT customer_id, SUM(amount) AS total_spent
FROM payment
GROUP BY customer_id
HAVING total_spent > 100
```

Instead, need to state the original column name.

`HAVING SUM(amount) > 100`



## INNER JOINS

Syntax:

```
SELECT * FROM table_a
INNER JOIN table_b
ON table_a.col_match = table_b.col_match
```

Example:

```
SELECT * FROM Registrations
INNER JOIN Logins
ON Registrations.name = Logins.name
```

* Note that when using inner join, the column that is being matched will show up twice, once for each original table.
* If you do not want a duplicate, need to specify what table the column to return.

Example:

```
SELECT reg_id, Logins.name, log_id FROM Registrations
INNER JOIN Logins
ON Registrations.name = Logins.name
```

* Notice that you need to specify which of the two tables is the matched column being retrieved from and not merely the column name (e.g. Logins.name instead just name).
* If you use JOIN without the INNER, PostgreSQL will treat it as an INNER JOIN.
* Note that the 2 columns to be matched need not have the same name. 



## FULL OUTER JOINS

Syntax:

```
SELECT * FROM table_a
FULL OUTER JOIN table_b
ON table_a.col_match = table_b.col_match
```

Example:

```
SELECT * FROM Registrations
FULL OUTER JOIN Logins
ON Registrations.name = Logins.name
```

For rows where not all info is present, SQL will fill in null values

To get rows with no intersections, we can use the following:

Syntax:

```
SELECT * FROM table_a
FULL OUTER JOIN table_b
ON table_a.col_match = table_b.col_match
WHERE table_a.id IS null OR
Table_b.id IS null
```

Example:

```
SELECT * FROM Registrations
FULL OUTER JOIN Logins
ON Registrations.name = Logins.name
WHERE Registrations.reg_id IS null OR Logins.log_id IS null
```

This will show us results with unique rows in table_a only or in table_b only. For rows with rows in both tables, there will not be a null and therefore it will not be shown.

Essentially this is the opposite of an inner join.



## LEFT JOIN

Syntax:

```
SELECT * FROM table_a
LEFT OUTER JOIN table_b
ON table_a.col_match = table_b.col_match
```

LEFT JOIN can also be used instead of LEFT OUTER JOIN.

Note that order matters for left join. The first table indicated (e.g. table_a) is the table where all rows will be returned. The other table (e.g. table_b) will only be returned for rows where there is a match.

Example:

```
SELECT * FROM Registrations
LEFT OUTER JOIN Logins
ON Registrations.name = Logins.name
```

If we instead want to get entries unique to table_a and not found in table_b.

```
SELECT * FROM table_a
LEFT OUTER JOIN table_b
ON table_a.col_match = table_b.col_match
WHERE table_b.id IS null
```

If the column from table_b is null, this means that there is no match in table_a and that the rows returned are unique to table_a and not found in table_b.



## RIGHT JOIN

Essentially the same as LEFT JOIN, except the tables are switched.
Exactly the same as switching the table order in a LEFT JOIN.

Syntax:

```
SELECT * FROM table_a
RIGHT OUTER JOIN table_b
ON table_a.col_match = table_b.col_match
```

RIGHT JOIN can also be used instead of RIGHT OUTER JOIN.

If we instead want to get entries unique to table_b and not found in table_a.

```
SELECT * FROM table_a
RIGHT JOIN table_b
ON table_a.col_match = table_b.col_match
WHERE table_a.id IS null
```

Main benefit to RIGHT JOIN is just to keep the tables in the same 'order'. However, if not relevant, you can just stick to LEFT JOIN and flip the table order to get the same result as a RIGHT JOIN.



## UNION

* The UNION operator is used to combine the results-set of two or more SELECT statements.
* Serves to concatenate two results together.

Syntax:

```
SELECT column_names FROM table1
UNION
SELECT column_names FROM table2
```

Note: Code should be logical and they should match up in a way that you can stack the results right on top of another.

Example:

```
SELECT * FROM Sales2021Q1
UNION
SELECT * FROM Sales2021Q2
ORDER BY name;
```

That way you will get the sales results for everyone for the first half of 2021 (i.e. both Q1 and Q2).



## JOIN CHALLENGE TASKS

California sales tax laws have changed and we need to alert our customers to this through email. What are the emails of the customers who live in California?

Solution:

```
SELECT district, email FROM address
INNER JOIN customer
ON address.address_id = customer.address_id
WHERE district = 'California'
```

A customer is a huge fan of the actor "Nick Wahlberg" and wants to know which movies he is in. Get a list of all movies "Nick Wahlberg" has been in.

Solution:

```
SELECT title, first_name, last_name
FROM film_actor INNER JOIN actor
ON film_actor.actor_id = actor.actor_id
INNER JOIN film
ON film_actor.film_id = film.film_id
WHERE first_name = 'Nick'
AND last_name = 'Wahlberg'
```

