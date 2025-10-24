More E/R Diagrams and their link to Relations.
- We need a table for many-to-many relations to keep track of primary keys and links between relations.
- We do not need specific tables for many-to-one or one-to-many relation connections, nor one-to-one connections.

# Normalization
- "What is the definition of a good schema."
- Lots of different choices for a schema:
	- How do we decided between these choices?
	- If we are creating from an ER diagram, how do we make the right choices?
- We typically try to avoid sets or lists (reference to the movie stuff and stars in slides):
	- Hard to represent.
	- Hard to query.
- Maximizing decomposition of your database isn't always good. 
	- *Decomposition*: reducing databases down to a primary key and a minimal attribute pairing. Ex: The relation {Name, LawyerID, Address} with name as primary key, is decomposed into two relations: {Name, LawyerID} and {Name, Address}. 
	- Requires a significant number of joins to extract meaningful data. Joins are computationally expensive.
## Traits of a Good Schema:
- No sets
- Correct and faithful to the original design.
	- Avoid lossy decompositions.
- As little redundancy as possible.
	- Avoid potential anomalies.
- No "inability to represent information"
	- Nulls shouldn't be required to store information.
- Dependency preservation.
	- Should be possible to check for constraints.

- Sometimes not always possible.
	- We often relax these guidelines for simpler schemas and fewer joins during querying.
## Approach:
1. We will encode and list all our knowledge about the schema.
	- Functional dependencies (FDs):
		- $a1 \rightarrow a2$  (a1 implies a2).
		- Example: 
			- SSN implies name. 
			- Movie title does not imply length.
			- Movie title and movie year may imply length.
2. Define a set of rules that the schema must follow to be considered good.
	- "Normal forms": 1NF, 2NF, 4NF, BCNF, 4NF...
	- A normal form specifies constraints on the schemas and FDs. **We will focus on BCNF.**
3. If not in a "normal form", we modify the schema.

#### Definitions:
- A *relation instance* r *satisfies* a set of functional dependencies, *F*, if the FDs hold on the relation.
- *F* *holds on* a relation schema *R* if no legal (allowable) relation instance of *R* violates it.
- A functional dependency, $A \rightarrow B$, is called *trivial* if:
	- $B$ is a subset of $A$.
	- e.g. {SSN, Name} implies Name. Trivial.

# Using FDs to Create Good Schemas
- So far all of our FDs are single valued dependencies. There are also multi-valued dependencies.
## BCNF: Boyce-Codd Normal Form
- A relation schema $R$ is "in BCNF" if:
	- Every functional dependency $A \rightarrow B$ that holds on it is EITHER:
		1. Trivial - If the right is a subset of the left.
		2. A is a superkey of R
- Side note, $A \rightarrow B$, $B \rightarrow C$, for $R(A, B, C)$ is NOT in BCNF since $B !\rightarrow A$, therefore $B$ is not a superkey that can account for all $C$. $A \rightarrow B$ is a "many-to-one" relation.
- BCNF guarantees that there will be no redundancy that results from a functional dependency.
- Consider a relation $r(A, B, C, D)$ with functional dependency $A \rightarrow B$ and two tuples: $(a_1, b_1, c_1, d_1)$ and $(a_1, b_1, c_2, d_2)$.
	- NOT IN BCNF.
### How it Works
- Given an FD, $A \rightarrow B$, if $A$ repeats, then $(B-A)$ also has to repeat (any part of B that is not in A also must repeat).
- If rule 1 (trivial) is satisfied, $(B-A)$ is empty, thus not a problem.
- If rule 2 is satisfied, then $A$ can't be repeated, so this doesn't happen either.
## Creating BCNF Schemas
- For all dependencies $A \rightarrow B$ that hold on a relation, check if $A$ is a superkey.
- If not, then
	- Choose a dependency that breaks the BCNF rules, say $A \rightarrow B$
	- Create $R1 = (A, B)$
	- Create $R2 = R - (B-A)$ (new relation WITHOUT B).
	- Note that: $R1 \cap R2 = A$ and $A \rightarrow AB (=R1)$, so this is lossless decomposition.
Not always possible to find a dependency-preserving decomposition that is in BCNF.
- NP-Hard to find one that exists.
## BCNF and Redundancy
- Complex when there's multi-valued dependencies.
- 3NF is an alternative to BCNF that preserves dependencies, but may allow some FD redundancy.
- 4NF removes redundancies due to multi-valued dependencies.
- 