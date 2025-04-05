We will learn: 
- [[Fall24/CMSC320/Decision Trees]] 
- Random forests
- Neural Networks
- Naive Bayes
- Support Vector Machines
## Supervised Learning Classification Problem
#### Ex
Given historical bank data, a client comes in requesting a large loan. If you accept the request you may profit or the client fails to pay back the loan, making you lose money. Do you give them a loan?

It has:
- A target variable you want to predict (called a 'class' or a 'label')
- A set of historical data where that target label is known.
- New data where that target variable is unknown.
- A *model* is a mathematical object that takes data where the label is *unknown* and assigns it a label.
##### Vocabulary
- K-Nearest is a *supervised learning model*, which means the training data has labels.
- This example is called *classification* where we are deciding between categories.
- We are doing *binary classification* where there are only two categories.
- KNN is an *instance based model* meaning it requires all the training data be stored to work (so we can constantly feed data to KNN and have it evolve).
- A *hyperparameter* is an argument to the model that determines how the model behaves (this changes with every problem, so k in KNN).

## K-Nearest Neighbor
An ML Algorithm designed to perform analysis on a "scatter plot" of points. It takes the weighted average of the k-nearest neighbors based on their Euclidean distance from the unknown and determines the label.
$$
y=\sum_{k\in{K}}\frac{1}{dist(k,p)}*k_{label}
$$
![[Pasted image 20241015094046.png]]
### Pros/Cons
- Extremely accurate. Used in large scale production in industry.
- You must be able to store the entire training set - this could be on the order of peto-bytes or exo-bytes.
- K must be tuned correctly.
- Searching for nearest neighbors is very time consuming, the spherical approach helps.
## K-Nearest Spherical Neighbor
Designed to take the average of neighbors that are encompassed by a radius around the unknown label and determines the label.
