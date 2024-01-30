# Introduction
We are used to "typing" in programming (int, double, boolean, etc.). 
In different languages, the behaviors between types do not transfer over to other languages as shown below with Java and C.

| Language | 3 | true | 3 + 4 | 10 && 4 |
| --- | --- | --- | --- | --- |
| Java | int | boolean | 7 - int | N/A |
| C | int | N/A | 7 - int | 1 - int |

##### Vocabulary:
- **Type Checking:** The process of determining a variable's type. 
- **Dynamic Typing:** Type checking is performed at runtime (*Python*). 
- **Static Typing:** Type checking is done at compile (*C, OCaml*). 
- **Manifest (Explicit) Typing:** Explicitly telling the compiler the type of new variables (*int x = 3*).
	- "int x can only hold int now and forever"
- **Latent (Implicit) Typing:** Not needing to provide a type for a variable (*Python does this*). 
	- "x can mutate and change the type of data it holds"
---
# Functional Programming
A programming paradigm based on functions.
#### Functional Programming is:
- Typically used interchangeably with language features.
- Many languages have overlap.
- Python is imperative, object oriented, and functional.
- OCaml is imperative, object oriented, and functional.
#### Features of Functional Languages:
- Immutable state (*you cannot change the values of any variable once set*).
- Declarative programming.
- Referential transparency.
- Non-imperatively to minimize side-effects. Functions have consistent outputs for every input.
- Builds "correct" programs due to the precise nature of functional languages.
##### Vocabulary:
- **Programming Paradigm:** Classification of programming approaches based on code behavior.
- **Program State:** The state of the machine at any given time.
	- Typically described as the contents of variables.
- **Imperative:** State is mutable, change or destructive.
