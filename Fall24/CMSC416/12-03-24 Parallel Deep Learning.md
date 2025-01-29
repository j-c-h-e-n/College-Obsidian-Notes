## Background
- Main Idea: The evolution of HPC systems and rise of a new revolution in AI.
- In the last two decades, an enormous amount of compute power has become available.
- Large datasets and open source software such as PyTorch have also emerged.
- Led to a frenzy in the world of AI and the effects are being felt in almost every other domain.
### Use of AL/ML in HPC
- Some of the earliest works were around 2007 on using traditional machine learning approaches to model performance and power.
- 2007-2017: Supervised and unsupervised algorithms for creating prediction models.
- 2018-present: Deep learning and transfer learning starting to become popular.
### AL/ML for Computation Science
- Using of AI models for approximating computation in scientific codes.
- Black box models.
- Physics-informed machine learning models.
![[Pasted image 20241205131258.png]]
# Deep Neural Networks (DNNs)
- An area of machine learning that uses artificial neural networks to learn complex functions.
	- Often from high-dimensional data: text, images, audio...
- Widespread use in computer vision, natural language processing, etc.
- Neural networks can be used to model complex functions.
- Several layers that process "batches" of the input data.
- ![[Pasted image 20241205131411.png|200]]
## Deep Learning and Language Models
![[Pasted image 20241205131450.png]]
### Other Definitions
- Learning/training: task of selecting weights that lead to an accurate function.
- Loss: a scalar proxy that when minimized leads to higher accuracy.
- Gradient descent: process of updating the weights using gradients (derivates) of the loss weighted by a learning rate.
- Mini-batch: small subsets of the dataset processed iteratively.
- Epoch: one pass over all the mini-batches.

# LLM Training and HPC
![[Pasted image 20241205131820.png]]
## Parallel/Distributed Training
- Many opportunities for exploiting parallelism.
- Iterative processes of training (epochs).
- Many iterations per epoch (mini-batches).
- Many layers in DNNs.
  ![[Pasted image 20241205133150.png]]
## Sequential LLM Training
```cpp
while (remaining_batches){
	Read a single batch

	Forward pass: perform matrix multiplication to copmute output
		activations, and a loss on the batch

	Backward pass: matrix multiplications to compute gradients of the loss
		w.r.t paramters via backpropagation

	Optimizer step: use gradients to update hte weights or parameters
		such that loss is gradually reduced.
}
```
## Data Parallelism
- Divide training data among the workers (GPUs).
- Each worker has a full copy of the entire Neural Network and processes different mini-batches.
- All reduce operation to synchronize gradients.
- Example:
	- PyTorch's DDP.
	- ZeRO.
![[Pasted image 20241205133457.png|300]]
## Inter-layer Parallelism
- Assign entire layers to different processes/GPUs.
	- Ideally map contiguous subsets of layers.
- Point-to-point communication (activations and gradients) between processes/GPUs managing different layers.
- Use a pipeline of mini-batches to enable concurrent execution.
![[Pasted image 20241205133738.png]]
## Intra-layer Parallelism
- Enables training neural networks that would not fit on a single GPU.
- Distribute the work within each layer to multiple processes/GPUs.
	- Essentially parallelize matrix operations such as matmuls across multiple GPUs.
- Example: Megatron-LM.
# Hybrid Parallelism
- Using two or more approaches together in the same parallel framework.
- 3D parallelism: use all three.
- Popular serial frameworks: PyTorch, Tensorflow.
- Popular parallel frameworks: DDP, MeshTensorFlow, Megatron-LM, ZeRO.
## 4D Hybrid Parallel Approach
- Combines data parallelism with 3D parallel matrix multiplication.
- ![[Pasted image 20241205134105.png|400]]
## Enabling 3D Parallel Matrix Multiplication in AxoNN
- Each layer is multiplying input activations with weights to produce output activations.
- Distribute I and W across a 3D grid of GPUs.
- Compute partial output activations, O on each GPU.
![[Pasted image 20241205134225.png]]
### Easy Parallelization with AxoNN
- Requires minimal code changes to model architecture.
```python
from axonn.intra_layer import auto_parallelize

with auto_parallelize():
	net = # your sequential model here
```
- AxoNN intercepts all declarations of `torch.nn.Linear` and parallelizes them.
![[Pasted image 20241205134427.png]]
