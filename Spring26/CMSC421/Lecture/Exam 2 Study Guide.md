## important concepts
- World representation:
	- Propositional Logic
		- $P \land Q$ 
		- If we have $p \rightarrow q$:
			- converse: $q \rightarrow p$
			- contra-positive: $\lnot q \rightarrow \lnot p$
			- inverse: $\lnot p \rightarrow \lnot q$
	- First Order Logic
	- Horn Clauses and Prolog
	- Ontologies
- Logical Inference:
	- Entailment (|=)
		- "One thing follows from another".
		- A knowledge base ENTAILS sentence $a$ IIF $a$ is true in all worlds where the KB is true.
			- ex: If KB has "Giants won" and "Reds won", then it entails the sentence "Either the Giants or Reds won".
		- A relationship between sentences that is based on semantics.
	- Inference (|-$_i$)
		- Sentence $a$ can be derived from the KB by procedure i
	- Unification
	- Enumeration, forward and backward chaining
- Planning:
	- STRIPS
	- Representing planning problems
	- Goal regression
	- Progression/regression
	- Partial order planning
	- Hierarchical planning
	- Causal links
	- Handling Resources
	- GraphPlan

## study guide
- Syntax and semantics 
- what is a knowledge base?
	- The set of all facts/axioms/statements/relationships that we know about the environment.
- WFF (Well-formed-formula)
- Expressiveness
- Representational power of various logics
- Entailment and inference
- Soundness and completeness in the context of logic
- Complexity and computability of inference for different logics
- Propositional Logic (and what makes a WFF)
- FOL (and what makes a WFF)
- FOL predicates, functions, relations, and variables.
- Differences between prop logic and FOL
- What is a horn clause
- Modus Ponens
- Forward Chaining
- Backward Chaining
- Unification
- Quantifiers and what they mean
- Problem representation in Propositional Logic
- Problem representation with FOL
- Problem representation with Horn clauses
- Frame, Qualification, and Ramification problems
- Ontologies, semantic networks, and knowledge graphs
- Relationships encoded as triple stores (subset_of, isa, etc)
- Building knowledge bases
- STRIPS-style planning
- STRIPS-style operators
- Progression/regression planning
- Representation of action: preconditions and effects
- Linear planning and forward planning
- Linear planning vs partial order planning vs hierarchical planning vs nonlinear planning
- Regression of a goal, goal interactions, and sussman anomaly
- Planning problems such as Block World
- Resource constraints in planning
- Time in planning
- Setting up planning problems
- Defining operators
- Pre/post conditions and effects
- Least commitment


## 1. Logical Foundations

This phase covers the "grammar" and rules of formal systems.
- **Syntax vs. Semantics:** Distinguish between the formal symbols (**Syntax**) and their mapped meanings or truth values (**Semantics**).
- **WFF (Well-Formed Formula):** The specific rules that define a valid string in a given logic.
- **Metalanguage Concepts:** 
	- **Entailment ($\models$):** Logical following (if A is true, B must be true).
    - **Inference ($\vdash$):** The process of deriving new sentences from old ones.
    - **Soundness:** If the system derives it, it is true ($P \vdash Q \implies P \models Q$).
    - **Completeness:** If it is true, the system can derive it ($P \models Q \implies P \vdash Q$).

## 2. Logic Systems & Inference
Compare the power and computational limits of different frameworks.
- **Propositional Logic:** Boolean variables; limited **Expressiveness**.
- **First-Order Logic (FOL):** Adds **Predicates, Functions, Variables,** and **Quantifiers** ($\forall, \exists$).
- **Inference Mechanisms:**
    - **Modus Ponens:** The fundamental rule $(P \land (P \implies Q)) \vdash Q$.
    - **Horn Clauses:** Disjunctions with at most one positive literal (efficient for inference).
    - **Unification:** The process of finding substitutions that make different logical expressions identical.
    - **Chaining:** **Forward** (data-driven) vs. **Backward** (goal-driven).

## 3. Knowledge Engineering
Focus on how we structure a **Knowledge Base (KB)** to model the world.
- **Representational Structures:** 
	- **Ontologies & Semantic Networks:** Hierarchical structures.
    - **Knowledge Graphs:** Data represented as **Triple Stores** (Subject-Predicate-Object).
- **Modeling Challenges:**
    - **Frame Problem:** Accounting for what _doesn't_ change after an action.
    - **Qualification:** Dealing with the infinite preconditions for an action.
    - **Ramification:** Accounting for the indirect effects of an action.

## 4. Automated Planning
Applying logic to achieve goals through sequences of actions.
- **STRIPS Formalism:** Representing actions via **Operators** with defined **Preconditions** and **Effects** (Add/Delete lists).
- **Search Directions:** **Progression** (Forward from start) vs. **Regression** (Backward from goal).
- **Planning Paradigms:**
    - **Linear vs. Partial Order:** Sequential steps vs. a "Least Commitment" approach where steps are only ordered when necessary.
    - **Hierarchical (HTN):** Breaking complex tasks into primitive sub-tasks.
- **Classic Problems:** The **Sussman Anomaly** (where sub-goal interactions break linear planning) in the **Blocks World** domain.