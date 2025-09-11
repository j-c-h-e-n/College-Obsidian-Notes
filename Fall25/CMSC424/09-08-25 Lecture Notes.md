## Relational Data Model
Introduced by Ted Codd (late 60's - early 70's).
- Before = "Network Data Model" and "Hierarchical Data Model".
- Very contentious: Database Wars (Charlie Bachman vs. Ted Codd).
Relational data model contributes:
1. Separation of logical, physical data models (data independence).
2. Declarative query languages.
3. Formal semantics.
4. Query optimization (key to commercial success).
1st prototypes:
- Ingres -> CA -> Actian
- Postgres -> Illustra -> Informix -> IBM
- System R -> Oracle, DB2

### Key Abstraction: Relation
Why called relations?
- Closely corresponds to mathematical concept of a relation.
Terms:
- Table : relation.
- Rows : tuples.
- Columns : attributes.
- Schema : "format".

### Definitions
- Relation Schema:
	- A list of attributes and their domains.
- Relation Instance:
	- A particular instantiation of a relation with actual values.
- Domains of an attribute/column:
	- The set of permitted values. We typically assume domains are **atomic**, i.e., the values are treated as indivisible (no lists).
- Null Value:
	- A special value used if the value of an attribute for a row is:
		- unknown, inapplicable, withheld/hidden.
	- Different interpretations all captured by a single concept - leads to major headaches and problems.

## Keys (IMPORTANT)
https://www.geeksforgeeks.org/dbms/types-of-keys-in-relational-model-candidate-super-primary-alternate-and-foreign/
- Let K be a subset of R (all attributes).
### Superkey
- K is a **superkey** of R if values for K are sufficient to identify a unique tuple of any possible relation r(R).
- Superkey K is a candidate key if K is minimal (no subset of it i s a superkey).

### Primary key
- One of the candidate keys is selected to be the primary key.
	- Typically one that is small and immutable.
- Primary key is typically highlighted (underlined, etc.).
### Foreign Key
"Primary key of a relation that appears in another relation."
- {ID} from student appears in *takes, advisor*.
- Student is called referenced relation.
- takes is the referencing relation.
- Typically shown as an arrow from referencing to referenced.

### Foreign key constraint
The tuple corresponding to that primary key must exist.
- Imagine:
	- Tuple: ('student101', 'CMSC424)