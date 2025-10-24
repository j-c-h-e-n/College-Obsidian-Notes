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
	5. (*Physical design phase*)