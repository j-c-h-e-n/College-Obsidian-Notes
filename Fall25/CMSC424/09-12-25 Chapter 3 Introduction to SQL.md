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
- 
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
