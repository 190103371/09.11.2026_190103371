# Task 16: Dense Matrix Multiplication Scaling (GEMM)

## 1. Introduction

Dense General Matrix Multiplication (GEMM) is a fundamental operation in scientific computing, numerical simulation, and machine learning.

For two square matrices of dimension \(N \times N\), matrix multiplication is defined as:

$$
C=A\times B
$$

Each element of the resulting matrix is calculated as:

$$
C_{ij}=\sum_{k=1}^{N}A_{ik}B_{kj}
$$

The computational complexity of dense matrix multiplication is:

$$
W(N)=O(N^3)
$$

The memory requirement for storing the input and output matrices is:

$$
M(N)=O(N^2)
$$

Under Gustafson's weak-scaling assumption, the workload increases with the number of processors while execution time remains approximately constant.

The objective is to determine how matrix dimension \(N\) must grow as processor count \(P\) increases.

## 2. Computational Workload

For classical dense matrix multiplication, the approximate number of floating-point operations is:

$$
W(N)\approx 2N^3
$$

The factor of two accounts for one multiplication and one addition for each term in the matrix product.

Suppose the system contains \(P\) processors with approximately equal computational performance.

Under ideal parallel execution, the workload is distributed across the processors:

$$
W_{\text{processor}}(N,P)\approx\frac{2N^3}{P}
$$

To maintain constant execution time, the computational workload assigned to each processor must remain approximately constant.

Therefore:

$$
\frac{2N^3}{P}=K
$$

where \(K\) is a constant representing the workload per processor.

## 3. Derivation of Matrix Dimension Growth

Starting with:

$$
\frac{2N^3}{P}=K
$$

Rearranging:

$$
2N^3=KP
$$

Dividing by two:

$$
N^3=\frac{K}{2}P
$$

Taking the cube root:

$$
N=\sqrt[3]{\frac{K}{2}P}
$$

Since \(K/2\) is constant:

$$
\boxed{N(P)=\Theta(P^{1/3})}
$$

Therefore, the matrix dimension must increase proportionally to the cube root of processor count to maintain constant computational work per processor.

## 4. Relative Matrix Dimension

Let the initial system contain \(P_0\) processors and use a matrix dimension \(N_0\).

For a larger system with \(P\) processors:

$$
\frac{N(P)^3}{P}=\frac{N_0^3}{P_0}
$$

Rearranging:

$$
\frac{N(P)^3}{N_0^3}=\frac{P}{P_0}
$$

Taking the cube root:

$$
\boxed{
N(P)=N_0\left(\frac{P}{P_0}\right)^{1/3}
}
$$

This equation provides the matrix dimension required to maintain constant computational workload per processor.

## 5. Scaling Examples

Assume a baseline configuration with:

$$
P_0=1,\qquad N_0=100
$$

The matrix dimension for each processor count is:

$$
N(P)=100P^{1/3}
$$

| Processor Count | Matrix Dimension \(N\) | Relative Matrix Size |
|---:|---:|---:|
| 1 | 100 | 1 |
| 8 | 200 | 8 |
| 27 | 300 | 27 |
| 64 | 400 | 64 |
| 125 | 500 | 125 |
| 512 | 800 | 512 |
| 1000 | 1000 | 1000 |

The total computational workload grows proportionally with processor count:

$$
W(P)\propto N(P)^3\propto P
$$

This preserves approximately constant computational work per processor.

## 6. Memory Scaling

Dense matrix multiplication requires storage for matrices \(A\), \(B\), and \(C\).

Each matrix contains:

$$
N^2
$$

elements.

For three matrices, the total number of stored elements is:

$$
M_{\text{elements}}=3N^2
$$

If each element occupies \(b\) bytes:

$$
M(N)=3bN^2
$$

Since:

$$
N(P)=\Theta(P^{1/3})
$$

memory grows as:

$$
M(P)=\Theta(P^{2/3})
$$

Therefore:

$$
\boxed{M(P)\propto P^{2/3}}
$$

The computational workload grows linearly with processor count, while the total matrix storage grows proportionally to \(P^{2/3}\).

This indicates that computational work grows faster than the storage requirement under this idealized scaling model.

## 7. Computational Intensity

Computational intensity describes the amount of computation performed relative to the amount of data transferred.

For classical matrix multiplication:

$$
I=\frac{\text{Floating-point operations}}{\text{Memory traffic}}
$$

The approximate arithmetic operation count is:

$$
O(N^3)
$$

while the basic matrix storage requirement is:

$$
O(N^2)
$$

At a simplified asymptotic level:

$$
I=O(N)
$$

As matrix dimension increases, the amount of computation grows faster than the amount of stored matrix data.

However, actual arithmetic intensity depends on cache blocking, data reuse, and memory traffic. Matrix storage alone is not sufficient to determine exact memory bandwidth requirements.

## 8. Practical Limitations

The derivation assumes ideal parallel execution and constant workload per processor.

Real GEMM implementations may experience additional costs:

1. **Communication overhead:** Distributed matrix multiplication requires exchanging matrix blocks between processors.

2. **Memory bandwidth:** Processors may compete for access to memory.

3. **Load imbalance:** Uneven work distribution can leave some processors idle.

4. **Synchronization:** Distributed matrix operations may require coordination between computational stages.

5. **Hardware limitations:** Available memory and processor performance may restrict the maximum matrix dimension.

Consequently, the theoretical scaling relationship does not guarantee constant execution time in a real distributed system.

## 9. Conclusion

Dense matrix multiplication has computational complexity:

$$
W(N)=O(N^3)
$$

Under ideal weak scaling, total computational work must increase proportionally with processor count:

$$
W(P)\propto P
$$

Since matrix multiplication performs cubic work with respect to matrix dimension:

$$
N^3\propto P
$$

Therefore, the required matrix dimension grows as:

$$
\boxed{N(P)=\Theta(P^{1/3})}
$$

The total memory requirement grows as:

$$
\boxed{M(P)=\Theta(P^{2/3})}
$$

This demonstrates that dense matrix multiplication can support larger problem dimensions as more processors become available while maintaining approximately constant computational work per processor.

In practice, communication, memory bandwidth, and synchronization must also be considered when evaluating actual weak-scaling performance.