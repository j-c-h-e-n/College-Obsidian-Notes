# K-Means
- An unsupervised model (no labels).
- Objects are represented as points in some space.
- The goal is to form clusters.
	- Given points $x_1$, $x_2$, ..., $x_n$ forming clusters $A_1$, $A_2$, ..., $A_k$ so that every point belongs to exactly one cluster.
	- Every cluster $A_j$ has a center point, $c_j$. In K-Means we form clusters to minimize loss: $$ min_{(A, c)} \sum_{j=1}^{K}\sum_{x_{i}\in{A_j}}(c_j - x_i)^2$$
#### Vector Quantization of Images Example
- An image consists of pixels.
	- Each pixel's color is described by RGB.
	- 8 bits per color, 24 bits per pixel.
	- Storing images with P pixels takes 24P.
	- ![[Pasted image 20250210142643.png|100]]
- Or, we can create a table containing all different colors.
	- We can represent each pixel's color using a pointer to the table.
	- With P pixels and K colors this takes $24K + P*log(K)$ bits.
	- We can cluster pixels into K clusters and replace each pixel with the corresponding cluster center.
	- ![[Pasted image 20250210142821.png|300]]

## K-means Algorithm
1. Initialize K cluster centers.
2. Assign every point to a cluster center.
3. Recompute every cluster center.
4. If the cluster centers don't change, exit. Else return to 2.
We perform steps 2 and 3 to reduce the loss as much as possible.
### About K-Means
- This is a common type of algorithm. When it's hard to optimize over all variables, we hold some fixed and optimize over the others, then optimize over remaining values, then repeat.
- This is not guaranteed to lead to a globally optimal solution.
- As a consequence, different initializations can lead to different solutions.
### 1. Initializing K Cluster Centers
- SUPER important.
	- Different initializations lead to different solutions.
- But initializations are pretty heuristic.
### 2. Assigning Every Point
- To minimize the loss, we assign each point to the nearest cluster center.
- Note each point appears in the loss once.
	- The term with a point is smallest when the cluster center is closest.
- In case of a tie, doesn't matter which center you pick.
### 3. Recomputing Centers
- Choose cluster center to minimize sum-of-square distance from center to every point in cluster.
- $m$ denotes a possible choice for cluster center. Minimize the error $\sum_{i=1}^{n}(m-x_i)^2$
	- $x_i$ are the actual positions of the points we are adapting.
- To do this we take derivative and set to 0.
- $\frac{dL}{dm} = \sum_{i=1}^{n}2(m-x_i)=0$
- $nm = \sum_{i=1}^{n}(x_i)$ so $m=\frac{\sum_{i=1}^{n}(x_i)}{n}$
###  4. If No Change
- Note that if the centers don't change, the assignments won't change, so the centers won't change, ... so we can stop.
- This is guaranteed to converge.
	- Every change in assignment either reduces the loss or leaves it the same.
	- Changing the centers either reduces the loss, or leaves it the same.
		- It only leaves it the same if there is no change in the centers.
- So, the loss keeps going down. When it stops going down, the centers will not be changing.
- There are only a finite (exponential) number of possible assignments.
	- So there are only a finite number of sets of centers that could be copmuted.
	- In one iteration you either stay with the same centers and converge, or move to centers with a lower loss.
	- So you can never come back to a set of centers you left.
## Choosing a K
- There is some work on this, but no single great solution.
- Depends on your goals:
	- Big K -> underfitting, little K -> overfitting.
- For images, it's a trade off between image quality and image storage size.