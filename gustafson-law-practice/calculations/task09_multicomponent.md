# Task 09: Multi-Component Workload Partitioning

## 1. Introduction

This task analyzes a scientific simulation consisting of three computational phases.

The objective is to identify the aggregate serial fraction and calculate Gustafson's scaled speedup for two processor configurations.

## 2. Workload Breakdown

The simulation consists of three phases:

| Phase | Description | Execution Time |
|---|---|---:|
| A | Initial disk I/O and parameter broadcast | 2% |
| B | Boundary message-passing synchronization | 6% |
| C | Core domain flux computation | 92% |

The first two phases represent serial and synchronization overhead in this simplified model.

The third phase represents the parallel computational workload.

## 3. Aggregate Serial Fraction Calculation

The aggregate serial fraction is calculated by combining the first two phases:

$$
\alpha=0.02+0.06
$$

Therefore:

$$
\boxed{\alpha=0.08=8\%}
$$

The parallel fraction is:

$$
p=0.92=92\%
$$

The total workload is:

$$
0.02+0.06+0.92=1.00
$$

## 4. Gustafson's Law Formulation

Gustafson's scaled speedup formula is:

$$
S(P)=P-\alpha(P-1)
$$

Substituting the aggregate serial fraction:

$$
S(P)=P-0.08(P-1)
$$

Expanding:

$$
\boxed{S(P)=0.92P+0.08}
$$

## 5. Speedup Calculation for 512 Processors

Given:

$$
P=512,\quad \alpha=0.08
$$

Substituting:

$$
S(512)=512-0.08(512-1)
$$

$$
S(512)=512-0.08(511)
$$

$$
S(512)=512-40.88
$$

Therefore:

$$
\boxed{S(512)=471.12}
$$

## 6. Speedup Calculation for 2048 Processors

Given:

$$
P=2048,\quad \alpha=0.08
$$

Substituting:

$$
S(2048)=2048-0.08(2048-1)
$$

$$
S(2048)=2048-0.08(2047)
$$

$$
S(2048)=2048-163.76
$$

Therefore:

$$
\boxed{S(2048)=1884.24}
$$

## 7. Comparative Results

The calculated results are summarized below:

| Processor Count | Scaled Speedup |
|---:|---:|
| 512 | 471.12 |
| 2048 | 1884.24 |

When the processor count increases from 512 to 2048, the scaled speedup increases significantly.

The speedup remains approximately 92% of the ideal linear speedup because the serial fraction is 8%.

## 8. Performance Analysis

The simulation spends 8% of its normalized execution time on initial I/O and synchronization overhead.

The remaining 92% is dedicated to parallel domain flux computation.

As the number of processors increases, the parallel workload can scale while the serial fraction remains constant under the Gustafson model.

However, in real distributed systems, communication and synchronization overhead may change with processor count.

Therefore, the calculated results represent an idealized theoretical model.

## 9. Conclusion

The aggregate serial fraction of the scientific simulation is 8%, while the parallel fraction is 92%.

Using Gustafson's Law, the scaled speedup is 471.12 for 512 processors and 1884.24 for 2048 processors.

These results demonstrate how workload partitioning and serial overhead influence the scalability of scientific simulations.