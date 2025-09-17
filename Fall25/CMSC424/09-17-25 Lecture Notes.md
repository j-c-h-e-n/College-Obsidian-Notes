# Aggregates: Group By
Splits the tuples into groups and compute the aggregates for each group.
```SQL
select dept_name, avg(salary)
from instructor
group by dept_name;
```
- Very similar to DataFrame group by.
- The avg(salary) is the aggregate portion which is the attached column to dept_name in the returning relation.
# Nested Sub-Queries
```SQL
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in
		(select course_id, sec_id, semester, year
		from teaches
		where teaches.ID=10101);
```
## Multi-comparisons
- "What if we want to compare a value with multiple other values?"
- Uses the `in` keyword, `select * from table where val IN (attribute from other table)`.

## Popular WHERE-clauses
- `in`
- `not in`
- `exists`
- `not exists`
- `some`
	- rough translation to python, ideologically equivalent to comparing if a value is of some relation to a set of elements.
		- ex: 5 < some (0, 5, 6) = true since 5 < 6.
- `all`

```sql
select distinct T.name
from instructor T, instructor S
where T.salary > S.salary and S.dept_name = 'Biology'
```
not great. Use this one instead:
```SQL
select name
from instructor
where salary > some(select salary 
					from instructor
					where dept_name = 'Biology');
```
