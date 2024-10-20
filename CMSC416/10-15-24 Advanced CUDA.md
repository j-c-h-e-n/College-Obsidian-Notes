## Striding
```cpp
__global__ void saxpy(float *x, float *y, flkoat alpha, int N) {
	int i0 = blockDim.x * blockIdx.x + threadIdx.x;
	int stride = blockDim.x * gridDim.x;

	for (int i = i0; i < N; i+= stride)
		y[i] = alpha * x[i] + y[i];
}

int main() {
	...
	int blockSize = 8; int gridSize = 16;

	saxpy<<<gridSize, blockSize>>>*(x, y, alpha, N);
	...	
}
```
- Striding allows us to allow a kernel (an executing function) to handle input sizes that are larger than the number of available threads.
	- Striding refers to the total number of threads in a grid (grid-stride loops, flexibility).
## Grid and Block Dimensions
- Blocks in a grid and threads in a block can be arranged in a 3D virtual mesh.
- Can also be indexed in 3D.
![[Pasted image 20241020150816.png]]
## Adding 2D Matrices
```cpp
__global__ void matrixAdd(float **X, float **Y, float alpha, int M, int N) {
	int i = blockDim.x * blockIdx.x + threadIdx.x;
	int j = blockDim.y * blockIdx.y + threadIdx.y;

	if (i < M && j < N)
		Y[i][j] = alpha * X[i][j] + Y[i][j];
}

int main() {
	dim3 threadsPerBlock(16, 16);
	dim3 numBlocks(M/threadsPerBlock.x + (M % threadsPerBlock.x != 0),
			N/threadsPerBlock.y + (N % threadsPerBlock.y != 0));

	matrixAdd<<<numBlocks, threadsPerBlock>>>(X, Y, alpha, M, N);
}
```
## Matrix Multiply
```cpp
__global__ void matmul(double *C, double *A, double *B, size_t M, size_t L, size_t N) {
	int i = blockDim.x * blockIdx.x + threadIdx.x;
	int j = blockDim.y * blockIdx.y + threadIdx.y;

	if (i < M && j < N) {
		for (int k = 0; k < L; k++) {
			C[i*N+j] += A[i*L+k] * B[k*N+j];
		}
	}
}

int main() {
	...
	dim3 threadsPerBlock(BLOCK_SIZE, BLOCK_SIZE);
	dim3 numBlocks(M/threadsPerBlock.x + (M % threadsPerBlock.x != 0),
				N/threadsPerBlock.y + (N % threadsPerBlock.y != 0));

	matmul<<<numBlocks, threadsPerBlock>>>(C, A, B, M, L, N);
}
```
- Some issues with this algorithm are that it has poor data re-use.
	- Every value of A & B are loaded from the global DRAM.
	- Both A and B are read multiple times.
- A much faster version is introduced farther down.

# Manipulating Memory for Performance
- Global Memory: the "main memory" on a CPU (host).
	- Allocated using `cudaMalloc`
- Shared Memory: very fast memory located in the SMs (within the GPU/device).
	- All threads in a block can access this shared memory.
	- Allocated in the kernel using `__shared__`
	- ![[Pasted image 20241020155441.png|400]]
- Local Memory: everything on the stack that can't fit in registers.
	- *Stored in global memory*.
	- Private to each thread.
	- ![[Pasted image 20241020155502.png|400]]
## Common Kernel Patterns
- Copy from Global to Shared Memory.
- `__syncthreads()`: synchronizes all threads in a block.
- Perform computation in shared memory.
	- `__syncthreads()`: as needed.
- Copy from Shared to Global Memory.
## Reversing Array Using Shared Memory
```cpp
__global__ void reverse (int *vec) {
	__shared__ int sharedVec[N];

	int idx = threadIdx.x;
	int idxReversed = N - idx - 1;

	sharedVec[idx] = vec[idx];
	__syncthreads();
	vec[idx] = sharedVec[idxReversed];
}
```
## Matrix Multiply (now with shared memory)
- Each (i, j) block computes a sub-block $C_{ij}$ .
- Individual threads in the block work on different elements of sub-block $C_{ij}$
![[Pasted image 20241020155916.png]]
## Unified Memory
- Data is allocated both on the CPU and GPU.
- GPU takes care of the synchronization.
```cpp
void sortfile(FILE *fp, int N) {
	char *data;
	cudaMallocManaged(&data, N);

	fread(data, 1, N, fp);
	qsort<<<...>>>(data, N, 1, compare); // activating kernel to execute
	cudaDeviceSynchronize();

	... use data on CPU ..
	cudaFree(data);
}
```
# CUDA Streams
- Streams can be used to perform multiple CUDA operations simultaneously.
	- CUDA kernel <<<>>>
	- `cudaMemcpyAsync()`
	- Operations on the CPI
```cpp
int main() {
	cudaStream_t stream;
	cudaStreamCreate(&stream);
	...

	kernel<<<grid, block, 0, stream>>>(x, b);
}
```
## Serial vs Concurrent Execution in Streams
![[Pasted image 20241020160534.png]]
## Launching All Streams at Once
```cpp
cudaStream_t stream[nStreams];
for (int i = 0; i < nStreams; i++) {
	cudaStreamCreate(&stream[i]);
}

for (int i = 0; i < nStreams; i++) {
	int offset = i * streamSize;
	cudaMemcpyAsync(&d_a[offset], &a[offset], streamBytes, cudaMemcpyHostToDevice, stream[i]);

	kernel<<<streamSize/blockSize, blockSize, 0, stream[i]>>>(d_a, offest);

	cudaMemcpyAsync(&a[offset], &d_a[offset], streamBytes, cudaMemcpyDeviceToHost, stream[i]);
}
```

# CUDA Libraries
- Linear Algebra:
	- cuBLAS, CUTLASS, cuSPARSE
- Deep Learning
	- cuDNN
- Graphics
	- OpenCV, OpenGL, FFmpeg
# Profiling GPUs
- HPCToolkit + Hatchet
	- Add `-e gpu=nvidia` to hpcrun command.
- Nsight Systems
	- Whole applciation tracing.
- Nsight Compute
	- Kernel-level profiling.
## Nsight Systems
- Outputs a .qdrep file that can be visualized in the Nsight GUI
	- `nsys profile -t {cuda, nvtx, openmp, mpi, ...}`
![[Pasted image 20241020160325.png]]

