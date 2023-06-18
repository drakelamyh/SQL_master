# Data Types

* Boolean
	* True or False
* Character
	* char, varchar, and text
* Numeric
	* integer, floating-point number
* Temporal
	* date, time, timestamp, and interval
* UUID - Universally Unique Identifiers
	* algorithmically unique code to make sure have unique identifier for particular row
* Array
	* Stores array of strings, numbers, etc.
* JSON
* Hstore key-value pair
* Network address
* Geometric data

Documentation for limitations of data types:
postgresql.org/docs/current/datatype.html

Example - for phone numbers, best to store it as a text type (or phone number specific data types which can be installed via additional libraries) instead of numeric type:
* No arithmetic required
* Leading zeros can cause issues, as 07 and 7 are treated the same numerically, but are not the same phone number.
* Symbols in country codes (e.g. +65)

Considerations for selecting data types to use:
* Search for best practices online on which data type is best used.
* Plan for long term storage.
* Easier to remove historical data not needed in future, but harder to go back in time to add data.
