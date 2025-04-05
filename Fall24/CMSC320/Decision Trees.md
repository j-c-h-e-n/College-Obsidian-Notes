  "An ML model you can explain to your grandparents"
- Outdated, but super simple. Not very good.
- We use it as a building block for our stronger algorithms.
- Super susceptible to outlier data since every single case is weighted the same in the tree.
## Classification Algorithm
![[Pasted image 20241015094658.png|500]]
![[Pasted image 20241015094718.png|500]]
1. Get a new piece of data with no label.
2. Start at the top of the tree.
3. Go down each branch based on the appropriate feature.
4. When you get to a leaf, classify.
### IRL Decision Tree
![[Pasted image 20241015095045.png| 400]]
This is literally a flowchart which is a computer trained model that helped with heart attack diagnosis.
## How to get a Decision Tree
There are usually two algorithms, a "training" and "classification" in a *supervised learning model* (KNN does not require training).
- The decision tree is built during the training phase.
- Using the decision tree is considered classification.
## Smaller is Better
"ML models are powerful because they **generalize**"
- This means, the less specific a model is (decision tree), the better it is.
	- Simple is better.
- More rules means more specificity and therefore less generalization, and the less good the model is.
*We want as few rules as possible to classify our data*
- This means we want to split the decision tree as efficiently as possible.

# Entropy
A measure of how much information something provides (from Information Theory).
- Measured in bits.
- One bit of entropy is the amount of information it takes to encode a yes/no signal.
	- Ex: Coin flip. If heads, 1. If tail, 0.
		- Results of two coin flips = {11, 01, 10, 00}, *two bits* of entropy.
	- If something is *high entropy* it is extremely hard to compress it down since there's a lot of raw information.
## Formula for Entropy
We need a function that is:
- Continuous.
- Should be 1 if all events are equally likely.
- Should be 0 if the outcome is certain.
- And every time we double the number of potential outcomes (one coin flip has two outcomes, two has four, three has eight possible outcomes), we want entropy to increase by one.
If all events are equal probability:
$$ H(X) = lg(p_x)$$
Otherwise:
$$ H(X) = \sum_xp_x*lg(p_x)$$
With the coin flip example, we'd have:

## Purpose
- High entropy means there are multiple different outcomes.
	- Complex, chaotic.
- Low entropy means there are less outcomes (therefore simpler).
**Connecting back to decision tree...**
- We may have high entropy at the root, but once we get to the leaves, we want a low entropy where it is simple.
- Approach:
	- Making a greedy algorithm that utilizes entropy.
		- Finding features that has the greatest difference in entropy from their parent and we categorize each level with the biggest difference.
		- (getting the most worth out of your split/categorization).
### Algorithm for Picking a Split (Decision Tree)
This is called the ID3 algorithm.
- Has flaws where it is a greedy approach, and therefore does not always give the most efficient splits.
- Cannot represent more complex situations where there is a clear straight line boundary in the data. Can at best represent some sort of diagonal.
- Always tries to find a "perfect" split with the data although it may be impossible to find with what's given in the data.
	- Need to limit the depth of the recursive algorithm.
![[Pasted image 20241015101329.png|300]]
Start with the complete data:
- Check shape: compute the weighted information gain of splitting on square vs circle.
- Check color: compute the weighted information gain of splitting on red vs green.
- Check size: compute the weighted information gain of splitting on small vs large.
![[Pasted image 20241015101529.png|400]]
We find that shape and size offer the same amount of information gain. 
We then repeat the process on shape and size:
![[Pasted image 20241015101609.png|400]]
Now we see that it is most beneficial to split by shape.
Typically, it repeats and repeats until a complete split is found. Not good, infinite loop, so we need to have a break condition.
#### Math-Wise:
![[Pasted image 20241015101710.png]]

# Summary
To use a decision tree:
- We first go through the training phase where the tree decides on all of its possible splits.
- We then use it for classification where we show it data with no label and it picks the label it thinks is best.
## Use
When to use decision trees:
- When you want a pretty picture to explain machine learning to someone.
- When you want a general sense of your data you can look at (pandas has a helpful function for this).
When not to use decision trees:
- When you have an actual problem you want to solve.
