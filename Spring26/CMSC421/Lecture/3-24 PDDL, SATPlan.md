Resources, PDDL, Non-linear planning, hierarchical planning, graph plan, SATPlan, and more

- Previously: representations in planning, using actions as preconditions + effects.
- STRIPS: the basic planning representation.
- Planning as a kind of search, searching through a space of partial plans.
	- Searching over symbols, making strings of symbols work the best.
- Linear planning: working on one goal at a time, completely solving before progressing to the next.
- Partial-order-planning: constraining the ordering of the problem only as much as you need to.

- Planning problems:

- Graph plan creates a complete plan that results in a "linear" sequence of events, fixing contradictions and invalid actions.

- STRIPS language cannot handle:
	- hierarchical plans
	- complex conditions, such as if-then-else conditional situations.
	- resources, i.e. budgets, actions need to incorporate resource generation and usage.
	- time


- PDDL:
	- PDDL uses STRIPS as a encoded formalism.
	- Components:
		- Objects
		- Predicates
		- Initial State
		- Goal
		- Actions
	- Planning tasks requires two files:
		- A domain file for predicates and actions
		- A problem file for objects, initial state, and goal specification

- Hierarchical Decomposition Planning
	- Consider the problem of building a house:
		- Symbols:
		- Predicates:
		- Operators:
		- Goals:
		- State description:

- Non-monotonicity in AI Planning:
	- Statements/responses can change as the knowledge base grows.