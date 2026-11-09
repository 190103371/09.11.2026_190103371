# Task 11: The Sun and Ni Memory-Bounded Speedup Comparison

## 1. Introduction

Parallel performance can be evaluated under different assumptions about problem size and available hardware resources. Amdahl's Law assumes a fixed problem size, while Gustafson's Law assumes that the workload grows with the number of processors while parallel execution time remains approximately constant.

Sun and Ni introduced the Memory-Bounded Speedup model to include another important resource: memory capacity. Their model connects problem scalability to the amount of memory available in a parallel system.

The key distinction is that adding processors does not automatically mean that the application can increase its workload proportionally. The actual growth depends on memory capacity and the relationship between memory and computational work.

## 2. Amdahl's Law: Fixed-Size Model

Amdahl's Law describes speedup when the total problem size remains constant.

Its mathematical formulation is:

$$
S_A(P)=\frac{1}{\alpha+\frac{1-\alpha}{P}}
$$

where:

- \(P\) is the number of processors.
- \(\alpha\) is the serial fraction.
- \(1-\alpha\) is the parallelizable fraction.

As the number of processors approaches infinity:

$$
\lim_{P\to\infty}S_A(P)=\frac{1}{\alpha}
$$

This means that speedup is limited by the serial portion of the program.

Amdahl's model is useful when the workload cannot be expanded and the objective is to complete the same problem faster.

## 3. Gustafson's Law: Fixed-Time Model

Gustafson's Law assumes that the problem size increases as more processors become available. The parallel execution time remains approximately constant.

The scaled speedup is:

$$
S_G(P)=P-\alpha(P-1)
$$

An equivalent expression is:

$$
S_G(P)=\alpha+(1-\alpha)P
$$

The model assumes that additional processing capacity can be used to solve a proportionally larger problem.

For a large processor count, the speedup can approach the number of processors multiplied by the parallel fraction:

$$
\lim_{P\to\infty}S_G(P)\approx(1-\alpha)P
$$

Unlike Amdahl's Law, Gustafson's model does not impose a fixed upper speedup limit when the serial fraction is less than one.

However, it assumes that the workload can grow sufficiently with the available processing capacity.

## 4. Sun-Ni Memory-Bounded Speedup Model

The Sun-Ni model introduces memory capacity as a constraint on problem growth.

Let:

- \(P\) be the number of processors or nodes.
- \(f\) be the parallelizable fraction of the baseline workload.
- \(1-f\) be the sequential fraction.
- \(G(P)\) be the workload growth factor enabled by the increase in available memory.

The simplified Memory-Bounded Speedup formulation is:

$$
S_{MB}(P)=
\frac{(1-f)+fG(P)}
{(1-f)+\frac{fG(P)}{P}}
$$

The numerator represents the execution time of the expanded workload on one processor.

The denominator represents the execution time of the same expanded workload distributed across \(P\) processors, assuming ideal parallel execution and no additional communication overhead.

The function \(G(P)\) describes how much the parallelizable workload grows when memory capacity increases.

If the workload is directly proportional to available memory, then:

$$
G(P)=P
$$

If the workload remains fixed, then:

$$
G(P)=1
$$

This makes the Sun-Ni model a general framework connecting fixed-size and fixed-time speedup.

## 5. Relationship Between the Three Models

### Case A: Fixed-Size Problem

When the workload does not grow:

$$
G(P)=1
$$

Substituting into the Memory-Bounded Speedup formula:

$$
S_{MB}(P)=
\frac{(1-f)+f}
{(1-f)+\frac{f}{P}}
$$

Since the fractions sum to one:

$$
S_{MB}(P)=\frac{1}{(1-f)+\frac{f}{P}}
$$

This is Amdahl's Law, with \(1-f\) representing the serial fraction.

### Case B: Fixed-Time Problem

When the workload grows proportionally with the processor count:

$$
G(P)=P
$$

Substituting:

$$
S_{MB}(P)=
\frac{(1-f)+fP}
{(1-f)+f}
$$

Therefore:

$$
S_{MB}(P)=(1-f)+fP
$$

This is Gustafson's scaled speedup formula.

### General Case

For other values of \(G(P)\), the Sun-Ni model represents a workload whose growth is determined by memory capacity rather than being automatically fixed or proportional to processor count.

Consequently, Amdahl's Law and Gustafson's Law can be treated as special cases of the simplified Memory-Bounded Speedup model.

## 6. How Memory Capacity Determines Workload Growth

Suppose the total memory capacity of a parallel system increases proportionally with the number of nodes:

$$
M(P)=P\cdot M(1)
$$

The workload growth factor depends on the relationship between computational work and memory:

$$
G(P)=\frac{g(M(P))}{g(M(1))}
$$

where \(g(M)\) describes the workload that can be supported by memory capacity \(M\).

For example, if workload growth follows:

$$
g(M)=cM^b
$$

then:

$$
G(P)=P^b
$$

The exponent \(b\) determines how quickly the workload grows relative to memory capacity.

- If \(b=1\), workload grows linearly with memory.
- If \(0<b<1\), workload grows more slowly than memory.
- If \(b>1\), workload grows faster than memory under the assumed workload model.

This demonstrates why processor count alone cannot fully predict achievable speedup.

## 7. When Gustafson's Law Overestimates Throughput

Gustafson's Law may overestimate achievable throughput when its assumption of proportional workload growth is not supported by the actual memory system.

Several conditions can cause this:

1. **Insufficient memory capacity:** The system cannot store the larger workload required by the assumed scaling model.

2. **Memory bandwidth limitations:** Additional processors compete for memory bandwidth, preventing computational throughput from increasing proportionally.

3. **Data replication:** Distributed applications may need to duplicate data across nodes, reducing the memory available for useful workload growth.

4. **Communication overhead:** Larger distributed workloads may require more data exchange and synchronization, increasing execution time.

5. **Uneven workload distribution:** Some processors may receive more work or memory-intensive tasks than others, reducing overall efficiency.

In these situations, the actual workload growth factor may satisfy:

$$
G(P)<P
$$

The workload then grows more slowly than the linear assumption made by Gustafson's model.

## 8. When Gustafson's Law Underestimates Throughput

Gustafson's Law can underestimate achievable throughput when the application or hardware allows performance improvements beyond the model's assumed linear workload growth.

Possible conditions include:

1. **Superlinear memory effects:** Additional memory may reduce disk access, swapping, or repeated data loading.

2. **Improved cache behavior:** A larger distributed memory hierarchy may allow more data to remain close to the processors.

3. **Algorithmic improvements:** A parallel implementation may use a more efficient algorithm or data structure than the original sequential implementation.

4. **Memory-enabled workload growth:** The memory system may support a larger useful workload than the linear growth assumed by Gustafson's model.

If the effective workload growth factor is greater than the processor count:

$$
G(P)>P
$$

the memory-bounded model may predict a speedup greater than the simplified Gustafson model, under the idealized assumptions of the formulation.

However, this does not guarantee superlinear performance in real systems. Communication, memory access costs, and other overheads must still be considered.

## 9. Comparative Summary

| Model | Main Assumption | Workload Growth | Main Limitation |
|---|---|---|---|
| Amdahl | Fixed total problem size | \(G(P)=1\) | Serial fraction limits speedup |
| Gustafson | Fixed parallel execution time | \(G(P)=P\) | Assumes proportional workload growth |
| Sun-Ni | Workload constrained by memory capacity | \(G(P)\) depends on memory | Requires a realistic memory-workload relationship |

The three models describe different operating conditions rather than competing universal predictions.

Amdahl's Law is appropriate for fixed workloads, Gustafson's Law is appropriate for fixed-time scaling, and Sun-Ni provides a broader framework when memory capacity determines how much the workload can grow.

## 10. Conclusion

The Sun-Ni Memory-Bounded Speedup model extends traditional parallel performance analysis by incorporating memory capacity into the relationship between processor count and workload size.

Its simplified formulation includes Amdahl's Law as the fixed-size case and Gustafson's Law as the fixed-time case.

The comparison demonstrates that adding processors is not sufficient to guarantee proportional throughput improvements. The memory system must also support the larger workload, and the application must be able to use that capacity efficiently.

Gustafson's Law may overestimate performance when memory capacity, bandwidth, communication, or synchronization limits workload growth. It may underestimate performance when additional memory or improved data locality enables more useful work than the linear scaling assumption predicts.

Therefore, memory capacity should be treated as a fundamental factor when evaluating the scalability of modern parallel and distributed systems.

## References

1. Sun, X.-H., and Ni, L. M. (1990). *Another View on Parallel Speedup*. Proceedings of Supercomputing '90, pp. 324-333. DOI: 10.1109/SUPERC.1990.130037.
2. Sun, X.-H., and Ni, L. M. (1993). *Scalable Problems and Memory-Bounded Speedup*. Journal of Parallel and Distributed Computing, 19(1), 27-37. DOI: 10.1006/jpdc.1993.1087.
3. Sun, X.-H., and Lu, X. (2023). *The Memory-Bounded Speedup Model and Its Impacts in Computing*. Journal of Computer Science and Technology, 38(1).