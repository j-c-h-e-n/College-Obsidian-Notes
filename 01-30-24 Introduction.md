**Note: We use OCaml for our introduction to functional programming**
# Introduction
We are used to "typing" in programming (int, double, boolean, etc.). 
In different languages, the behaviors between types do not transfer over to other languages as shown below with Java and C.

| Language | 3 | true | 3 + 4 | 10 && 4 |
| --- | --- | --- | --- | --- |
| Java | int | boolean | 7 - int | N/A |
| C | int | N/A | 7 - int | 1 - int |

##### Vocabulary
- **Type Checking:** The process of determining a variable's type. 
- **Dynamic Typing:** Type checking is performed at runtime (*Python*). 
- **Static Typing:** Type checking is done at compile (*C, OCaml*). 
- **Manifest (Explicit) Typing:** Explicitly telling the compiler the type of new variables (*int x = 3*).
	- "int x can only hold int now and forever"
- **Latent (Implicit) Typing:** Not needing to provide a type for a variable (*Python does this). 
	- "x can mutate and change the type of data it holds"
# Functional Programming
A programming paradigm based on functions. <br> Wikipedia states:
>Functional programming is a programming paradigm where programs are constructed by applying and composing functions.
## Declarative vs Imperative
In a program that finds even numbers in a list, an imperative solution would:
- Make an empty list called results.
- Divide the item by 2 and look at the remainder.
- If the remainder is 0, add the value to the results list.
- Return the results list after all list items have been considered.
A declarative solution would:
- Take all the values that are divisible by 2 from the list.
- Return those values

We will use Python to highlight the differences:
```python
#imperative.py
def evens(arr):
	results = []
	for x in arr:
		remainder = item % 2
		if remainder == 0:
			results.push(item)
	return ret
```
Notice how we walk through every detail in creating what we need.
```
#declarative.py
results = [x for x in arr if x % 2 == 0]
```
Notice how we tell and get what we want without going into detail.
## Side Effects and Immutability
One of the goals of functional programming is to minimize or even remove the possibilities of side effects. <br>
For example, the following Python code:
```python
#side_effects.py
count = 0
def f(node):
	global count
	node.data = count
	count += 1
	return count
```
The side effect here is the manipulation of "count". We normally consider this a feature and a just a way of programming in other languages, but functional programming wants to treat functions as mathematical equivalences, meaning only local variables are modified. Thus the following should be true:
$$f(x) + f(x) + f(x) = 3f(x)$$
However according to the python code:
$$f(x) + f(x) + f(x) = 1 + 2 + 3$$
when we expect: 
$$f(x) = 1$$
$$3*f(x) = 3$$
In more complex projects, side effects such as these makes it harder and harder to predict and debug our code. To prevent this, OCaml makes ALL variables immutable to maintain referential transparency.

## Functional Programming is...
- Typically used interchangeably with language features.
- Many languages have overlap.
- Python is imperative, object oriented, and functional.
- OCaml is imperative, object oriented, and functional.
## Features of Functional Languages
- Immutable state (*you cannot change the values of any variable once set*).
- Declarative programming.
- Referential transparency.
- Non-imperatively to minimize side-effects. Functions have consistent outputs for every input.
- Builds "correct" programs due to the precise nature of functional languages.
##### Vocabulary
- **Side Effect:** When non-local variables are modified.
- **Referential Transparency:** The ability to replace an expression or a function with its value and still obtain the same output.
- **Programming Paradigm:** Classification of programming approaches based on code behavior.
- **Program State:** The state of the machine at any given time.
	- Typically described as the contents of variables.
- **Imperative:** State is mutable, changing or destructive.
---

