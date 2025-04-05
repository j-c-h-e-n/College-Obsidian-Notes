- Goal is to train some machine learning model to be able to accurately label our provided tabular data.
## Training Set
Each instance is described by a set of common features.
- In this example, they have discrete values.
- But they could be continuous (temperatures).
Each instance has a binary label.
- i.e. did we play tennis?
- Labels might not be binary.
- This is a classification problem. If labels are continuous, this is regression.
![[Pasted image 20250129141444.png|400]]
## Learning Decision Trees
- What makes a decision tree good? What is the "best" one?
- How do we find a good decision tree?
- These two questions are common to learning many different kinds of models.
	- E.g. when training neural networks, we define a *loss* which represents the quality of the model, and search for a good model iteratively, with gradient descent.
### Quality of a Decision Tree
- What fraction of the training data is correctly classified?
- If there are n binary features, and the tree is full, then there are $2^{n}$ leaves.
	- There are n trees of depth 1.
	- There are $n*(n-1)^2$ trees of depth 2.
	- There are $n*(n-1)^2*(n-2)^4$ trees of depth 3.
### Top-down Induction of Decision Trees (Greedy)
```
CurrentNode = Root

DTtrain(examples for CurrentNode, Features at CurrentNode):
	1. Find F, the "best" decision feature for next node.
	2. For each value of F, create new descendant of node.
	3. Sort training examples to leaf nodes.
	4. If training example sperfectly classified, STOP.
	   Else, Recursively apply DTtrain over new leaf nodes.
```
- Relating this back to CMSC320, we are maximizing our information gain, and inversely, reducing our entropy.
- With n features building a tree with m nodes, T training examples, and d max depth, we have O(nmTd).
### Issues of Finding the "Best" DT
- We can have the tree run infinitely and fully exhaust all nodes, having a 100% match rate. 
	- Tree becomes too big.
	- Overfit as hell.
	- Sucks at generalizations.
	- Interpretability becomes worse as there are more and more features.
## Bayes Optimal Classifier
- If we know the probabilities of everything happening perfectly, we can always make the best possible splits in our DT.
- (Fill in more)
## Probabilistic Viewpoint
- It may seem unintuitive sometimes to think of features as random.
	- Ex: if I'm trying to decide if a dog is a poodle or a beagle, poodles have hair and beagles have fur. This is not random.
	- However, our ability to detect features is somewhat random.
- Often our features are incomplete.
	- Whether I like a movie isn't random.
	- But if all we know is that it's a big budget comedy, it is kind of random.
## DT Size and Errors
- Growing the DT always redices errors on **training set**, or keeps them the same.
- Suppose A samples reach a node with label 1 and B samples reach it with label 0, and A>B.
	- If this is a leaf, it will make B mistakes on the training data.
- Suppose we add a feature here and create two new leaves.
- $A_1$ samples with label 1 and $B_1$ samples with label 0 reach the first leaf.
- $A_2$ samples with label 2 and $B_2$ samples with label 0 reach the second leaf.
- $A_1 + A_2 = A$, $B_1 + B_2 = B$.
	- If $A_1 > B_1$, $A_2 > B_2$, we make $B_1 + B_2 = B$ mistakes.
	- Vice versa.
## How to Prevent Overfitting
- Add a regularization term to the loss.
	- Instead of just measuring accuracy, measure accuracy + simplicity of model.
- Restrict the model so it can't just fit anything.
	- Max depth for DT. Linear regression instead of non-linear regression.
- Try to measure the confidence that adding a node will improve label prediction.
	- How much data is there? How much does it improve classification?
The easiest and most general is to use a validation set.
- Sequester some training data. Do not use it to train.
- As you build the DT, test its performance on the validation set.
- Larger trees always do as well or better on the test set.
- But they may do worse on the validation set.
### Misc
- What about weak features?
	- $P(x_3=1|y=1)=.51$ and $P(x_3=1|y=0)=.49)$ 
		- Should not use, there isn't much point in using such a small distinction. Could just be noise.
