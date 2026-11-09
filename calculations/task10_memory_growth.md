# Task 10: Memory-Bounded Problem Growth

## 1. Introduction

Weak scaling describes a situation in which the workload increases proportionally with the number of processors. The goal is to keep the amount of work per processor approximately constant while the total problem size grows.

The workload is defined as:

$$
W(P)=P \cdot W(1)
$$

where:

- \(W(P)\) is the total workload on \(P\) processors.
- \(W(1)\) is the workload on one processor.
- \(P\) is the number of processors.

Under ideal weak scaling, each processor handles approximately the same amount of work.

## 2. Memory Growth

In a memory-bounded system, each processor contributes additional memory capacity.

The aggregate memory capacity is:

$$
M(P)=c \cdot P
$$

where \(c\) is the memory capacity of one processor or node.

For this task, each node has 32 GB of memory:

$$
c=32\text{ GB}
$$

Therefore:

$$
M(P)=32P\text{ GB}
$$

This means that increasing the number of nodes increases the total available memory proportionally.

## 3. Maximum Problem Dimension for a 3D Grid

A three-dimensional grid contains:

$$
N^3
$$

grid cells, where \(N\) is the number of cells along each dimension.

If each grid cell requires \(b\) bytes of memory, the total memory requirement is:

$$
M_{\text{grid}}=bN^3
$$

The memory constraint is:

$$
bN^3 \leq 32 \cdot 10^9 \cdot P
$$

Solving for the maximum grid dimension:

$$
N_{\max}=\sqrt[3]{\frac{32\cdot10^9\cdot P}{b}}
$$

The value of \(b\), the number of bytes required per grid cell, is not specified in the task sheet. Therefore, an exact numerical grid dimension cannot be calculated without an additional assumption.

## 4. Calculation for 64 Nodes

For 64 nodes:

$$
M(64)=32\cdot64=2048\text{ GB}
$$

Using the memory constraint:

$$
N_{\max,64}=\sqrt[3]{\frac{2048\cdot10^9}{b}}
$$

Therefore, the maximum grid dimension depends on the memory required by each grid cell.

## 5. Calculation for 512 Nodes

For 512 nodes:

$$
M(512)=32\cdot512=16384\text{ GB}
$$

Using the same formula:

$$
N_{\max,512}=\sqrt[3]{\frac{16384\cdot10^9}{b}}
$$

The system with 512 nodes has eight times the aggregate memory capacity of the system with 64 nodes.

## 6. Comparison

The ratio of aggregate memory capacities is:

$$
\frac{M(512)}{M(64)}=\frac{16384}{2048}=8
$$

Since the grid is three-dimensional, its dimension grows with the cube root of the available memory:

$$
\frac{N_{\max,512}}{N_{\max,64}}=\sqrt[3]{8}=2
$$

Thus, increasing the number of nodes from 64 to 512 doubles the maximum linear dimension of the grid, assuming the same memory requirement per cell.

| Parameter | 64 Nodes | 512 Nodes |
|---|---:|---:|
| Memory per node | 32 GB | 32 GB |
| Total memory | 2,048 GB | 16,384 GB |
| Memory capacity ratio | 1 | 8 |
| Maximum grid dimension ratio | 1 | 2 |

## 7. Conclusion

Memory-bounded scaling allows a parallel system to solve increasingly larger problems as additional nodes contribute more memory.

For a three-dimensional grid, total memory grows proportionally to the number of nodes, while the maximum linear grid dimension grows according to the cube root of the total memory.

In this example, increasing the system from 64 to 512 nodes provides eight times more aggregate memory and allows the maximum linear grid dimension to double.

However, the exact numerical grid dimensions cannot be determined from the task sheet alone because the memory requirement per grid cell is not provided. The parameter \(b\) must be specified to calculate exact values.

This demonstrates that memory capacity is an important factor in weak scaling, but practical performance also depends on memory bandwidth, communication overhead, and computational efficiency.