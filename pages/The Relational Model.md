- ## The Advantages of The relational model:
	- Provide a simple, limited approach to structuring data, yet is reasonalbly versatile, so anything can be modeled.
	- Provides a limited, yet useful, collection of operations on data.
- ## Terminologies
	- _Relation_: Two dimensional table.
	- _Attributes_: The column of a relation.
	- _Schema_: The name of a relation and the set of attributes.
		- ```text
		  Movies(title, year, length, genre)
		  ```
	- _Database Schema_: The set of schemas.
	- _Tuples_: The rows of a relation, other than the header row containing the attribute names.
	- _Domain_: The type of attributes. Each column values shoud belongs to its domain.
	- _Relation Instance_: the set of tuples of a given relation is a instance of that relation.
	- _Keys_: The set of attributes forms a key for a relation if we do not allow two tuples in a relation instance to have the same values in all the attribute of the key.
- ## [[Relation Schema and SQL]]
- ## [[An Algebraic Query Language]]
-