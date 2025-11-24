# Query Processing cont.
## Join
- `select * from R, S where R.a = S.a`
	- Called "equi-join".
- `select * from R, S where |R.a - S.a| < 0.5`
	- Not an "equi-join".

## Nested loops join