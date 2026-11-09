# Task 17: 3D Stencil Hydrodynamics Simulation

## 1. Introduction

A 7-point three-dimensional stencil is commonly used in finite-difference simulations, including hydrodynamics and numerical solutions of partial differential equations.

Each grid point is updated using its own value and the values of its six direct neighbors:

- Left and right.
- Front and back.
- Top and bottom.

For a cubic computational domain with dimensions \(N \times N \times N\), the computational workload is proportional to the volume of the grid, while boundary communication is proportional to its surface area.

The objective is to demonstrate how the communication-to-computation ratio changes when the domain is scaled proportionally with the number of processors.

## 2. Computational Complexity

The total number of grid points in a cubic domain is:

$$
V=N^3
$$

Assuming a constant amount of computation per grid point, the total computational workload is:

$$
W(N)=c_wN^3
$$

where \(c_w\) is the computational cost per grid point.

Therefore:

$$
\boxed{W(N)=O(N^3)}
$$

The computational workload grows proportionally to the volume of the domain.

## 3. Communication Complexity

In a 7-point stencil, each subdomain must exchange boundary information with neighboring subdomains.

For a cubic domain, the surface area is:

$$
A=6N^2
$$

Assuming a constant communication cost per boundary element:

$$
C(N)=c_c6N^2
$$

where \(c_c\) is the communication cost per boundary element.

Therefore:

$$
\boxed{C(N)=O(N^2)}
$$

Communication grows proportionally to the surface area, while computation grows proportionally to the volume.

## 4. Communication-to-Computation Ratio

The communication-to-computation ratio is:

$$
R(N)=\frac{C(N)}{W(N)}
$$

Substituting the complexity expressions:

$$
R(N)=\frac{6c_cN^2}{c_wN^3}
$$

Simplifying:

$$
\boxed{
R(N)=\frac{6c_c}{c_wN}
}
$$

Therefore:

$$
R(N)=O\left(\frac{1}{N}\right)
$$

As the linear dimension of the domain increases, the communication cost becomes smaller relative to the computational workload.

This is the surface-to-volume effect.

## 5. Weak Scaling with Processor Count

Under ideal weak scaling, the total computational workload increases proportionally with processor count:

$$
W(P)\propto P
$$

Since stencil computation has cubic complexity:

$$
N^3\propto P
$$

Taking the cube root:

$$
\boxed{
N(P)\propto P^{1/3}
}
$$

Substituting this relationship into the communication-to-computation ratio:

$$
R(P)\propto\frac{1}{P^{1/3}}
$$

Therefore:

$$
\boxed{
R(P)=O(P^{-1/3})
}
$$

Under this idealized global-domain model, the communication-to-computation ratio decreases as processor count increases.

## 6. Numerical Scaling Example

Consider a baseline cubic domain with:

$$
N_0=100
$$

Assume the number of processors increases while the total workload grows proportionally.

The required domain dimension is:

$$
N(P)=100P^{1/3}
$$

The normalized communication-to-computation ratio is proportional to:

$$
R(P)=\frac{1}{N(P)}
$$

| Processor Count | Grid Dimension | Relative Workload | Relative Communication Ratio |
|---:|---:|---:|---:|
| 1 | 100 | 1 | 1.000 |
| 8 | 200 | 8 | 0.500 |
| 27 | 300 | 27 | 0.333 |
| 64 | 400 | 64 | 0.250 |
| 125 | 500 | 125 | 0.200 |
| 512 | 800 | 512 | 0.125 |
| 1000 | 1000 | 1000 | 0.100 |

The table demonstrates that the global surface-to-volume ratio decreases as the domain becomes larger.

## 7. Important Distinction: Global Domain vs. Local Subdomains

The previous derivation describes the surface-to-volume ratio of the entire cubic domain.

In a distributed-memory implementation, however, the domain is divided among processors.

Under ideal three-dimensional weak scaling, each processor receives approximately the same local cubic subdomain:

$$
n^3=\text{constant}
$$

where \(n\) is the local dimension.

Each processor communicates approximately proportional to its local boundary area:

$$
C_{\text{local}}=O(n^2)
$$

Its computational workload is:

$$
W_{\text{local}}=O(n^3)
$$

Therefore, the local communication-to-computation ratio is:

$$
R_{\text{local}}=O\left(\frac{1}{n}\right)
$$

Since local dimension \(n\) remains constant under ideal weak scaling:

$$
\boxed{
R_{\text{local}}=O(1)
}
$$

This distinction is important.

The global surface-to-volume ratio decreases because the total domain becomes larger. However, if each processor receives a fixed-size local subdomain, its local communication-to-computation ratio remains approximately constant.

## 8. Does Communication Improve or Degrade?

The answer depends on which ratio is being evaluated.

**Global surface-to-volume ratio:**

$$
R_{\text{global}}=O(P^{-1/3})
$$

This improves as the global domain expands.

**Local communication-to-computation ratio:**

$$
R_{\text{local}}=O(1)
$$

This remains approximately constant when local subdomain size is fixed.

Therefore, ideal weak scaling does not necessarily reduce the communication burden experienced by each individual processor.

It preserves approximately constant local computational and communication requirements.

Real systems may experience additional overhead from network latency, synchronization, message aggregation, and hardware topology.

## 9. Practical Limitations

The mathematical model assumes:

- A regular cubic computational domain.
- A constant number of operations per grid point.
- A constant communication cost per boundary element.
- Balanced domain decomposition.
- Fixed local workload under weak scaling.
- Negligible network contention and synchronization overhead.

Actual performance may differ because of:

1. Network latency.
2. Limited communication bandwidth.
3. Uneven domain decomposition.
4. Memory bandwidth limitations.
5. Synchronization between neighboring subdomains.
6. Hardware topology and placement of processes.

These factors can prevent real execution time from remaining constant.

## 10. Conclusion

A 7-point three-dimensional stencil has computational complexity:

$$
W(N)=O(N^3)
$$

and boundary communication complexity:

$$
C(N)=O(N^2)
$$

The global communication-to-computation ratio is:

$$
\boxed{
R_{\text{global}}=O\left(\frac{1}{N}\right)
}
$$

Under weak scaling, the global domain dimension grows as:

$$
N(P)=O(P^{1/3})
$$

Therefore:

$$
\boxed{
R_{\text{global}}=O(P^{-1/3})
}
$$

However, the local subdomain size remains constant under ideal three-dimensional weak scaling. Consequently, each processor's local communication-to-computation ratio remains approximately constant.

The main conclusion is that the global surface-to-volume ratio improves as the domain expands, but local communication requirements do not automatically decrease. Maintaining constant execution time still depends on network performance, synchronization, and balanced domain decomposition.