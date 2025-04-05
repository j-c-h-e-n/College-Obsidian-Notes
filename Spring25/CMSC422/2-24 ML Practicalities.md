# Feature Selection
- Good features are:
	- Informative about the label.
		- if a feature almost always has the same value, probably not informative.
	- Not redundant with other features.
- We can use statistical tests to try to determine this, or domain knowledge.
	- You probably have an idea that the stars or genre may predict whether someone will like a movie, while the identity of the sound editor is less likely to.
- When we have enough data, we can often rely on the learning model to determine automatically which features are relevant.
# Normalizing Features
- Given each feature 0 mean, same dynamic range.
- What happens to perceptron if one feature has values 1000x bigger than another?
- A bunch of formulas in CIML chapter 5.1-5.7.
# Precision/Recall
- This is relevant when you are looking for members of a class, and retrieve some set of top candidates.
- For example, with perceptron you could vary the number of retrieved examples by how far they are from the decision boundary.
- A bunch of formulas in CIML 5.8-5.12
## Precision/Recall Curves
- Gives information about range of performance.
- Some applications require high precision, some require high recall.
- Curves generally ~monotonic.
- Allows more detailed comparison.

# Cross-Validation for Hyperparameter Selection
![[Pasted image 20250224143628.png]]
