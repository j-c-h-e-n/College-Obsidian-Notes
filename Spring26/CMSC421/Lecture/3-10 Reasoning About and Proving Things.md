Logic:
- Syntax: What sentences are allowed?
- Semantics: what are possible words?
- Logical inference: creating new sentences that logically follow a set of rules.
- Propositional logic is a logic of T/F statements.

Resolution:
- if A or B is true, and not B or C is true, then A or C is true.
- $(A \lor B) \land (\lnot B \lor C) \rightarrow A \lor C$ 

Horn clauses only allow one positive literal in each clauses.
- Invalid statements:
	- $A \lor B$ since both can be true.
	- $A \oplus B$ 
	- And a few more.

Forward and Backward Chaining
- FC is data driven, automatic, unconscious processing, finding the implications of an input.
	- Derives every atomic sentence entailed by knowledge base.
	- Reaches a fixed point where no more new atomic sentences can be derived.
	- Basically expands every possible deduction from the knowledge base.
- BC is goal driven, appropriate for problem-solving, looking for a path to an answer.
	- Prolog uses BC.
	- 

