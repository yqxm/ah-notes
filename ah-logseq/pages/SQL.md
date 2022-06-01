- **Query**
	- ```SQL
	  # Query a table
	  SELECT <columns> FROM <tb1> 
	  
	  # filter rows
	  SELECT <columns> FROM <tb1> WHERE <predicates>
	  ```
	- use boolean operators `NOT`, `AND`, `OR` for complicate predicates.
		- the order of evaluation is
			- 1. `NOT`
			- 2. `AND`
			- 3. `OR`
	- **NULL**
		- `NULL` can be used as a value for any data type.
			- If you do anything with `NULL`, you'll just get NULL.
			- `NULL` is falsey, meaning that `WHERE NULL` is just like `WHERE FALSE`
			- `NULL` [short-circuits]([[NULL short-circuits in SQL]]) with boolean operators.
-