# Views
- Relations that are not defined in conceptual model but made visible to the user as a "virtual relation".
- 3 uses:
	- Expression in the "with" clause that you want to reuse.
	- Backwards compatibility
		- DBA wants to change the schema but some applications still want the old schema.
	- Security/authorization
		- Don't want to give user access to entire table, just subset.
## Definition
- `CREATE VIEW v AS <query expression>`
- `CREATE VIEW v <colnames> as <query expression>`
- Once a view has been defined, you can address and reuse it by calling its view name `v`.
- NOT the same as creating a new relation by evaluating the query expression (i.e. does not store a relation you can refer to).
	- The expression is just saved and substituted into queries when you call `v`.
	- Some DBs will create a "materialized view" which does save the results.
- Akin to a function(?).
## Updating Views
```sql
create view cust_list(customerid) as 
	(select customerid
	from flewon
	where flightid = 'DL119')
```
- `insert into cust_list values ('cust263')`
	- View does not exist independently, so system will try to insert into the relations from which this view is derived.
	- It will fail due to a foreign-key/primary key dependency in `flewon` which links it to the `customers` relation.
- `insert into cust_list values ('cust63')`
	- This case works since 'cust63' exists already. But `nulls` are filled into the other fields of `flewon`.
- These updates are extremely hard to do and track, so many databases usually have these result in an error.
# Functions and Procedures
- Prof REALLY doesn't want us to use procedures if we can help it. But they are unavoidable.
- Most database systems enable user defined functions and procedures to be written in a DB-specific procedural language.
```sql
CREATE or REPLACE FUNCTION how_many_consec(cust varchar(10))
RETURNS integer AS $$
DeCLARE
	rc RECORD;
	cnt int;
	prev date;
BEGIN
	prev = null;
	count = 0;
	-- Iterate through every row from flewon for this customer
	FOR rc IN SELECT * 
				FROM flewon
			   WHERE customerid = cust 
			   ORDER BY flightdate LOOP
		IF prev is not null THEN
			IF prev + interval '1 day' = rc.flightdate THEN
				cnt = cnt + 1;
			END IF;
		END IF;
		prev = rc.flightdate;
	END LOOP;
	RETURN cnt;
END;
$$ LANGUAGE plpgsql;
```
- Optimizer hates procedural code.
	- It MUST do what you say.
	- Cannot optimize it.
- Ideally use something like Python to interface instead.
- Maintainability and migration to other DB services greatly suffers if you use plpgsql.
## Usage
- `select how_many_consec('cust100');`
- `select customerid, how_many_consec(customerid) from flewon group by customerid;`
- `update customers set consec_days = how_many_consec(customerid);`
# Triggers
- A statement that is executed automatically by the system as a side effect of a modification to the database.
	- Can choose to run trigger code before or after modification.
- Good for
	- Auto data cleaning.
	- Updating derived tables/columns
	- Performing external world actions as a result of the modified DB.
- Most systems have their own syntax.
- Be careful of cascading triggers, infinite sequences, etc.
## Example
```sql
CREATE TRIGGER update_consec_days
	   AFTER INSERT 
	   OR UPDATE 
	   OR DELETE
	   FOR EACH ROW
	   EXECUTE PROCEDURE update_consec_days();
```
# Transactions
- Unit of work
- Atomic transaction
	- *Either fully executed or rolled back as if it never occurred.*
- Isolation from concurrent transactions
- Transactions begin implicitly
	- Ended by commit work or rollback work.
- But default on most databases: each SQL statement commits automatically.
	- Can turn off auto commit of ra session (e.g. using API)
	- In SQL: 1999, can use: `begin atomic .... end`
		- Not supported on many systems.
		- 