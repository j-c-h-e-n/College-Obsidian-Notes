# Intro
>The relational mode's independence form any specific underlying low-level data structures has allowed it to persist despite the advent of new approaches to data storage...
# 2.1 Structure of Relational Databases
- Database:
	-  A collection of tables, each with a unique name.
- Relation/Table:
	- Houses rows of relationships. The data.
- Attribute:
	- The column names in a relation/table.
- Tuple/Row:
	- Represents a *relationship* among a set of values.
	- A table is a collection of these relationships.
		- As a result, close correspondence with the term *table* and the math concept *relation*.
- Relation Instance:
	- A specific instance of a relation containing a specific set of rows (subset?).
- Domain:
	- The set of permitted values for an attribute of a relation.
	- These are atomic, i.e. these values are indivisible (no lists, etc. they are single values).
	- Null is a special value denoting that the value is unknown or does not exist.
# 2.2 Database Schema
- Database schema: The logical design of the database.
- Database instance: A snapshot of the data in the database at a given instance in time.
- Relations are to variables as schemas are to type definitions.
- 