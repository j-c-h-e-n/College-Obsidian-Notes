We can sometimes have too many dimensions in our dataset.
We can easily remove certain columns such as:
- Things we don't care about.
- Irrelevant data.
But most of the time it isn't easy.

## Principal Component Analysis
- A tool for dimensionality reductions.
- Find the principle components of the data:
	- An ordered series of vectors along which the *most* variance lies.
- The first principle is the long one, the one that explains the most variance., The second is the second longest, etc.
- We can decide to keep however many components we want.
### How It Works
First, we center our point son the origin. Then we we create the covariance matrix for our data:
- The principle components of our data are what's called the *eigenvectors* of this matrix.
![[Pasted image 20241003095829.png]]
### Benefits
- **Dimension Reduction**: PCA reduces the number of features while retaining essential information, making datasets more manageable.
- **Noise Reduction**: Eliminate noise in data, improving feature quality and model performance.
- **Feature Transformation**: Transforms original features into orthogonal components, potentially revealing hidden patterns.
- **Visualization**: PCA simplifies data into 2 or 3D for easier visual understanding.
- **Feature Selection**: PCA ranks principal components, helping indirectly in feature selection.
- *TLDR: Removes extraneous data by ranking and a threshold and re-formats the data better.*

## Covariance
Variance: For a single column of data, it's the average of how far something is from the mean.
Covariance: For two columns of data, measures how correlated being far away from the mean is.
$$\sigma(x,y) = \frac{1}{n-1}\Sigma_{i=1}^{n}(x_i-\overline{x})(y_i-\overline{y})$$
![[Pasted image 20241003094323.png]]
