Take notes on chapter 5 to end of 5.1.1
Chapter 5.2 to 5.2.1
Chapter 5.3
Chapters 3 and 4 provided detailed coverage on the basic structures of SQL.
Chapter 5 will cover how procedural code can be executed within the database either by extending SQL to support procedural actions or by allowing functions defined by procedural languages for the database.

# 5.1 Accessing SQL from a Programming Language
- SQL provides a declarative query language with many functions but it's better to have access to a general-purpose programming language for the following reasons:
	- Not all queries can be expressed with SQL. We can embed SQL into languages like Python, C, or Java.
	- Nondeclarative actions such as printing a report or interacting with a user can't be done with SQL.
- Two approaches:
	- Dynamic SQL: Allows the program to construct a SQL query as a character string at runtime, submit it, and then retrieve the result and store into program variables. (Basically what we've used for project 1).
	- Embedded SQL: Also allows programs to interact with a database server. But the SQL statements are identified at compile time which translates these requests into function calls in SQL. 5.1.4 covers it.
## 5.1.1 JDBC
- The JDBC is a standard that defines an API (application program interface) that Java programs can use to connect to database servers.
### 5.1.1.1 Connecting to the Database
![[Pasted image 20251015164945.png|500]]
- `getConnection()`: 
	- first parameter is a string that specifies the URL or machine name of the database and includes specific parameters and port numbers.
	- Second parameter is the database's user identifier (string).
	- Third is a password (string).
### 5.1.1.2 Shipping SQL Statements to the Database System
- Once a connection is open (through `getConnection()`) the program can use it to send SQL statements to the DB for execution.
- Seen in the `conn.CreateStatement()` which is then called in `stmt.<function>`.
- The `Statement` object is an object that allows the Java program to invoke methods that ship SQL statements to execute.
### 5.1.1.3 Exceptions and Resource Management
- Executing any SQL method might throw an exception. So we wrap things in `try... catch` blocks.
- There are `SQLException`, SQL specific, and regular Java `Exception`, native to Java.
- Opening connections, statements, and other JDBC objects all consume DB system resources. Thus programs must close all resources to avoid choking up the server.
	- Thus the preferred method is "try-with-resources".
	- The `getConnection` and `createStatements` all happen in the try block, thus when the try ends or fails, the connections are closed.
### 5.1.1.4 Retrieving the Result of a Query
- A query is executed with `stmt.executeQuery()`. The same function returns the results of the query as a `ResultSet`. The `ResultSet` is kind of a linked list that requires you to call `rset.next()` to access subsequent tuples.
### 5.1.1.5 Prepared Statements


# 5.3 Triggers
- A statement that the database system executed automatically as a side effect of a modification to the database.
- To define, we must:
	- Specify when the trigger executes. Specifically, define an *event* that causes the trigger to be checked, then a *condition* that must be satisfied for the trigger to execute.
	- Upon execution, specify the *actions* that happen as a result.
## 5.3.1 Need for Triggers
- Useful for:
	- Implementing certain integrity constraints that cannot be defined using the constraint mechanisms of SQL.
	- Creating alerts.
	- Automatic task execution upon condition fulfillment.
- Note: Triggers cannot usually perform updates outside the database. Thus changes are only in the database system.
## 5.3.2 Triggers in SQL (Implementation)
- Based on SQL standard, but many DBs implement their own syntax.
```sql
-- #1
CREATE TRIGGER timeslot_check1 AFTER INSERT ON section
REFERENCING NEW ROW AS nrow
FOR EACH ROW
WHEN (nrow.time_slod_id NOT IN (
	  SELECT time_slot_id
	    FROM time_slot)) /* time_slot_id not present in time_slot */
BEGIN /* enters the BEGIN...END if the statement before is TRUE */
	ROLLBACK
END;

CREATE TRIGGER timeslot_check2 AFTER DELETE ON timeslot
REFERENCING OLD ROW AS orow
FOR EACH ROW
WHEN (orow.time_slot_id NOT IN(
	  SELECT time_slot_id
		FROM time_slot) /* last tuple for time_slot_id deleting from time_slot */
	AND orow.timeslot_id IN(
	  SELECT time_slot_id
		FROM section)) /* and time_slot_id still referenced from section */
BEGIN
	ROLLBACK
END;
```
- Shows how triggers can be used to ensure referential integrity on the *time_slot_id* attribute of the *section* relation.
- Order of operation (for #1):
	1. `timeslot_check1` activates after any insert on the *section* relation.
	2. `FOR EACH ROW` (iteration). Every new row to be inserted has to be evaluated by the `WHEN` condition.
	3. `nrow` is created as a reference to the newly inserted row.
	4. `WHEN` is the condition check. In this case, checks if `nrow.time_slot_id` is not in the existing *time_slot* relation.
	5. If the condition evaluates to TRUE, the `BEGIN...END` is entered and `ROLLBACK` occurs.
	6. If the condition evaluates to FALSE, we iterate to the next new row to be inserted.
- `REFERENCING NEW ROW` allows us to refer to new values thrown into the database (insertions, new updated values).
- `REFERENCING OLD ROW` allows us to refer to old values removed from the database (deletes, old values prior to updates)
- 