Perceptrons:
- The algorithm requires trainable weights that allowed supervised learning.
- Scoring: $z = w\cdot f(x)$, then filtered through the sigmoid function: $\phi(z) = \frac{1}{1+e^{-z}}$
	- Weight vector $w$, activation value $f(x)$ respective of current class.
	- $y = argmax_y w_y \cdot f(x)$ 
![[Pasted image 20260423141537.png]]
Logistic Regression:
$$ max_w ll(w) = max_w \sum_i log(P(y^i|x^i;w)$$
- where P(x) is a sigmoid function.

Multiclass Logistic Regression

Gradients
- Remember, partial derivatives.
- Gradient Ascent (hill climbing, local maxima) and Gradient Descent (local minima).
- Learning rate is adjustable through $\alpha$. Hyperparameter that influences convergence rate (under shooting and over shooting on new iterations).
- With Momentum:
	- Additional parameter of $\beta$, which multiplies onto $\alpha$. Influences the convergence rate without influencing the learning rate.
- Stochastic Gradient Ascent
	- Noisy, high variance. Needs smaller learning rates.
- Mini-batch Gradient Ascent
	- Moderate variance, needs an intermediate learning rate, sometimes with decay.

Neural Networks
- Just multi-layer perceptrons.

Batch Sizes

Log Loss

CNN (Convoluted Neural Network)
- Neural Networks for image classification.
