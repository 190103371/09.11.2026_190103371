# Task 07: Reverse Engineering Maximum Tolerable Serial Overhead

## 1. Introduction

This task determines the maximum serial fraction that a parallel computing system can tolerate while maintaining a required scaled speedup.

The system contains 8192 processing cores and must achieve a scaled speedup of at least 7500.

## 2. Given Parameters

The number of processors is:

$$
P=8192
$$

The minimum required scaled speedup is:

$$
S(P)\geq7500
$$

Gustafson's Law is:

$$
S(P)=P-\alpha(P-1)
$$

## 3. Algebraic Derivation

Substituting the given values:

$$
8192-\alpha(8192-1)\geq7500
$$

Simplifying:

$$
8192-8191\alpha\geq7500
$$

Subtracting 7500 from both sides:

$$
692\geq8191\alpha
$$

Dividing both sides by 8191:

$$
\alpha\leq\frac{692}{8191}
$$

Therefore, the maximum allowable serial fraction is:

$$
\boxed{\alpha_{\max}=\frac{692}{8191}}
$$

## 4. Numerical Calculation

Evaluating the expression:

$$
\alpha_{\max}\approx0.0844829691
$$

Converting to a percentage:

$$
\alpha_{\max}\times100\approx8.44829691\%
$$

Therefore:

$$
\boxed{\alpha_{\max}\approx8.4483\%}
$$

## 5. Verification

At the maximum allowable serial fraction:

$$
S(8192)=8192-0.0844829691(8191)
$$

$$
S(8192)\approx7500
$$

The required performance threshold is satisfied, subject to rounding.

Any larger serial fraction would reduce the scaled speedup below 7500.

## 6. Conclusion

The maximum tolerable serial fraction for a system with 8192 processors is approximately 8.4483%.

To maintain a scaled speedup of at least 7500, the serial portion of the parallel execution time must not exceed this threshold.

This calculation demonstrates how Gustafson's Law can be used to determine acceptable serial overhead in large parallel computing systems.