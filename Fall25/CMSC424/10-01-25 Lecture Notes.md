# Client-server Architectures
- Usually two-tier and three-tier architectures.
	- Two tier has user + application for the client. A network connects them to the database system under the "server label".
	- Three tier has user + application client for the client label. A network connects them to the application server + database system under the "server label".
# JDBC and ODBC
- API for a program to interact with a database server.
## SQL Injections
- USE PREPARED STATEMENTS.
- Prevents users from copy pasting malicious queries and these queries getting directly processed as is.
# ER (Entity-Relationship) Model
- Entities:
	- An object that exists and is distinguishable from other objects.
		- Example: Bob Smith, BofA, CMSC424
	- Have *attributes* (people have names and addresses).
	- Form *entity sets* with other entities of the same type that share the same properties
		- Set of all people, set of all classes.
	- Entity sets may overlap.
- Relationships:
	- Relates 2 or more entities
		- Bob *has an account at* College Park Branch
	- Forms *relationship sets* with other relationships of the same type that share the same properties.
		- Customers *have accounts at* Branches.
## Relationship Cardinalities
- We may know:
	- One customer can only open one account
	  OR
	  One customer can open multiple accounts.
- Allows us to better manipulate data.
### Mapping Cardinalities
- One-to-One
- One-to-Many
- Many-to-One
	- "x can have at most 1 of y"
- Many-to-Many
- N-ary relationships? (we will not talk about this in CMSC424)
## Types of Attributes
- Simple vs Composite
	- First name + last name composes a name.
	- Date of birth can be composite since it is composed of month, day, year.
- Single-valued vs Multi-valued.
	- Phone numbers are multi-valued.
- Derived.
	- If date-of-birth is present, age can be derived.
	- Helps in avoiding redundancy.