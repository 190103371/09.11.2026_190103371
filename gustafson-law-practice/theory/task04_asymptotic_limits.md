# Task 04: Asymptotic Analysis of Parallel Efficiency

## 1. Introduction

Parallel efficiency measures how effectively processing resources are utilized in a parallel computing system.

This task analyzes the asymptotic behavior of efficiency under Gustafson's Law and Amdahl's Law as the number of processors approaches infinity.

## 2. Gustafson's Parallel Efficiency

Gustafson's scaled speedup is defined as:

$$
S_G(P)=P-\alpha(P-1)
$$

Parallel efficiency is calculated as:

$$
E_G(P)=\frac{S_G(P)}{P}
$$

Substituting the speedup formula:

$$
E_G(P)=\frac{P-\alpha(P-1)}{P}
$$

Expanding the numerator:

$$
E_G(P)=\frac{P-\alpha P+\alpha}{P}
$$

Dividing each term by P:

$$
E_G(P)=1-\alpha+\frac{\alpha}{P}
$$

## 3. Gustafson's Asymptotic Limit

We calculate the limit as the number of processors approaches infinity:

$$
\lim_{P\to\infty}E_G(P)
=
\lim_{P\to\infty}\left(1-\alpha+\frac{\alpha}{P}\right)
$$

Since α is constant:

$$
\lim_{P\to\infty}\frac{\alpha}{P}=0
$$

Therefore:

$$
\boxed{\lim_{P\to\infty}E_G(P)=1-\alpha}
$$

This result demonstrates that Gustafson's model maintains a non-zero limiting efficiency when the serial fraction is smaller than 1.

## 4. Amdahl's Parallel Efficiency

Amdahl's speedup formula is:

$$
S_A(P)=\frac{1}{s+\frac{1-s}{P}}
$$

Parallel efficiency is:

$$
E_A(P)=\frac{S_A(P)}{P}
$$

Substituting:

$$
E_A(P)=\frac{1}{P\left(s+\frac{1-s}{P}\right)}
$$

Simplifying:

$$
E_A(P)=\frac{1}{Ps+(1-s)}
$$

## 5. Amdahl's Asymptotic Limit

Taking the limit as P approaches infinity:

$$
\lim_{P\to\infty}E_A(P)
=
\lim_{P\to\infty}\frac{1}{Ps+(1-s)}
$$

For a fixed positive serial fraction s, the denominator approaches infinity.

Therefore:

$$
\boxed{\lim_{P\to\infty}E_A(P)=0}
$$

This means that efficiency approaches zero as the number of processors increases indefinitely under Amdahl's fixed-size scaling assumptions.

## 6. Comparative Analysis

The two models produce different asymptotic efficiency limits.

Gustafson's Law:

$$
\lim_{P\to\infty}E_G(P)=1-\alpha
$$

Amdahl's Law:

$$
\lim_{P\to\infty}E_A(P)=0
$$

Gustafson's model assumes that workload size increases with processor count while execution time remains approximately constant.

Amdahl's model assumes a fixed workload, meaning that the serial portion increasingly limits the utilization of additional processors.

## 7. Conclusion

The asymptotic analysis demonstrates that Gustafson's Law can maintain a non-zero parallel efficiency as processor count increases.

In contrast, Amdahl's Law predicts that parallel efficiency approaches zero for any fixed positive serial fraction.

These results highlight the importance of workload characteristics when evaluating the scalability of parallel computing systems.