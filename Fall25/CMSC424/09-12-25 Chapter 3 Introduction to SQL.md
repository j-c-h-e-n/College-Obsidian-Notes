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
	- If an attribute was defined as char(10) and a string "Hi" was stored, 8 blank spaces will be appended to make it 10 characters.
- varchar(n): A variable-length character string with user-specified maximum length n. The full form, character varying, is equivalent.
- int: An integer (a finite subset of the integers that is machine dependent). The full form, integer, is equivalent.
- smallint: A small integer (a machine-dependent subset of the integer type).
- numeric(p, d): A fixed-point number with user-specified precision. The number consists of p digits (plus a sign), and d of the p digits are to the right of the decimal point. Thus, numeric(3, 1) allows 44.5 to be stored exactly, but neither 444.5 nor 0.32 can be stored exactly in a field of this type.
- real, double precision: Floating-point and double-precision floating-point numbers with machine-dependent precision.
- float(n): A floating-point number with precision of at least n digits.
- More types defined in Ch 4.5
- Each type may also include a special value called *null*, the absence of data.
## 3.2.2 Basic Schema Definition
- SQL relations are defined using the `create table` command.
```SQL
create table department(
	dept_name varchar(20),
	building   varchar(15),
	budget     numeric(12,2),
	primary key (dept_name)
);
```
- Creates a relation called department with 3 attributes.
- General form:
	- ![[Pasted image 20250914164603.png|200]]
- Integrity-constraints:
	- Primary key: `primary key (A1, A2, A3, ..., AN)` states that attributes A1, ..., AN form the primary keys for the relation. They must be non-null and unique.
	- Foreign key: `foreign key (A1, A2, A3, ..., AN) references s` states that the values of the attributes must correspond to values of the same attributes in another relation s.
	- Not null: not formatted in the normal integrity constraint location. Appended at the end of each attribute definition to enforce that the values for those attributes cannot be null.
- `drop table r;` removes the relation r from the database.
- `delete from r;` retains the relation r but removes all tuples (rows) from the relation, including its attributes (schema).
- `alter table`: command that allows us to alter a relation's values.
	- `alter table r add A D;` adds attribute A with definitions D to table r.
	- `alter table r drop A;` removes attribute A (the entire column) from table r.
# 3.3 Basic Structure of SQL Queries
Basic structure consists of `select`, `from`, and `where`.
## 3.3.1 Queries on a Single Relation
- "Find the names of all instructors":
	- `select name from instructor;`
- "Find all unique names of all instructors":
	- `select distinct name from instructor;`, keyword "distinct".
- "Ensure all names, including duplicates, are listed":
	- `select all name from instructor;`
- `select` can also use arithmetic.
	- `select ID, name, dept_name, salary*1.1, from instructor;`
- "Find the names of all instructors in the Computer Science department who have salary greater than $70,000"
	- `select name from instructor where dept_name = 'Comp. Sci.' and salary > 70000;`
## 3.3.2 Queries on Multiple Relations
```SQL
select name, instructor.dept_name, building
from instructor, department
where instructor.dept_name = department.dept_name;
```
- Selects the name, instructor.dept_name, and building from instructor and department relations under the condition that the instrcutor.dept_name = department.dept_name in the tuple.
- There's also nested queries but they evaluate outside-in but kind of recursively, bubbling the results up to the highest level.
# 3.4 Additional Basic Operations
## 3.4.1 The Rename Operation (as)
- Allows us to rename the attributes of a result relation when we use `select`.
	- Handles the niche case of when we use an arithmetic expression in the select clause, since the resulting relation has no name.
```SQL
select name as instructor_name, course_id
from instructor, teaches
where instructor.ID = teaches.ID
```
- Replaces the attribute `name` with `instructor_name`.
```SQL
select T.name, S.course_id
from instructor as T, teaches as S
where T.ID = S.ID
```
- Replaces instructor as T and teaches as S and allows the reference of these relations as T and S.
	- T and S are known as a "correlation name" in SQL. Also known as "table alias", "correlation variable", or "tuple variable".
- We can also run these without the `as` keyword.
## 3.4.2 String Operations
- Strings in SQL are denoted with single quotes.
	- 'Computer'
- Single quote characters that are part of the string can be specified using *two*single quotes.
	- 'It''s right'
- The SQL standard states that strings are case sensitive, but some implementations aren't. Totally up to system settings.
- Functions:
	- String concatenation uses ||.
	- Convert to upper: **upper**(s).
	- Convert to lower: **lower**(s).
	- String concatenation with concat('string', 'string').
- Pattern matching using the `like` operator:
	- `%` matches any substring.
		- `Intro%` matches any string beginning with 'Intro'.
	- `_` matches single characters.
		- `___` matches strings of exactly 3 characters.
	- If your string contains the `%` and `_` special characters you can use the `escape` keyword to define an escape character.
		- `like 'ab\%cd%' escape '\'` matches all strings beginning with "ab%cd". Note % is captured by the defined escape symbol.
## 3.4.3 Attribute Specification in the Select Clause (\*)
- The `*` symbol selects all attributes.
```SQL
select intructor.*
from instructor, teaches
where indstructor.ID = teaches.iD
```
## 3.4.4 Ordering the Display of Tuples (order by)
```SQL
select name
from instructor
where dept_name = 'Physics'
order by salary desc, name asc;
```
- The order by operation here sorts the name column items in descending order by salary. If there is collision, they are sorted in ascending order by name.
## 3.4.5 Where-Clause Predicates
- We can use `between`, and `not between` operators for specificity.
```SQL
select name
from instructor
where salary between 90000 and 100000;
```
Is better than:
```SQL
select name
from instructor
where salary <= 100000 and salary >= 90000;
```
# 3.5 Set Operations
- SQL provides `union`, `intersect`, and `except` operations for sets.
## 3.5.1 The Union Operation (union)
```SQL
(select course_id
from section
where semester = 'Fall' and year=2017)
union
(select course_id
from section
where semester = 'Spring' and year=2018);
```
- Does the equivalent of just a union between two sets.
## 3.5.2 The Intersect Operation (intersect)
```SQL
(select course_id
from section
where semester = 'Fall' and year = 2017)
intersect
(select course_id
from section
where semester = 'Spring' and year = 2018);
```
- Does the equivalent of an intersection between two sets. Only returns elements that appear in both sets.
## 3.5.3 The Except Operation (except)
```SQL
(select course_id
from section
where semester = 'Fall' and year = 2017)
except
(select course_id
from section
where semester = 'Spring' and year = 2018);
```
- Does the equivalent to set difference. Only returns elements that are in the first input and not in the second input.
# 3.6 Null Values
- Special value but introduces some headaches.
- The result of any arithmetic expression with null is always null.
- "unknown" is the truth value of null. So they are basically the same but you use "unknown" in boolean evaluations.
- The result of any comparison operator (<, >, =) with null/unknown is always unknown.
- The result of boolean operations with null/unknown is also unknown.
	- Except for the `or` case. `true or unknown` is unknown
# 3.7 Aggregate Functions
- SQL contains 5 built-in functions:
	- Average: avg
		- Minimum: min
	- Maximum: max
	- Total: sum
	- Count: count
## 3.7.1 Basic Aggregation
```sql
select avg(salary) as avg_salary
from instructor
where dept_name = 'Comp. Sci.';
```
- Calculates the average salary for CS professors and renames the relation as "avg_salary".
```sql
select count (distinct ID)
from teaches
where semester = 'Spring' and year = 2018;
```
- Counts the number of distinct IDs from the teaches relation where the semester attribute is 'Spring' and the year is 2018.
- SQL does not allow the use of the distinct keyword with count (\*). 
## 3.7.2 Aggregation with Grouping
- Previous aggregation techniques only involved single tuple operations.
- We can perform aggregation on groups using the `group by` clause, placing all tuples that match the condition into groups.
- A standard group by dept_name on the instructor relation:
	- ![[Pasted image 20250922115046.png|200]]
-  The following finds the average salary in each department.
```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name;
```
- ![[Pasted image 20250922115339.png|200]]
- The next query causes an error since ID is not present in the resulting group by dept_name:
	- ```sql
	  select dept_name, ID, avg(salary)
	  from instructor
	  group by dept_name;
	  ```
## 3.7.3 The Having Clause
- A clause that applies to groups rather than tuples.
```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name
having avg(salary) > 42000;
```
![[Pasted image 20250922121123.png|200]]
- This only shows departments with an average salary > 42000. Notice how Music is no longer in the resulting relation.
### 3.7.3.1 Order of Operations
1. `from`
2. `where` condition is applied into the `from`
3. Resulting tuples are then filtered through `group by`. If not applicable, resulting tuples from `where` are treated as one group.
4. `having` is then applied to each group. Groups not satisfying the condition are removed.
5. `select` finally runs, generates the resulting relation.
```sql
select course_id, semester, year, sec_Id, avg(tot_cred)
from student, takes
where student.ID = takes.ID and year = 2017
group by course_id, semester, year, sec_id
having count(ID) >= 2;
```
## 3.7.4 Aggregation with Null and Boolean Values
- The SQL standard has certain operations that ignore nulls.
	- All aggregate functions except `count (*)` ignores nulls.
- Functions/queries that handle booleans:
	- `some` computes the disjunction (or)
	- `every` computes the conjunction (and)
# 3.8 Nested Subqueries
- A subquery is a `select`-`from`-`where` expression within another query.
## 3.8.1 Set Membership
- `in` tests for set membership
- `not in` tests for absence of set membership
```sql
-- subquery --
/* This subquery gets course ids from the section relation where semester = 'Spring' and year = 2018 */
(select course_id)
 from section
 where semester = 'Spring' and year = 2018)
-- main query --
/* This uses the subquery to then extract distinct course_id from teh section relation where semester = Fall, year = 2017, and course_id is located in the returned subquery relation. */
select distinct course_id
from section
where semester = 'Fall' and year = 2017 and
	course_id in (select course_id
				  from section
				  where semester = 'Spring' and year = 2018);
```
## 3.8.2 Set Comparison
- "Greater than at least one" is `> some` in SQL.
- `=some` is IDENTIFICAL to `in` while `<>some` is NOT the same as `not in`.
- "Greater than all" is `>all`.
	- Also allows `<, <=, >, >=, <> all`.
	- `<>all` is identical to `not in`, but `=all` is not the same as `in`.
## 3.8.3 Test for Empty Relations
- We can test if subqueries return any results.
- `exists` returns the value `true` if the subquery is nonempty.
	- If false, nothing happens (does it raise an error/warning?)
- `not exists` returns true if the subquery is empty.
	- We can use this to check set containment: "relation A contains relation B" as `not exists (B except A)`
- "Find all students who have taken all courses offered in the Biology department"
```sql
select S.ID, S.name
from student as S
where not exists ((select course_id
				   from course
				   where dept_name = 'Biology')
				   except
				   (select T.course_id
				   from takes as T
				   where S.ID = T.ID));
```
- The `S` in the subquery is called a *correlation name* and the subquery is called a *correlated subquery*.
- Another example:
```sql
select course_id
from section as S
where semester = 'Fall' and year = 2017 and
	exists (select *
			from section as T
			where semester = 'Spring' and year = 2018 and
				S.course_id = T.course_id);
```
- The `S` called in the subquery used in the `where` clause is called a *correlation name* since S was defined in the above query.
- The subquery that used the *correlation name* is called a *correlated subquery*.
- Scoping rules are similar to those of programming languages.
## 3.8.6 The With Clause
- Allows the definition of a temporary relation whose definition is available only to the query in which the `with` clause occurs.
```sql
with max_budget (value) as
	(select max(budget)
	from department)
select budget
from department, max_budget
where department.budget = max_budget.value;
```
- I like to think of this as the queries as functions, and the `with` clause is just defining a variable within the scope of that function.
- We can re-assign sub-queries into `with` temporary relations to reduce the levels of nesting in our queries and promote readability.
## 3.8.7 Scalar Subqueries
- We can throw in subqueries wherever an expression returning a value is permitted.
- This allows us to create subqueries in the `select` clause.
```sql
select dept_name,
	(select count(*)
	 from instructor
	 where department.dept_name = instructor.dept_name)
	as num_instructors
from department;
```
- `department.dept_name` is also a correlation variable.
- Scalar subqueries can appear in `select`, `where`, and `having` clauses. Can also be defined without aggregates.
- One more note: It's not always possible to figure out at compile time if a subquery can return more than one tuple in its result. Thus if the result has more than one tuple, a run-time error occurs. 
- The scalar subquery is TECHNICALLY a relation, but it's just a relation of one tuple.
## 3.8.8 Scalar Without a From Clause
- There are queries that require a calculation but no reference to any relation. 
- "Suppose we wish to find the *average number of sections taught per instructor*, with sections taught by multiple instructors counted once per instructor."
	- `(select count(*) from teaches) / (select count(*) from instructor);`
	- Except this previous query is illegal in some systems due to a lack of a `from` clause.
- We can use a "dummy" relation to contain the result.
	- `select (select count(*) from teaches) / (select count(*) from instructor) from dual;`
	- We use a dummy relation called `dual` to store the result, making this valid.
# 3.9 Modification of the Database
Going over how we ad, remove, or change information in SQL.
## 3.9.1 Deletion
- `delete from r where P;`
- P is the predicate or condition and r is the relation.
- Finds all tuples (t) in r for which P(t) is true, then removes them.
- If `where` is omitted, then `delete from r` removes all tuples, not attributes.
	- `delete from instructor where salary between 13000 and 15000;` removes tuples from instructor where salaries are between 13000 and 15000.
	- `delete from instructor where dept_name in (select dept_name from department where building = 'Watson');` deletes dept_names from the instructor relation if they match with dept_names that come from the department relation where building = 'Watson'.
	- `delete from instructor where salary < (select avg(salary) from instructor);`
## 3.9.2 Insertion
- `insert into course values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);` Inserts one tuple into the course relation.
- `insert into course(course_id, title, dept_name, credits) values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);` same as before just specified the relation schema.
```SQL
insert into instructor
	select ID, name, dept_Name, 18000
	from student
	where dept_name = 'Music' and tot_cred > 144;
```
- This inserts the ID, name, dept_name, and 18000 from tuples in student where dept_name = 'Music' and tot_cred > 144 into the instructor relation. Converting these students into instructors.
## 3.9.3 Updates
If we want to change a value in a tuple without changing all values in the tuple, we use the update statement.
- `update instructor set salary=salary*1.05;` updates all salaries by 1.05.
![[Pasted image 20250914195315.png|250]]
