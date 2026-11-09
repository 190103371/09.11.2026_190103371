# Task 02: Formal Derivation of Gustafson's Scaled Speedup

## 1. Introduction

Gustafson's Law describes how parallel computing systems can achieve higher performance by increasing the problem size while maintaining approximately constant execution time.

This task derives the scaled speedup formula from the normalized execution time.

## 2. Initial Assumptions

Let:

- P be the number of processors.
- s be the serial execution time fraction.
- p be the parallel execution time fraction.

The total parallel execution time is normalized to 1:

\[
s+p=1
\]

Therefore:

\[
p=1-s
\]

## 3. Parallel Execution Time

On a parallel machine with P processors, the normalized execution time consists of two components:

\[
T_{parallel}=s+p=1
\]

The serial portion takes time s, while the parallel portion takes time p.

## 4. Equivalent Serial Workload

To execute the same total amount of computational work on a single processor, the parallel portion must be multiplied by P.

The equivalent serial execution time is:

\[
T_{serial}=s+Pp
\]

This is because all work performed simultaneously by P processors must be completed sequentially by one processor.

## 5. Derivation of Scaled Speedup

Speedup is defined as the ratio of serial execution time to parallel execution time:

\[
S(P)=\frac{T_{serial}}{T_{parallel}}
\]

Substituting the previous expressions:

\[
S(P)=\frac{s+Pp}{s+p}
\]

Since the normalized parallel execution time is equal to 1:

\[
S(P)=s+Pp
\]

Using:

\[
p=1-s
\]

We obtain:

\[
S(P)=s+P(1-s)
\]

Expanding:

\[
S(P)=s+P-Ps
\]

Rearranging:

\[
S(P)=P-s(P-1)
\]

## 6. General Gustafson's Law Formula

In Gustafson's formulation, the serial fraction measured on the parallel machine is represented by α.

Replacing s with α:

\[
\boxed{S(P)=P-\alpha(P-1)}
\]

The formula can also be written as:

\[
\boxed{S(P)=\alpha+(1-\alpha)P}
\]

Both expressions are mathematically equivalent.

## 7. Conclusion

The derivation demonstrates that Gustafson's scaled speedup depends on the number of processors and the serial fraction of the workload.

As the number of processors increases, the parallel portion of the workload can grow while execution time remains approximately constant.

Therefore, Gustafson's Law provides a mathematical foundation for weak scaling in parallel computing.