- Textbook refers to SQL as a "query language" but it can do many more than query a database.
	- Define data structures.
	- Modify data inside database.
	- Specify security constraints.
# 3.1 Overview of the SQL Query Language
- The SQL DDL provides commands for deleting relation schemas, deleting relations, and modifying relation schemas.
	- Includes the ability to enforce integrity constraints that the data in the database must satisfy.
	- Includes commands for defining views.
	- Includes ability to specify authorization/access rights to relations and views.
- The SQL DML provides the ability to query information, insert tuples, delete tuples, and modify tuples in the database.
- SQL includes commands for specifying the beginning and end points of transaction.
- Embedded and dynamic SQL define how SQL statements can be used in external programming languages such as Java, C, C++.
# 3.2 SQL Data Definition
- The set of relations in a database are specified using the DDL. Allows specification of not only a set of relations but also information about each relation including:
	- The schema for each relation.
	- The types of values associated with each attribute.
	- The integrity constraints.
	- The set of indices to be maintained for each relation.
	- The security and authorization information for each relation.
	- The physical storage structure of each relation on disk.
## 3.2.1 Basic Types
The SQL standard support a variety of built-in types, including:
- char(n): A fixed-length character string with user-specified length n. The full form, character, can be used instead.
- varchar(n): A variable-length character string with user-specified maximum length n. The full form, character varying, is equivalent.
- int: An integer (a finite subset of the integers that is machine dependent). The full form, integer, is equivalent.
- smallint: A small integer (a machine-dependent subset of the integer type).
- numeric(p, d): A fixed-point number with user-specified precision. The number consists of p digits (plus a sign), and d of the p digits are to the right of th decimal point. Thus, numeric(3, 1) allows 44.5 to be stored exactly, but neither 444.5 nor 0.32 can be stored exactly in a field of this type.
- real, double precision: Floating-point and double-precision floating-point numbers with machine-dependent precision.
- float(n): A floating-point number with precision of at least n digits.
- 