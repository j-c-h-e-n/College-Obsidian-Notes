# Data Sharing Defaults
- Most variables in an OpenMP program are shared by default.
- Global variables are shared.
- Exception: loop index variables are private by default.
- Exception: stack variables in function calls from parallel regions are also private to each thread (thread-private).

# Parallelizing Using OpenMP
- FIRST, is it worth it?
- Identify compute intensive regions/loops.
- Make the loop iterations independent.
- Add the appropriate OpenMP directive.
- saxpy (single precision $a*x+y$) 
	- Example:
	- Need to ensure that the same `z` doesn't get updated more than once.
	- Each thread has their own `i`.
```c
#pragma omp parallel for
for (int i = 0; i < n; i++) {
	z[i] = a * x[i] + y[i];
}
```
# Overriding Defaults Using Clauses
- Specify how data is shared between threads executing a parallel region.
- When variable values in threads are private, they are lost upon thread completion. The main thread does not receive their values.
- `private(list)`
- `shared(list)`
- `default(shared | none)`
- `reduction(operator: list)`
- `firstprivate(list)`
- `lastprivate(list)`
#### private() Clause
- Each thread has its own copy of the variables in the list.
- Private variables are **uninitialized** when a thread starts.
- The value of a private variable is **unavailable** to the primary thread after the parallel region has been executed.
#### default() Clause
- Determines the data sharing attributes for variables for which this would be implicitly determined otherwise.
- Possible values: `shared | none`.
- `shared` is the default for C/C++.
	- This makes it so that threads are initialized with the primary thread's copy.
	- The thread's value of the copy will be returned to the primary thread's copy.
- So `default(none)` can be used as a good programming practice.
	- Forces listing the sharing attribute for each variable.
#### firstprivate() Clause
- Initializes each thread's private copy to the value of the primary thread's copy upon entry to the parallel section.
- Private variables are **initialized** with the main thread's value.
#### lastprivate() Clause
- Writes the value belonging to the thread after it's last iteration to the primary thread's original copy.
- Last iteration is determined by sequential order (remember, the amount of loops that each thread gets is determined by total loops and total available threads).
- Private variables are **uninitialized** when a thread starts.
#### reduction() clause
- Will provide initialization to the values.
- Reduces values across private copies of a variable.
- Operators: `+, -, *, &, |, ^, &&, ||, max, min`.
- Can prevent race conditions when multiple threads are trying to read/write to the same shared variable.
- How it works:
	- Makes each thread have their own private copy of the specified variable.
	- Each then performs their own specified operations.