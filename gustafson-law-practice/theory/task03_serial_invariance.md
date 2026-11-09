# Task 03: The Serial Fraction Invariance Paradox

## 1. Introduction

Amdahl's Law and Gustafson's Law use different reference systems when measuring the serial fraction of a parallel workload.

This difference explains why the two models can predict different speedup limits.

## 2. Serial Fraction in Amdahl's Law

In Amdahl's Law, the serial fraction s is measured on a uniprocessor system.

The total workload remains fixed, and the serial fraction represents the portion of the original computation that cannot be parallelized.

The speedup formula is:

\[
S_A(P)=\frac{1}{s+\frac{1-s}{P}}
\]

As P approaches infinity:

\[
\lim_{P\to\infty}S_A(P)=\frac{1}{s}
\]

Therefore, the serial fraction creates a theoretical upper limit on speedup.

## 3. Serial Fraction in Gustafson's Law

In Gustafson's Law, the serial fraction α is measured on the parallel machine.

The workload is scaled according to the available processing resources while execution time remains approximately constant.

The scaled speedup formula is:

\[
S_G(P)=P-\alpha(P-1)
\]

Here, α represents the observed serial portion of the execution time on the parallel system.

The measurement reflects the expanded workload rather than the original fixed-size problem.

## 4. Analytical Comparison

The two models use different reference frames.

Amdahl's Law evaluates a fixed workload using its uniprocessor serial fraction.

Gustafson's Law evaluates an expanded workload using the serial fraction observed during parallel execution.

The distinction can be expressed mathematically:

Amdahl:

\[
S_A(P)=\frac{1}{s+\frac{1-s}{P}}
\]

Gustafson:

\[
S_G(P)=P-\alpha(P-1)
\]

The formulas describe different scaling assumptions and should not be interpreted as identical measurements of the same workload.

## 5. Why the Apparent Paradox Is Resolved

The apparent contradiction arises when the serial fractions from the two models are treated as directly interchangeable.

In reality, the reference workload and measurement conditions differ.

Amdahl's Law assumes a fixed total problem size.

Gustafson's Law assumes that the problem size increases with the number of processors.

Consequently, a serial fraction measured on a parallel machine does not necessarily represent the same proportion as a serial fraction measured on a uniprocessor system.

## 6. Conclusion

The serial fraction is dependent on the workload and the measurement reference.

Amdahl's Law uses a uniprocessor reference for fixed-size scaling, while Gustafson's Law measures the serial fraction on the parallel system under scaled workload conditions.

Understanding this distinction is essential when comparing strong and weak scaling models.