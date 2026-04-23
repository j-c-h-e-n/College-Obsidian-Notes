Viterbi Algorithm, Particle Filters, Kalman Filters

Hidden Markov Models
- Similar to MDPs, but unknown state.
- Only know the outcomes of each state, not the state itself.


State Space Models:
- Hidden Markov Models (HMM)
- Kalman Filter Model (KFM)
- Dynamic Bayesian Networks (DBNs)

Viterbi Algorithm:
- Given sensor inputs, what is the most likely state that explains these inputs?
- Uses Maximum Likelihood to determine which explanation is most likely.
	- "The path that most likely produced the observed sequence".
- Steps (see slides):
	- Initialize
	- Recurse
	- Termination
- $\pi^*$ is found through back-tracking.
- Gets it right more often than it gets it wrong.

Some Notation:
- $x$ is the sequence of symbols emitted by model, $x_i$ is symbol at time $i$.
- $\pi$ is a sequence of states, i.e. a path. $\pi_i$ is the i-th state in the path $\pi$.
- The most likely path $\pi^*$ satisfies $\pi^* = argmax_{\pi} Pr(x, \pi)$ 

Filters:
- 

Bayes Filter:
- Two steps: Predict and Update.
- Assumes the 1st order Markov Process.
- Starts from Bayes Theorem: $p(x_0 | z_0) = \frac{p(z_0 | x_0) p(x_0)}{p(z_0)}$ 

Particle Filter:
- "We don't need to know the PDF, we'll just represent with Particles".
- Aka Sequential Monte Carlo Methods, represent belief by sets of random samples or particles.
	- $Bel(x_t) \sim S_t = \{<x_t^i, w_t^i, >| i = 1,...,n\}$ 

Kalman Filter (more in slides)
- Model:
	- Linear discrete time dynamic system (motion model)
		- $y_k, y_{k-1}$ is the state
		- $u_k$ is the control input
		- $A$ is the state transition function (matrix)
		- $B$ is the control input function (matrix)
		- $y_k = Ay_{k-1} + Bu_k + w_{k-1}$
	- Measurement equation (sensor model)
		- $z_K = Hy_k + v_k$ 
- Prediction is based on measurements computed from previous timesteps.
- Then we refine our prediction with current information, creating our final measurement at time k.
	- "Prediction-Correction" cycle, constantly refining to reduce error.
- Highly efficient due to its linear nature, polynomial in measurement.
- Kalman filters break if the transition is non-linear.
- Used in:
	- object tracking
	- economics
	- navigation
	- computer vision