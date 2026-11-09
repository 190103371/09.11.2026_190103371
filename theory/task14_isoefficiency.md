# Task 14: Isoefficiency Relation with Gustafson Scaling

## 1. Introduction

The isoefficiency function describes how quickly the problem size must increase as the number of processors increases in order to maintain constant parallel efficiency.

It is an important metric in parallel computing because adding processors does not automatically guarantee that an application will use them efficiently.

Gustafson's Law describes weak scaling, where the workload grows with processor count while parallel execution time remains approximately constant.

The purpose of this analysis is to connect Gustafson's scaling model with the isoefficiency concept and determine the workload growth required to maintain constant efficiency.

## 2. Parallel Efficiency and Overhead

Parallel efficiency is defined as:

$$
E(P)=\frac{S(P)}{P}
$$

where:

- \(E(P)\) is parallel efficiency.
- \(S(P)\) is speedup.
- \(P\) is the number of processors.

Let:

- \(W\) be the total useful computational work.
- \(T_P\) be the parallel execution time.
- \(T_1=W\) be the normalized sequential execution time.

The parallel speedup is:

$$
S(P)=\frac{W}{T_P}
$$

Therefore:

$$
E(P)=\frac{W}{PT_P}
$$

The total parallel overhead is defined as:

$$
T_o(P,W)=PT_P-W
$$

Rearranging:

$$
PT_P=W+T_o(P,W)
$$

Substituting into the efficiency equation:

$$
\boxed{
E(P)=\frac{W}{W+T_o(P,W)}
}
$$

This equation demonstrates that efficiency depends on the relationship between useful computational work and parallel overhead.

## 3. Deriving the Isoefficiency Condition

The objective of isoefficiency analysis is to maintain a constant efficiency \(E_0\), where:

$$
0<E_0<1
$$

Starting with:

$$
E_0=\frac{W}{W+T_o(P,W)}
$$

Multiply both sides:

$$
E_0(W+T_o)=W
$$

Expand:

$$
E_0W+E_0T_o=W
$$

Rearrange:

$$
E_0T_o=W(1-E_0)
$$

Therefore:

$$
\boxed{
W=\frac{E_0}{1-E_0}T_o(P,W)
}
$$

For a fixed target efficiency, the coefficient is constant.

Consequently:

$$
\boxed{
W=\Theta(T_o(P,W))
}
$$

This is the fundamental isoefficiency condition.

The problem size must grow at the same asymptotic rate as the parallel overhead to preserve constant efficiency.

## 4. Gustafson's Law and Weak Scaling

Gustafson's scaled speedup is:

$$
S_G(P)=P-\alpha(P-1)
$$

An equivalent expression is:

$$
S_G(P)=\alpha+(1-\alpha)P
$$

Parallel efficiency is:

$$
E_G(P)=\frac{S_G(P)}{P}
$$

Substituting:

$$
E_G(P)=\frac{\alpha+(1-\alpha)P}{P}
$$

Therefore:

$$
\boxed{
E_G(P)=1-\alpha+\frac{\alpha}{P}
}
$$

Taking the limit as the number of processors approaches infinity:

$$
\lim_{P\to\infty}E_G(P)=1-\alpha
$$

If the serial fraction remains constant, the efficiency approaches a nonzero constant.

Under ideal weak scaling, each processor performs approximately the same amount of useful work.

Let \(w_0\) represent the workload assigned to one processor.

Then:

$$
W(P)=Pw_0
$$

Therefore:

$$
\boxed{
W(P)=\Theta(P)
}
$$

The total workload must grow linearly with processor count.

## 5. Isoefficiency Function of an Ideal Gustafson-Compliant Program

Consider an ideal weak-scaling application where:

1. The workload per processor remains constant.
2. The serial fraction \(\alpha\) remains constant.
3. Communication and synchronization overhead do not increase enough to reduce efficiency.
4. The total workload grows proportionally with processor count.

The total workload is:

$$
W(P)=Pw_0
$$

The efficiency is:

$$
E_G(P)=1-\alpha+\frac{\alpha}{P}
$$

For a fixed \(\alpha\), this efficiency approaches:

$$
E_0=1-\alpha
$$

Thus, efficiency remains asymptotically constant.

The corresponding ideal isoefficiency growth rate is:

$$
\boxed{
W(P)=\Theta(P)
}
$$

This means that an ideal Gustafson-compliant weak-scaling program requires a linear increase in total workload as processor count increases.

For example:

| Processor Count | Relative Workload | Workload per Processor |
|---:|---:|---:|
| 1 | 1 | 1 |
| 2 | 2 | 1 |
| 4 | 4 | 1 |
| 8 | 8 | 1 |
| 16 | 16 | 1 |
| 32 | 32 | 1 |
| 64 | 64 | 1 |

The workload per processor remains constant while the total workload increases linearly.

## 6. Effect of Additional Parallel Overhead

The ideal linear relationship does not automatically hold in real parallel applications.

Suppose parallel overhead grows according to:

$$
T_o(P)=cP^k
$$

where:

- \(c\) is a positive constant.
- \(k\) describes the overhead growth rate.

The isoefficiency condition requires:

$$
W(P)=\Theta(T_o(P))
$$

Therefore:

$$
\boxed{
W(P)=\Theta(P^k)
}
$$

Different overhead growth rates produce different isoefficiency requirements.

| Overhead Growth | Required Workload Growth |
|---|---|
| \(T_o(P)=\Theta(1)\) | \(W(P)=\Theta(1)\) |
| \(T_o(P)=\Theta(P)\) | \(W(P)=\Theta(P)\) |
| \(T_o(P)=\Theta(P\log P)\) | \(W(P)=\Theta(P\log P)\) |
| \(T_o(P)=\Theta(P^2)\) | \(W(P)=\Theta(P^2)\) |

If communication and synchronization overhead increase faster than the workload, maintaining constant efficiency requires a faster increase in problem size.

This may become impractical because memory capacity, communication bandwidth, and available data may limit workload growth.

## 7. Analytical Critique

The ideal Gustafson model predicts linear workload growth:

$$
W(P)=\Theta(P)
$$

However, the isoefficiency framework shows that actual scalability also depends on the overhead function.

Several limitations must be considered.

**1. Communication overhead**

Distributed applications may require increasing amounts of data exchange as the number of processors grows.

If communication overhead grows faster than linearly, a linear workload increase may not preserve constant efficiency.

**2. Synchronization overhead**

Applications with frequent synchronization may experience increasing processor waiting time.

This can increase total parallel overhead and reduce efficiency.

**3. Memory limitations**

Weak scaling assumes that each processor can maintain its share of the growing workload.

Insufficient memory or memory bandwidth can prevent this assumption from holding.

**4. Load imbalance**

If the workload is distributed unevenly, some processors may remain idle while others continue processing.

The resulting overhead can reduce achievable efficiency.

**5. Constant serial fraction assumption**

Gustafson's model assumes that the measured serial fraction remains constant.

In real systems, the serial fraction may change as communication, synchronization, and data-management costs increase.

Therefore, the linear isoefficiency result applies to the idealized weak-scaling case rather than every parallel algorithm.

## 8. Comparison with Amdahl's Law

Amdahl's Law assumes a fixed total workload.

Its speedup is:

$$
S_A(P)=\frac{1}{\alpha+\frac{1-\alpha}{P}}
$$

As processor count increases, its efficiency is:

$$
E_A(P)=\frac{1}{1+\frac{\alpha P}{1-\alpha}}
$$

For a nonzero serial fraction:

$$
\lim_{P\to\infty}E_A(P)=0
$$

The fixed workload does not grow with processor count, while the serial component limits the benefit of additional processors.

Gustafson's Law instead allows:

$$
W(P)=\Theta(P)
$$

This makes it possible to maintain a nonzero asymptotic efficiency under the assumption of a constant serial fraction.

The difference is that Amdahl's Law studies how quickly a fixed problem can be solved, while Gustafson's Law studies how much larger a problem can be solved in approximately the same time.

## 9. Conclusion

The isoefficiency function connects parallel workload growth with the overhead required to maintain a target efficiency.

Starting from:

$$
E(P)=\frac{W}{W+T_o(P,W)}
$$

the constant-efficiency condition is:

$$
W=\frac{E_0}{1-E_0}T_o(P,W)
$$

Therefore, the workload must grow at the same asymptotic rate as parallel overhead.

For an ideal Gustafson-compliant weak-scaling program, the workload per processor remains constant:

$$
W(P)=Pw_0
$$

Consequently, the ideal workload growth rate is:

$$
\boxed{
W(P)=\Theta(P)
}
$$

This result demonstrates that linear problem growth is sufficient to maintain constant efficiency when the serial fraction and overhead behavior satisfy the ideal weak-scaling assumptions.

However, Gustafson's Law alone does not determine the isoefficiency function of every real application. A precise result requires an explicit model of communication, synchronization, and other parallel overheads.

The practical conclusion is that scalable parallel software must increase useful workload while controlling the growth of overhead. Linear workload growth is an ideal target, not a universal guarantee of constant efficiency.