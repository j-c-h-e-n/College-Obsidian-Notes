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

# 4.4 Integrity Constraints
