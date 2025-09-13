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
# 2.5 Relational Query Languages
- Query Language: A language in which a user requests information from the database. Can be imperative, functional, or declarative.
	- Imperial query language: User instructs system to use operations to compute a result.
	- Functional query language: basically just a functional programming language but used to query.
	- Declarative query language: User describes the desired information without specifying steps to retrieve it. it is the job of the database to give the information.
# 2.6 The Relational Algebra
- Subset of algebra that consists of a set of operations that take one or two relations as input and produces a new relation as the result.
- Select, product, and rename are *unary* - for one relation.
- Union, cartesian product, and set difference are *binary* - operates on pairs of relations.
- Relational algebra forms the basis of SQL.
- **It is worth reminding** that relations are SETS of tuples. Therefore there are no repeating tuples.
	- But in practice, tables in databases do allow duplicates.
## 2.6.1 The Select Operation (σ)
- Connectives where we can chain and modify our parameters:
	- $\wedge$ (and), $\vee$ (or), $\neg$ (not)
- Selects tuples that satisfy a given predicate. Represented with sigma (σ).
	- ex:
		- $\sigma_{dept\_name=''Physics''}(instructor)$ selects tuples with dept_name = "Physics" in the instructor relation.
		- $\sigma_{salary>90000}(instructor)$ selects tuples with salaries > 90000 in the instructor relation.
		- $\sigma_{dept\_name=''Physics''\wedge salary>90000}(instructor)$ selects tuples with dept_name = "Physics" and salary > 90000 in the instructor relation.
		- $\sigma_{dept\_name = building}(department)$ selects tuples where the dept_name value equals the building value.
## 2.6.2 The Project Operation ($\Pi$)
- Allows us to view attributes from a relation with other attributes left out.
	- ex:
		- $\Pi_{ID, name, salary} (instructor)$ shows the ID, name, and salary attributes in the relation.
	- We can also impose operations on specific attributes:
		- $\Pi_{ID, name, salary/12} (instructor)$ results in a table where the salary attribute has been divided by 12.
## 2.6.3 Composition of Relational Operations
- The result of a relational operation is itself a relation is important since we can now "chain and compound" our commands.
	- ex:
		- $\Pi_{name}(\sigma_{dept\_name = ''Physics''} (instructor))$ shows the name attribute after selecting dept_name = "Physics" in the instructor relation.
## 2.6.4 The Cartesian-Product Operation ($\times$)
- Combines information from any two relations.   
![[Pasted image 20250912183235.png|250]]
![[Pasted image 20250912183256.png|250]]
![[Pasted image 20250912184126.png|500]]
- Instructor is $r_1$, teaches is $r_2$. It results in concatenating the entirety of $r_2$ for each tuple in $r_1$. Doesn't tell us much.
## 2.6.5 The Join Operation ($\bowtie$)
- Suppose we want to find the information about all instructors together with the *course_id* of all the courses they have taught. The join operation takes advantage of the cartesian product and enforces a requirement on the select.
	- (this really isn't an operation. it's just a composition of select and cartesian product).
	- It basically does $\sigma_{instructor.ID = teaches.ID} (instructor \times teaches)$
![[Pasted image 20250912193917.png|500]]
- $\sigma_{instructor.ID = teaches.ID} (instructor \times teaches)$ is the equivalent of $instructor \bowtie_{instructor.ID = teaches.ID} teaches$.
- Simply put: $\sigma_{\theta} (r_1 \times r_2) \Leftrightarrow r_1 \bowtie_{\theta} r_2$
## 2.6.6 Set Operations
- Like in discrete math, $\cap$ (union), $\cup$ (intersection), $-$ all mean the same thing and act accordingly.
## 2.6.7 The Assignment Operation ($\leftarrow$)
- Works like assignment how assignment typically works in programming languages.
- ex:
	- $var \leftarrow \Pi_{course\_id}(\sigma_{semester = ''Fall'' \wedge year = 2017} (section))$ Assigns the statement to a temporary relation called var.
	- To view it, have var by itself.
## 2.6.8 The Rename Operation ($\rho$)
- Results of relation-algebra expressions do not have a name that we can use to refer to.
- $\rho_x (E)$ returns the result of the expression E under the name x.
- We can also apply $\rho$ onto a relation to rename it.
- ex:
	- $\rho_{x(A_1, A_2, ..., A_n)} (E)$ renames the relation E to x and its attributes to $A_1, A_2, ..., A_n$.
## 2.6.9 Equivalent Queries
- There is often more than one way to write queries in relational algebra.
	- $\sigma_{dept\_name=''Physics''}(instructor \bowtie_{instructor.ID = teaches.ID}teaches)$
	- $(\sigma_{dept\_name=''Physics''}(instructor))\bowtie_{instructor.ID=teaches.ID}teaches$
	- Both are equivalent.