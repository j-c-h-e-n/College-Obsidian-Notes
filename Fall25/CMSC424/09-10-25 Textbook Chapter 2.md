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
# 2.3 Keys
> A way to specify how tuples within a given relation are distinguished....
> ..., no two tuples in a relation are allowed to have exactly the same value for all attributes.
- Superkey: A set of one or more attributes that allows us to identify unique a tuple in the relation (NOTE, SINGULAR RELATION).
	- Key word, these attributes must be unique, for example, IDs.
	- If K is a superkey, then any superset of K is also a superkey.
- Candidate keys: Superkeys but minimal. The simplest superkey in the set of all superkeys. Does not have a subset that can form a valid superkey.
	- ex: SSN in the US
- Primary key: A candidate key that has been chosen by a dev as the main form of identifying tuples within a relation.
	- The designation of a key also introduces a constraint in database modeling. So they're also known as primary key constraints.
- Foreign key: A primary key in attributes $A$ in relation $r_1$ that also is found in $B$  in $r_2$. 
- Foreign-key constraint: A constraint from attribute(s) $A$ of $r_1$ to the primary key $B$ of relation $r_2$ states that on any database instance, the value of each tuple for $A$ in $r_1$ must also be the same values for $B$ in $r_2$.
	- $r_1$ is called the referencing relation of the Foreign-key constraint.
	- $r_2$ is called the referenced relation.
	- For example:
	  `instructor(_ID_, name, dept_name, salary)`
	  `department(_dept_name_, building, budget)`
	  _dept_name_ in instructor is a foreign key **from** _instructor_, referencing _department_; note that _dept_name_ is the primary key of _department_.
  - Referential integrity constraint: Requiring the values associated with the attribute(s) in $r_1$ also appear in $r_2$ that have the same attribute(s).
	  - Foreign-key constraints are a special case of referential integrity constraints.
# 2.4 Schema Diagrams
- Database schemas along with their primary keys and foreign-key constraints can be represented by schema diagrams.
![[Pasted image 20250911220645.png]]

- 