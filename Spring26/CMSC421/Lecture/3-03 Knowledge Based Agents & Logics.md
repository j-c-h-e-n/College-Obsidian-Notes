- What is a state?
	- So far, we have examined Atomic, Factored, and Structured states for reflex/goal/model agents.
- What's different?
	- Sensors
	- Actuators
	- Internal workings
	- Notion of state: becomes a structured state.

- First Incompleteness Theorem
	- Any consistent formal system strong enough to express arithmetic contains true statements it cannot prove.
- Second Incompleteness Theorem
	- Such a system also cannot prove its own consistency.
- Leads to Turing's "halting problem".

- Logical Inference is a kind of search.

- First-order logic
	- Horn clauses (Prolog is derived from this)
- Propositional logic

- Proof Methods
	- Model checking (proof as SAT)
		- Truth table (inference by enumeration)
		- Improved backtracking.
		- Heuristic search of assignments in model space (sound but incomplete).
	- Application of inference rules (proof as search)
		- Proof is a sequence of rule applications.
		- Easier to do if logics are in a structured form.
	- Algorithms used are DPLL and WalkSAT

- First Order Logic:
	- Propositional logic assumes world contains only facts.
	- First-order logic assumes:
		- Objects
		- Relations
		- Functions
	- Promotes a much richer way of representing the world, can have much more complex models representing objects and their states.