# Task 01: Strong Scaling vs. Weak Scaling Foundations

## 1. Introduction

Parallel computing allows multiple processors to execute different parts of a computational workload simultaneously.

Two fundamental models describe parallel performance: Amdahl's Law (1967) and Gustafson's Law (1988). They differ mainly in how problem size behaves when the number of processors increases.

## 2. Amdahl's Law (1967)

Amdahl's Law describes strong scaling, where the total problem size remains constant while the number of processors increases.

The speedup formula is:

\[
S_A(P)=\frac{1}{s+\frac{1-s}{P}}
\]

Where:
- P is the number of processors.
- s is the serial fraction of the workload.
- 1-s is the parallelizable fraction.

As the number of processors increases, the parallel portion becomes faster. However, the serial portion remains unchanged and limits the maximum achievable speedup.

When P approaches infinity:

\[
\lim_{P\to\infty}S_A(P)=\frac{1}{s}
\]

Therefore, Amdahl's Law predicts a theoretical speedup limit determined by the serial fraction.

## 3. Gustafson's Law (1988)

Gustafson's Law describes weak scaling, where the problem size increases with the number of available processors while total execution time remains approximately constant.

Its scaled speedup formula is:

\[
S_G(P)=P-\alpha(P-1)
\]

Where:
- P is the number of processors.
- α is the serial fraction measured on the parallel system.
- 1-α is the parallel fraction.

The formula can also be expressed as:

\[
S_G(P)=\alpha+(1-\alpha)P
\]

Unlike Amdahl's Law, Gustafson's Law considers the possibility of solving larger problems when additional processors become available.

## 4. Problem Size Behavior as P Approaches Infinity

The two models make different assumptions about workload growth.

**Amdahl's Law:**

The total problem size remains constant:

\[
W(P)=W_0
\]

Even when the number of processors approaches infinity, the workload does not increase.

**Gustafson's Law:**

The problem size increases proportionally with the number of processors under ideal weak scaling:

\[
W(P)=P\cdot W(1)
\]

As P approaches infinity, the theoretical workload also increases proportionally, assuming sufficient memory and computational resources.

## 5. Fixed-Size vs. Fixed-Time Scaling

Amdahl's Law is called fixed-size scaling because the same computational problem is distributed across an increasing number of processors.

Gustafson's Law is called fixed-time scaling because the execution time is approximately constant while the computational workload increases with the number of processors.

The main distinction is that Amdahl's Law focuses on completing a fixed task faster, whereas Gustafson's Law focuses on completing larger tasks within approximately the same time.

## 6. Conclusion

Amdahl's Law demonstrates that serial execution limits the speedup of a fixed-size workload.

Gustafson's Law shows that parallel computing can achieve near-linear scaled speedup when the workload grows with the number of processors.

Both models provide important perspectives for evaluating parallel computing systems. The appropriate model depends on whether the application uses strong scaling or weak scaling.