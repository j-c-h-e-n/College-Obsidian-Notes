#  Introduction
>A database-management system (DBMS) is a collection of interrelated data and a set of programs to access those data.

- The collection of data is called a database. 
- The goal of a DBMS is to provide a way to store and retrieve information in a convenient and efficient way.
- Databases are responsible for:
	- Storing massive amounts of information.
	- Defining structures for information storage.
	- Providing methods to manipulate information.
	- Ensuring the safety of the information stored (encryption, security, backups).
	- Ensuring reliability of the information stored.

## 1.1 Database-System Applications
- Earliest systems arose in 1960s.
- Data/information is king in the modern age.
- Databases manage information that are:
	- Highly valuable.
	- Relatively large.
	- Accessed by multiple users and applications, often at the same time.
- Modern databases heavily rely on abstraction to be able to store information with varying structures.

## 1.2 Purpose of Database Systems
Listed examples:
- To help with data redundancy and inconsistency.
- To ease data access.
- To keep data together and not isolated.
- Ensuring data integrity.
- Keeping redundancy for situations of failure (restore points, backups, etc).
- Ensure reliable concurrent access.
- Keeping security.

## 1.3 View of Data
Databases provide users an abstract view of the data, hiding complexities and nuanced details.
### 1.3.1 Data Models
- Relational Model:
	- Uses tables to represent data and the relationships amongst the data.
	- Columns are unique.
	- Tables, aka relations.
	- Think CSV, DataFrame.
	- Relational models are an example of a record type database.
		- Fixed formats of several types. Each table contains records of a particular type.
		- One of the most widely used database models.
- Entity-Relationship (E-R) Model:
	- Uses a collection of basic objects called *entities* and maps *relationships* between these entities.
	- Widely used.
- Semi-structured Data Model:
	- Individual data items of the same type may have different sets of attributes.
	- JSON and XML are semi-structured data representations.
- Object-Based Data Model:
	- Objects are already well integrated into relational databases.
	- Antiquated method that was developed due to the proliferation of object-oriented programming.

### 1.3.2 Relational Data Model
- Same as above

### 1.3.3 Data Abstraction
- Used to simplify user's interactions with the system by hiding complexities that they don't need to know about.

### 1.3.4 Instances and Schemas
- 
