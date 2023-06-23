# Primary and Foreign Keys

## Primary Keys
* A primary key is a column or group of columns used to identify a row uniquely in a table.
* Must be unique for each row.
* Must be non-null, must have entry for all primary keys.
* Allow us to easily discern what columns should be used for joining tables together.
* SERIAL data type is a way to automatically create unique integers for each row.
* Note that in pgAdmin, you can look for \[PK\] in a query call to see column denoted as Primary Key.

Examples:
* customer_id for customer database.
* NRIC for citizen data

## Foreign Keys
* A foreign key is a field or group of fields in a table that uniquely references to the primary key in another table.
* Table containing foreign key is called referencing table or child table.
* Table to which foreign key references to is called referenced table or parent table.
* Table can have multiple foreign keys depending on its relationship with other tables.
* Note that pgAdmin will not alert you directly to Foreign Keys.

Examples:
* In a payment database table, each payment row has its unique payment_id (primary key), customer_id for customer that made the payment (foreign key), and staff_id for staff that served the customer and collected the payment (foreign key).


## Usage of Keys

Primary key and foreign key make good column choices for joining together two or more tables.

When creating tables and defining columns, can use constraints to define columns as being primary key, or attaching foreign key relationship to another table.

In pgAdmin, expand out the Databases, Schemas, public, Tables, select the relevant table name, Constraints. You will see little key symbols.
* The golden key symbol means the field is the primary key.
* The silver dual key symbol means the fields are foreign keys.

You can select the relevant key symbols and click on Dependencies to see the dependencies for that particular column and what tables it is referencing to.

You can also select the relevant key symbols and right click and click on Properties. Under the Columns, you can see what table it is referencing to.



# Constraints

* Constraints are the rules enforced on data columns on table.
* Used to prevent invalid data from being entered into database.
* Ensures accuracy and reliability of data in database.

## Column Constraints
* Constraints data in column to adhere to certain conditions

## Table Constraints
* Constraints applied to entire table rather than individual column

## Common Column Constraints
* NOT NULL constraint
	* Ensures column cannot have NULL value
* UNIQUE constraint
	* Ensures all values in column are different
* PRIMARY Key
	* Uniquely identifies each row/record in database table
* FOREIGN Key
	* Constraints data based on columns in other tables
* CHECK constraint
	* Ensures all values in a column satisfy certain conditions
* EXCLUSION constraint
	* Ensures if any two rows are compared on specified column or expression using specified operator, not all of these comparisons will return TRUE.

## Common Table Constraints
* CHECK(condition)
	* Check a condition when inserting or updating data
* REFERENCES
	* Constraint value stored in column that must exist in column in another table
* UNIQUE(column_list)
	* Forces values stored in columns listed inside the parentheses to be unique
* PRIMARY KEY(column_list)
	* Allows you to define the primary key that consists of multiple columns.
