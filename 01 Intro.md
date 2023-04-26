# Introduction

SQL stands for Structured Query Language.


## Database Overview

What are Databases
* Systems that allow users to store and organise data

Why use Databases
* Data Integrity, can handle massive amounts of data, quickly combine different datasets, automate steps for re-use, support data for websites and applications

Database Platform Options
* PostgreSQL (free open source, widely used on internet, multi platform)
* MySQL / MariaSQL (free open source, widely used on internet, multi platform)
* MS SQL Server Express (free w limitations, compatible w SQL server, Windows only)
* Microsoft Access (cost, not easy to use just SQL)
* SQLite (free open source, but mainly command line)

Use of SQL
* MySQL
* PostGreSQL
* Oracle Database
* Microsoft Access
* Amazon's Redshift
* Looker
* MemSQL
* Periscope Data
* Hive (Runs on top of Hadoop)
* Google's BigQuery
* Facebook's Presto


## Installation

1. Install PostGreSQL https://www.postgresql.org/
	- Be careful not to lose the database superuser password. Not security issue since running locally.
	- Port number set to 5432
2. Install PgAdmin https://www.pgadmin.org/
	- Graphical user interface for PostGreSQL
3. Download the .tar file
	- Do not open the .tar file directly
4. Restart PC after installation
5. Restore database
	- Ignore Failed Exit code if it appears
	- Open PgAdmin and set PgAdmin master password
		- Note that this master password is different from PostGreSQL's master password
	- Select the relevant version, enter the password for PostGreSQL, and save password
	- Server should connect with elephant symbol
		- If you get error with password for PostGreSQL, can try leaving it blank to see if it works
	- Create new database: R click on PostgreSQL - Create - Database. Type in name for database and click Save.
	- Restore the database: R click on name of database under Databases - Restore.
	- If you see "Utility not found: Utility file not found. Please correct the Binary Path in the Preferences dialog", go to: File > Preferences > Paths > Binary paths > PostgreSQL Binary Path. Type in the path based on Postgres location e.g. "C:\Program Files\PostgreSQL\14\bin"
	- For Format, select Custom or tar. Enter file location of .tar file into Filename. Under Restore options, select Pre-data as Yes, Data as Yes, Post-data as Yes. Then click Restore.
	- Refresh database to see changes. R click on database - Refresh.
6. Test out by opening Query Tool in PgAdmin
	- R click on database - Query Tool.
	- Try out query to see if database is restored correctly.
	- `SELECT * FROM film;`
	- Click Play button to execute query (or press F5)
	- Data should appear below in Data output

