Reviewed the some clause
Introduced the with clause

## Joins
- Natural join removes those without matching attributes - i.e. you lose information.
- Left outer join ($\sqsupset\bowtie$) will join tuples that "don't join". It just replaces the missing values with nulls
	- ```sql
	  select * from instructor natural left outer join teaches
	  ```
	- Guarantees a result for every tuple from the left relation.
- Right outer join ($\bowtie\sqsubset$) will also join tuples that "don't join" and replaces missing values with nulls. Guarantees a result for every tuple in the right relation.
- Full outer join returns the union of the left outer join and the right outer join results
	- ```sql
	  select * from instructor natural full outer join teaches
	  ```
## Correlated Sub-Queries
It's like a double for loop
```sql
select * 
from AAFlightCounts as fc1
where not exists
	/* inner "for loop" where we match every fc2 tuple 
	to an fc1 tuple, sequentially. */
	(select * 
	 from AAFlightCounts as fc2
	 where fc2.count between fc1.count - 2
							 and fc1.count + 2
		and fc1.flightid != fc2.flightid);
```
