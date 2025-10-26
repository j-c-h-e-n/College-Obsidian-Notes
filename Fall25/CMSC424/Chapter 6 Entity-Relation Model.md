Sections 6.1 through 6.4 of textbook (6.2 is somewhat redundant with material already covered)
Sections 6.5-6.7 of textbook

**The Database Design part of the class.**
# 6.1 Overview of the Design Process
- Involves the design of the database schema, the programs that access and update data, and the security scheme to control access to the data.
## 6.1.1 Design Phases
- Phases:
	1. Characterize the data needs of the users. Outcome is to gather a specification of user requirements.
	2. (*Conceptual-design* phase) Choose a data model and apply relevant concepts. Outcome is a usually an entity-relationship model which this chapter covers.
		- Ensuring the design fulfills requirements does not create conflicts. Focused on describing data and relationships, not physical storage details.
	3. Designers need to adhere to the *Specification of functional requirements* described by users. This involves design of the operations/transactions imposed on the data.
	4. (*Logical design phase*) Initiating the move from abstract ER model into an implementation enters the *logical design phase* where concepts are mapped into implementation. Usually from the ER model into a relation schema.
	5. (*Physical design phase*) Physical features of the database are specified. This includes the form of file organization and choice of index structures.
## 6.1.2 Design Alternatives
- *Entity*: a "thing" or "object" in the real world that is distinguishable from all other objects.
- When designing a schema we want to avoid two MAJOR pitfalls:
	1. *Redundancy*: A bad design may repeat information. The biggest problem with redundant representation of information "is that the copies of a piece of information can become inconsistent if the information is updated without taking precautions to update all copies of the information" (hard to keep track of all copies, therefore hard to update all).
	2. *Incompleteness*: Bad designs may fail to fully consider all aspects of the requirements. For example, "suppose we have a single relation where we repeat all of the course information once for each section that the course is offered. it would then be impossible to represent information about a new course, unless that course is offered.".
# 6.2 The Entity-Relationship Model
- Designed to facilitate database design by allowing specification of an *enterprise schema* that represents the overall logical structure of a database.
- Maps the meanings and interactions of real-world enterprises onto a conceptual schema.
- Three main basic concepts:
	1. Entity sets.
	2. Relationship sets.
	3. Attributes.
## 6.2.1 Entity Sets
- Entity examples:
	- A person in a university is an entity. Entities have properties, and the values of these properties must UNIQUELY identity the entity.
		- A person may have a *person_id* property that uniquely identifies that person.
- *Entity set*: A set of entities of the same type that share the same properties, or attributes. 
	- For example, the set of all people who are instructors at a given university can be called the entity set instructor.
- We usually refer to the term *entity set* in its abstract form. If we wanted to talk about the actual collection of entities for an entity set we use *extension*. 
	- For example, the set of actual instructors in the university forms the *extension* of the entity set instructor.
- Entities are represented by a set of attributes. *Attributes* are the descriptive properties possessed by each member of an entity set.
- Each entity has a *value* for each of its attributes.
- **Entity sets are represented with a rectangle in the E-R diagram**.
![[Pasted image 20251023221742.png|300]]
- ID is the primary key (underlined).
- The rest are other attributes.
## 6.2.2 Relationship Sets
- *Relationship* is an association among several entities.
- *Relationship instance* represents an association between the named entities in the E-R schema.
![[Pasted image 20251023222102.png|300]]
- We see relationship instances between *instructors* and *students*, particularly Shankar and Katz (the line drawn). Here, we call the relationship set *advisor*.
- **Relationship *sets* are represented with a diamond in the E-R diagram.**
![[Pasted image 20251023222229.png|400]]
- The above example shows the relationship set only showing the connection between two entity sets, but relationship sets can actually denote the association of more than two entity sets.
- Formally, relationship set is defined as: for $n \geq 2$, if $E_1, E_2, ..., E_n$ are entity sets, then relationship set $R \subset \{(e_1, e_2,..., e_n) | e_1 \in E_1, e_2 \in E_2, ..., e_n \in E_n\}$ where $(e_1, ..., e_n)$ is a relationship instance.
	- Additionally, association between entity sets is called *participation*. $E_1, E_2, ..., E_n$ *participate* in a relationship set $R$.
- The functions that an entity plays in a relationship is called their *role*.
	- Roles are implicit and not usually specified. However, useful when there needs to be clarification. Especially when the same entity set participates in a relationship set more than once, in different roles.
- Relationships where entity sets participate more than once are called *recursive* relationship sets. These need explicit role names for clarity.
	- Ex: Entity set *course* records information about all courses offered at a uni. To depict the situation where one course, $C2$, is a prerequisite for another, $C1$, we have relationship set *prereq* that is modeled by ordered pairs of *course* entities. The first entry in the pair is the course to be taken, and the second is the prerequisite, $(C1, C2)$. 
![[Pasted image 20251024191224.png|400]]
- A relationship can also have attributes called *descriptive attributes*. 
	- For example, the relationship set *takes* relates the tables *students* and *section*. A descriptive attribute for the relationship set could be *grade* which the student received in a course offering.
- *Descriptive attributes* are represented by an **undivided rectangle** in the E-R diagram linked by dashed lines.
![[Pasted image 20251024193112.png|400]]
- It is possible to have more than one relationship set involving the same entity sets.
	- Students may be TAs for a course. Thus the entity sets *section* and *student* may participate in a relationship set *teaching_assistant*, in addition to participating in the *takes* relationship set.
- The relationship sets *advisor* and *takes* provide examples of a *binary relationship set* (of degree 2). *Ternary relationship sets* are of degree 3.
![[Pasted image 20251024194830.png|400]]
# 6.3 Complex Attributes
- For each attribute, they have permitted values called the *domain* or *value set*.
	- The domain of attribute *course_id* might be the set of all text strings of a certain length.
	- The domain of attribute *semester* might be strings from the set Fall, Winter, Spring, and Summer.
- An attribute can be considered *simple* or *composite*.
- Simple attributes:
	- Atomic, cannot be divided into subparts.
- Composite attributes:
	- Can be divided into subparts. 
		- For example, the attribute *name* could be divided into *first_name* and *last_name*.
		- The attribute *address* could be divided into *street*, *city*, *state*, and *postal_code*.
- Single valued attributes:
	- For example, the *student_ID* attribute for a specific student entity refers to only one student ID, thus a single-valued attribute.
- Multivalued attributes:
	- For example, people may have multiple phone numbers. So the *phone_number* attribute could have zero, one, or several values.
- Derived attributes:
	- Values that can be derived from the values of other attributes/entities.
		- For example, let the *instructor* relation have an attribute called *students_advised*. Since each instructor has a relationship set with the *student*relation, we can count the number of *students* associated with them.
		- The age of a person can be derived from the current day and their *date_of_birth* attributes. *date_of_birth* could be considered as a *base* or *stored* attribute.
![[Pasted image 20251025175041.png|300]]
- Nested entries are composite attributes.
- { ___ } are multivalued.
- () are derived.
# 6.4 Mapping Cardinalities
- aka cardinality ratios, express the number of entities to which another entity can be associated via a relationship set.
- Most useful in describing binary relationship sets.
	- Can describe relationship sets that involve more than two entity sets.
- For binary relationship set $R$ between entity sets $A$ and $B$, the mapping cardinality must be:
	- One-to-One: An entity in $A$ is associated with at most one entity in $B$, and vice versa.
		- ![[Pasted image 20251025193407.png|200]]![[Pasted image 20251025193508.png|300]]
	- One-to-Many: An entity in $A$ is associated with any number (zero or more) of entities in $B$. However, an entity in $B$ can only be associated with at most one entity in $A$.
		- ![[Pasted image 20251025195052.png|200]]![[Pasted image 20251025195107.png|300]]
	- Many-to-One: An entity in $A$ is associated with at most one entity in $B$. However, an entity in $B$ can be associated with any number of entities in $A$.
		- ![[Pasted image 20251025193428.png|203]]![[Pasted image 20251025195301.png|300]]
	- Many-to-Many: An entity in $A$ is associated with any number of entities in $B$, and vice versa.
		- ![[Pasted image 20251025195638.png|200]]![[Pasted image 20251025195650.png|300]]
- Participation between entity sets can also be expressed with the lines drawn in the E-R diagram.
	- If every entity in an entity set participates in a relationship set, then it has total participation.
	- If not every entity in an entity set is in a relationship, then it has partial participation.
	- ![[Pasted image 20251025200238.png]]
- We can also show cardinality limits on relationships. These are denoted $l..h$ where $l$ is the minimum participation, where an entity must have at least $l$ relationships. $h$ is the maximum participation where an entity can only have at most $h$ relationships. \* denotes that there is no limit (upper bound).
	- $0..*$ combined with $1..1$ represents a many-to-one relationship.
# 6.5 Primary Key
- A way to specify how entities within a given entity set and relationships within a given relationship set are distinguished.
## 6.5.1 Entity Sets
- Conceptually, individual entities are distinct.
- But their differences must be expressed in terms of their attributes in a database.
- Therefore, the values of the attribute values must be such that they can uniquely identify the entity.
- The concepts of superkey, candidate key, and primary key are applicable to entity sets just as they are to relation schemas.
	- Review:
		- Superkey: The set of all key combinations that can uniquely identify tuples. Can contain nulls.
		- Candidate key: The simplest superkey (smallest, contains the least attributes). May contain nulls but in practice they should never.
		- Primary key: The candidate key chosen by the developer to represent the relation.
## 6.5.2 Relationship Sets
- Used to distinguish relationships of a relationship set.
- Let $R$ be a relationship set involving entity sets $E_1, E_2, ..., E_n$. Let primary-key($E_i$) denote the set of attributes that forms the primary key for entity set $E_i$. Assume attribute names of all primary keys are unique. The composition of the primary key for a relationship set depends on the set of attributes associated with the relationship set $R$.
	- But if a relationship set does not have attributes, then primary-key($E_1$) $\cup$ primary-key($E_2$) $\cup ... \cup$ primary-key($E_n$) is a superkey that describes an individual relationship in set $R$.
	- If a relationship set has attributes $a_1, a_2, ..., a_m$ then it's everything if it didn't have attributes, and all of the attributes appended: primary-key($E_1$) $\cup$ primary-key($E_2$) $\cup ... \cup$ primary-key($E_n$) $\cup \{a_1, a_2, ..., a_m\}$ is a superkey that describes an individual relationship in set $R$.
- If attribute names of primary keys are not unique across entity sets then the attributes are renamed. 
	- Can have an entity set's attribute renamed to the entity set's name + attribute name.
- Many-to-many relationships have its primary keys as the union of the primary keys of connected entity sets.
- One-to-many and many-to-one relationships have the primary keys of the "many" side as the relationship set's primary key.
- One-to-one relationships can use either participating entity set's primary keys as the chosen primary key.
- Nonbinary relationships have primary-key($E_1$) $\cup$ primary-key($E_2$) $\cup ... \cup$ primary-key($E_n$) has the only candidate key if there are no cardinality constraints. If they are present, it is much more complicated.
	- ![[Pasted image 20251025230934.png]]
## 6.5.3 Weak Entity Sets
- Consider the *section* entity: *section(course_id, semester, year, sec_id*).
- Consider the relationship set *sec_course* between entity sets section and course.
- Consider that the information in *sec_course* is redundant since *section* already has *course_id*.
	- We can remove this redundancy by removing the relationship. Not ideal since the relationship between section and course becomes implicit.
	- We can choose to not store the attribute *course_id* in the *section* entity and only store the rest instead. However, this makes it so that *section* will not have a valid primary key (unable to identify individual entities distinctly).
		- We can treat the relationship *sec_course* as a special relationship that provides extra information, notably *course_id*, to identify *section* entities uniquely.
- "A **weak entity set** is one whose existence is dependent on another entity set, called its **identifying entity set**;" (a weak entity set and identifying entity set are an inseparable pair. The identifying entity set owns the weak entity set.)
	- "Instead of associating a primary key with a weak entity, we use the primary key of the identifying entity, along with extra attributes, called **discriminator attributes** to uniquely identify a weak entity. An entity set that is not a weak entity is termed a **strong entity set**."
- This relationship between the identifying and weak entity sets is called the *identifying relationship*.
- The identifying relationship should not have any descriptive attributes. These descriptive attributes should be associated with the weak entity set.
- A weak entity set is represented with a double rectangle. Its discriminators are underlined with a dashed line. The relationship set connecting to the weak entity set is a double diamond. The line is also doubled but that is because a weak entity set must have total participation in its identifying relationship set, and it is a many-to-one relation from identifying to weak.
	- ![[Pasted image 20251025235749.png|400]]
- Additional information:
	- Weak entity sets can participate in other relationships.
	- Weak entity sets can be an identifying set to another weak entity set.
	- Weak entity sets can have multiple identifying sets.
# 6.6 Removing Redundant Attributes in Entity Sets

# 6.7 Reducing E-R Diagrams to Relational Schemas