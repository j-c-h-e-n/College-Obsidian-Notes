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