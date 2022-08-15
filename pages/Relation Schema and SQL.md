- Most databse implement something similar to standard SQL. There are two aspects to SQL:
	- The _Data-Definition_ sublanguage for declaring database schema.
	- The _Data-Manipulation_ sublanguage for _quering_ databases and for _modifying_ the database.
- ## Relations in SQL
	- _Stored relations_: which are called _tables_.
	- _Views_: which are relations defined by a computation. These relations are not stored, but constructed when needed.
	- _Temporary tables_: which constructed by SQL processor when it performs its job of executing queries and data modifications. These relations are then thrown away and not stored.
- ## Data types
	- _Character strings of fixed or varying length_
		- `CHAR(n)` denotes a **fixed-length** string of up to $n$ characters.
			- It implies that short strings are padded to make $n$ characters.
		- `VARCHAR(n)` denotes a string of up to $n$ characters.
			- It implies that an endmarker or string-length is used.
	- _Bit strigns of fixed or varying length_
		- `BIT(n)` denotes bit strings of length $n$.
		- `BIT VARYING(n)` denotes bit strings of length up to $n$.
	- _Typical integer values_
		- `INT` or `INTEGER`, `SHORTINT`
	- _Floating-point numbers_
		- `FLOAT` or `REAL`
		- `DOUBLE PRECISION`
		- `DECIMAL(n, d)`: $n$ is length, The decimal point assumed to be $d$ positions from the right.
			- E.g. `0123.45` is `DECIMAL(6, 2)`
		- `NUMERIC`
	- _Dates and times_
		- `DATE` and `TIME`, these values are essentially character strings of special form.
- ## Simple Table Declarations
	- ### Create table
		- ```SQL
		  CREATE TABLE movies (
		  	title 	CHAR(100),
		    	year 	INT,
		    	length 	INT,
		    	genre	CHAR(10),
		    	stuidioName	char(30)
		  );
		  
		  CREATE TABLE movies_star (
		  	name 	CHAR(30),
		    	address VARCHAR(255),
		    	gender 	CHAR(1),
		    	birthdate DATE
		  );
		  ```
- ## Modifying Relation Schemas
	- #### Delete table
		- `DROP TABLE R`
	- #### Modify schema
		- `ALTER TABLE _ ADD _ _`
			- `ALTER TABLE movie_star ADD phone CHAR(16)`
		- `ALTER TABLE _ DROP _`
			- `ALTER TABLE movie_star DROP birthdate`
- ## Default Values
	- If you do not want the values not set to be `NULL`, use `DEFAULT`
		- `ALTER TABLE movie_star ADD phone CHAR(16) DEFAULT 'unlisted'`
- ## Declaring Keys
	- Declare keys in `CREATE TABLE` statement. There two ways to declare it.
		- Declare one attribute to be a key when that attribute is listed in the relation schema.
			- ```SQL
			  CREATE TABLE movie_star(
			  	imdb_num 	CHAR(9) PRIMARY KEY,
			    	name 		VARCHAR(64)
			  );
			  ```
		- Add to the list of items declared in the schema an additional declaration that says a particular attribute or set of attributes forms the key.
			- ```SQL
			  CREATE TABLE movie(
			  	title 	CHAR(100),
			    	year	INT,
			    	PRIMARY KEY(title, year)
			  );
			  ```
	-