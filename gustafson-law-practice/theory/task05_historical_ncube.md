# Task 05: Deconstructing the "Amdahl's Pessimism" Fallacy

## 1. Introduction

In 1988, John Gustafson, Gary Montry, and Robert Benner demonstrated that massively parallel computing systems could achieve very high scaled speedup.

Their research used a 1024-processor MIMD hypercube system at Sandia National Laboratories.

The experimental results challenged the common interpretation that Amdahl's Law severely limits the practical benefits of large parallel systems.

## 2. Historical Experimental Scenario

The researchers developed parallel implementations for scientific applications, demonstrating the advantages of scaled parallel workloads.

The experimental system contained 1024 processors.

The research demonstrated scaled speedups exceeding 1000 times, despite a serial fraction of approximately 0.4%.

These results showed that large parallel workloads could be processed efficiently when problem size increased with processor count.

## 3. Amdahl's Law Prediction

Amdahl's Law assumes a fixed problem size.

Its speedup formula is:

$$
S_A(P)=\frac{1}{s+\frac{1-s}{P}}
$$

For this comparison, assume:

$$
P=1024
$$

$$
s=0.004
$$

Substituting:

$$
S_A(1024)=\frac{1}{0.004+\frac{0.996}{1024}}
$$

Therefore:

$$
\boxed{S_A(1024)\approx201.1}
$$

This simplified fixed-size calculation predicts approximately 201 times speedup.

## 4. Gustafson's Law Prediction

Gustafson's Law uses a scaled workload model.

The speedup formula is:

$$
S_G(P)=P-\alpha(P-1)
$$

Using:

$$
P=1024,\quad \alpha=0.004
$$

We obtain:

$$
S_G(1024)=1024-0.004(1023)
$$

$$
S_G(1024)=1024-4.092
$$

Therefore:

$$
\boxed{S_G(1024)=1019.908}
$$

The theoretical result is approximately 1020 times speedup.

This is consistent with the scale of the experimental results reported in the research.

## 5. Fixed-Size vs. Scaled-Size Experimental Results

The research demonstrated that scaled speedup could exceed 1000 times on a 1024-processor system.

The difference comes from the workload scaling assumptions.

In fixed-size measurements, the total problem size remains constant.

In scaled-size measurements, the workload increases with the number of processors while execution time remains approximately constant.

The second approach demonstrates the advantages of weak scaling for large scientific applications.

## 6. Techniques Used to Improve Parallel Performance

The researchers focused on reducing parallel execution overhead.

Important techniques include:

- Reducing communication overhead.
- Minimizing load imbalance.
- Reducing message startup time.
- Limiting sequential operation dependencies.
- Balancing communication and computation.

These techniques help maintain efficient parallel execution as the number of processors and problem size increase.

## 7. Why the Experiment Challenged Amdahl's Pessimism

The experimental findings showed that Amdahl's fixed-size assumptions did not fully describe applications designed for scalable parallel computing.

The apparent contradiction comes from comparing different workload models.

Amdahl's Law evaluates the speedup of a fixed problem, while Gustafson's Law evaluates the amount of additional work that can be completed in approximately the same execution time.

The experiment demonstrated that large-scale parallelism can be highly effective when applications are designed to scale.

However, it did not invalidate Amdahl's Law. Both models describe different performance scenarios.

## 8. Conclusion

The 1988 research demonstrated the practical potential of massively parallel computing.

The experiments on a 1024-processor system showed that scaled speedup could exceed 1000 times.

These results established the importance of workload scaling and helped popularize Gustafson's approach to parallel performance evaluation.

## 9. References

1. Gustafson, J. L. (1988). Reevaluating Amdahl's Law. Communications of the ACM, 31(5), 532-533.

2. Gustafson, J. L., Montry, G. R., and Benner, R. E. (1988). Development of Parallel Methods for a 1024-Processor Hypercube. SIAM Journal on Scientific and Statistical Computing.

3. Original research paper: https://users.cs.utah.edu/~hari/teaching/paralg/Gustafson88.pdf

4. SIAM publication: https://doi.org/10.1137/0909041