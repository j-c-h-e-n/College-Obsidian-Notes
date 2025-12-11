# Basic Query Structure
```SQL
select A1, A2, ..., An
from r1, r2, ..., rm
where P
```
- Select all attributes (\*).
- Having expressions in the select clause instead of using `where` makes it so that it returns True or False for each tuple of the respective attribute.
- `where` will fail if there's any tuples with NULL. (double check this claim)
- `natural join` is DANGEROUS. It does a `where` "equals" on every matching attribute name.

# NULLS
- Major headache for query optimization.
- Can be a value of any attribute.
- Could mean the value is:
	- unknown?
	- inapplicable?
	- withheld? We are not allowed to know.
## Arithmetic with Null:
- Just ignore. Not there.
- So any arithmetic with null = null.
## Boolean with Null:
- **Boolean comparisons will always result in UNKNOWN.**
- For <, <=, >=, <>, >, ...:
	- It will just not return it (the tuple).
- For null = null:
	- It will also not return because it is unknown.
## The (is) operator:
- This allows us to identify nulls.
- `where attribute is Null` will correctly identify and return the tuples with null.

# some fucked up stuff
order doesn't matter

FALSE OR UNKNOWN = UKNOWN
*TRUE OR UNKNOWN = TRUE*, *TRUE OR NULL = TRUE*
TRUE AND UNKNOWN = UNKNOWN
UNKNOWN OR UNKNOWN = UNKNOWN
UNKNOWN AND UNKNOWN = UNKNOWN
NOT (UNKNOWN) = UNKNOWN

