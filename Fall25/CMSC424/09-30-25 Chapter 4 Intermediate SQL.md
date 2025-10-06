# 4.1 Join Expressions
## 4.1.1 Natural Join
- Operates on two relations and produces a relation as the result.
- The cartesian product of two relations concatenates each tuple of the first relation with every tuple of the second.
- Natural join considers only those pairs of tuples with the same value on those attributes that appear in the schema of both relations.
	- If relation one has "yes", "no", "maybe" and relation two has "yes", "no", "absolutely", a natural join will only combine the relations on the tuples where "yes" and "no" have the same value.
- Also known as "natural inner joins". These do not preserve nonmatched tuples.
## 4.1.2 Join Conditions
- Also known as "inner joins". These do not preserve nonmatched tuples.
- `select * from <r1> join <r2> on <condition>`
- equivalent to `select * from <r1>, <r2> where <condition>`
## 4.1.3 Outer Joins
- Joins naturally remove unmatched tuples.
- Outer joins will include all of them, but substitute in Null for missing values. Preserves nonmatched tuples.
- `left outer join`:
	- Preserves tuples only in the relation named before the `left outer join` operation.
- `right outer join`:
	- Preserves tuples only in the relation named after the `right outer join` operation.
- `full outer join`:
	- Preserves tuples in both relations.
# 4.2 Views
## 4.2.1 View Definition
- `CREATE VIEW v AS <query expression>`
- `CREATE VIEW v <colnames> as <query expression>`
- Once a view has been defined, you can address and reuse it by calling its view name `v`.
- NOT the same as creating a new relation by evaluating the query expression (i.e. does not store a relation you can refer to).
	- The expression is just saved and substituted into queries when you call `v`.
	- Some DBs will create a "materialized view" which does save the results.
- Akin to a function(?).
## 4.2.3 Materialized Views
- Some DBs will allow view relations to be stored.
	- But to support this they need to ensure that this view is also kept up to date.
- This process of updating is called "view maintenance".
	- Some DBs will "lazily" update by only updating when the view is accessed.
	- Some will periodically update on defined intervals.
## 4.2.4 Update of a View
- Updates to a view must be translated to a modification to the actual relations in the logical model of the database (foreign keys, etc).
- Modifications are generally not allowed on view relations due to the complexity involved.
- A view is said to be "updatable" if:
	- `from` only has one database relation.
	- `select` only contains attribute names of the relation and does not have any expressions, aggregates, or distinct specification.
	- Any attribute not listed in `select` can be set to `null`. i.e. none of the attributes have a `not null` constraint, AND is not part of a primary key.
	- The query doesn't have a `group by` or `having` clause.
- Only the simplest of views are updatable.
# 4.3 Transactions
- Transaction: A sequence of query and/or update statements.
- One of these statements must end the transaction:
	- `Commit work` commits the current transaction: Makes the updates in the transaction permanent.
	- `Rollback work` causes all updates in the transaction to be reverted back to an original state.
- Once committed, a rollback cannot be applied.
- Transactions are defined as *atomic*: Either everything applies, or if something fails then everything fails (is removed).
- In many implementations, each SQL statement is a transaction and is committed as soon as it is executed.
# 4.4 Integrity Constraints
- Ensures that changes made to the database do not result in a loss of data consistency.
- Contrasts with *security constraints* which deals with user authorizations.
- Examples:
	- An instructor name cannot be *null*.
	- No two instructors can have the same ID.
	- Every department name in the *course* relation must have a matching department name in the *department* relation.
	- The budget of a department must be greater than $0.
- Chapter 7 will introduce *functional dependencies*.
- Integrity Constraints are usually defined in the database schema design process, part of the `create table` command.
- But they can be added using `alter table <table> add <constraint>` as well.
	- Verifies the relation FIRST. If the relation does not satisfy the constraint, it is rejected.
- 