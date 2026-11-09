# Task 08: Cross-Over Point Determination (Amdahl vs. Gustafson)

## 1. Introduction

This task compares the speedup predictions of Amdahl's Law and Gustafson's Law using the same serial fraction and processor count.

The objective is to calculate both speedup values and evaluate the percentage discrepancy between the models.

## 2. Given Parameters

The serial fraction is:

$$
s=\alpha=0.08
$$

The number of processors is:

$$
P=128
$$

## 3. Amdahl's Law Calculation

Amdahl's speedup formula is:

$$
S_A(P)=\frac{1}{s+\frac{1-s}{P}}
$$

Substituting the given values:

$$
S_A(128)=\frac{1}{0.08+\frac{0.92}{128}}
$$

Calculating the parallel fraction per processor:

$$
\frac{0.92}{128}=0.0071875
$$

Therefore:

$$
S_A(128)=\frac{1}{0.0871875}
$$

The resulting speedup is:

$$
\boxed{S_A(128)\approx11.4695}
$$

## 4. Gustafson's Law Calculation

Gustafson's scaled speedup formula is:

$$
S_G(P)=P-\alpha(P-1)
$$

Substituting the given values:

$$
S_G(128)=128-0.08(128-1)
$$

$$
S_G(128)=128-0.08(127)
$$

$$
S_G(128)=128-10.16
$$

Therefore:

$$
\boxed{S_G(128)=117.84}
$$

## 5. Percentage Discrepancy Between Models

To compare the predictions, the percentage discrepancy is calculated relative to Amdahl's speedup.

The formula is:

$$
D=\frac{S_G-S_A}{S_A}\times100\%
$$

Substituting the calculated values:

$$
D=\frac{117.84-11.4695}{11.4695}\times100\%
$$

Therefore:

$$
\boxed{D\approx927.42\%}
$$

Gustafson's predicted speedup is approximately 927.42% higher than Amdahl's prediction.

## 6. Comparative Analysis

The two models produce significantly different speedup predictions.

Amdahl's Law assumes a fixed problem size. The serial portion remains a limitation as additional processors are introduced.

Gustafson's Law assumes that the problem size increases with the number of processors while execution time remains approximately constant.

Consequently, Gustafson's model predicts much higher scaled speedup for applications capable of expanding their workloads.

The difference becomes particularly significant as processor count increases.

## 7. Why the Models Diverge

The main reason for divergence is the different workload scaling assumption.

Amdahl's Law focuses on reducing execution time for a fixed computational workload.

Gustafson's Law focuses on processing larger workloads using additional computing resources.

Therefore, the two models describe different performance scenarios rather than competing measurements of an identical workload.

## 8. Conclusion

For 128 processors and an 8% serial fraction, Amdahl's Law predicts a speedup of approximately 11.4695, while Gustafson's Law predicts 117.84.

The percentage discrepancy relative to Amdahl's prediction is approximately 927.42%.

This comparison demonstrates the importance of distinguishing between strong scaling and weak scaling when evaluating parallel computing performance.