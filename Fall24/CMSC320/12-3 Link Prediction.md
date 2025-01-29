# Link Prediction
- Predicting whether or not a link will form between two notes at time t+1.
- Determines using similar features.
- Supervised learning.
# Network Topology
- Understand where weak points in networks are, either to:
	- Shore them up.
	- Remove them.
- Unlike link prediction, this is unsupervised learning.
## Bridges:
- *Bridges* are basically an edge.
- These are rare in real-life networks:
	- Idea: relax the definition by checking if the distance between two terminal vertices increases if the edge is removed.
		- The larger the distance, the weaker the tie is.
		- (i.e. what is the distance to a node y if the edge between x-y is removed).
## Community
- Find a node that separates the graph as much as possible (maximizing your entropy per bridge/edge deletion, i.e. choosing the "most valuable edge").
- The resulting groups are called communities.
	- These are broken down until they are groups of 3 nodes.
	- Called the *[Girvan-Newman Method](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.community.centrality.girvan_newman.html)*

# Graph Neural Networks
## Node Embeddings
- We want a way to turn nodes into vectors (always trying to turn things into vectors).
- What should it mean for two nodes to be closer together?
### Network Homophily
- Def: We want nodes that are similar neighborhoods to be closer in the latent space (lower dimensional space, "more important features").
- The weights from this network give an embedding for each node.
### Structural Equivalence
- Def: Nodes that are structurally similar should be closer in the embedded space. So instead of keeping friends together, we keep 'popular kids' and 'loners' together.'
### Random Walks
- A random walk is where you randomly walk around (literally randomly taking edges).
- ![[Pasted image 20241203100312.png]]
#### Improving instead of random walk:
- We can have two parameters, p and q.
	- p: The return parameter. Controls the likelihood of immediately returning to a node we just visited in a walk.
	- q: The in-out parameter. It controls how likely we are to stay in the neighborhood of the node u, or are we more likely to visit nodes further away from node u.
- Adjusting p and q:
	- Staying in the neighborhood is like BFS. 
		- Becomes a structural-equivalent embedding.
	- Visiting further away is like DFS.
		- Will become similar to a network homophily structure.
### Skip Gram
- look into this later

