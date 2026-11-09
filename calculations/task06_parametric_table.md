# Task 06: Parametric Speedup Across Processor Densities

## 1. Introduction

This task evaluates Gustafson's scaled speedup and parallel efficiency for different processor configurations.

The serial fraction of the parallel workload is:

$$
\alpha=0.04
$$

The processor counts are:

$$
P=16,\ 64,\ 256,\ 1024,\ 4096,\ 16384
$$

## 2. Mathematical Formulation

Gustafson's scaled speedup is defined as:

$$
S(P)=P-\alpha(P-1)
$$

Substituting the given serial fraction:

$$
S(P)=P-0.04(P-1)
$$

Expanding the expression:

$$
S(P)=P-0.04P+0.04
$$

Therefore:

$$
\boxed{S(P)=0.96P+0.04}
$$

Parallel efficiency is calculated as:

$$
E(P)=\frac{S(P)}{P}
$$

Substituting the speedup formula:

$$
E(P)=\frac{0.96P+0.04}{P}
$$

Therefore:

$$
\boxed{E(P)=0.96+\frac{0.04}{P}}
$$

## 3. Worked Numerical Example

For P = 16 processors:

Scaled speedup:

$$
S(16)=16-0.04(16-1)
$$

$$
S(16)=16-0.04(15)
$$

$$
S(16)=16-0.6
$$

$$
\boxed{S(16)=15.4}
$$

Parallel efficiency:

$$
E(16)=\frac{15.4}{16}
$$

$$
\boxed{E(16)=0.9625=96.25\%}
$$

## 4. Complete Calculation Table

Applying the same formulas to all processor configurations gives:

| Processors (P) | Scaled Speedup S(P) | Efficiency E(P) |
|---:|---:|---:|
| 16 | 15.4000 | 96.250000% |
| 64 | 61.4800 | 96.062500% |
| 256 | 245.8000 | 96.015625% |
| 1024 | 983.0800 | 96.003906% |
| 4096 | 3932.2000 | 96.000977% |
| 16384 | 15728.6800 | 96.000244% |

## 5. Efficiency Analysis

The results demonstrate that parallel efficiency decreases slightly as processor count increases.

At 16 processors, efficiency is 96.25%.

At 16384 processors, efficiency approaches 96.000244%.

The efficiency approaches the theoretical limit:

$$
\lim_{P\to\infty}E(P)=1-\alpha
$$

Substituting the given serial fraction:

$$
\lim_{P\to\infty}E(P)=1-0.04
$$

Therefore:

$$
\boxed{\lim_{P\to\infty}E(P)=0.96=96\%}
$$

The efficiency loss becomes progressively smaller as the number of processors increases.

## 6. Speedup Scaling Analysis

The scaled speedup increases almost linearly with processor count.

The theoretical relationship is:

$$
S(P)=0.96P+0.04
$$

For large P, the constant term 0.04 becomes negligible.

Therefore:

$$
S(P)\approx0.96P
$$

This demonstrates that the system maintains approximately 96% of ideal linear speedup under the assumed Gustafson model.

## 7. Conclusion

The numerical analysis demonstrates that a serial fraction of 4% produces high parallel efficiency under Gustafson's Law.

As processor count increases from 16 to 16384, scaled speedup grows almost linearly.

Parallel efficiency approaches 96% rather than zero.

These results illustrate the advantages of weak scaling for workloads that can expand proportionally with available processing resources.