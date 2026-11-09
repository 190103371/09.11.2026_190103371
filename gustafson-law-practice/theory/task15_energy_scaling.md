# Task 15: Energy and Power Proportionality under Scaled Speedup

## 1. Introduction

Gustafson's Law explains how parallel systems can increase computational workload while maintaining approximately constant execution time.

However, increasing the number of active processors also affects power consumption and total energy usage.

A parallel system may complete a larger workload in the same amount of time, but it may consume more energy because additional processor nodes require electrical power.

This task develops an energy model for weak scaling and compares the energy consumption of one processor with a system containing \(P\) processors.

## 2. Power Consumption Model

Assume that each processor node consumes two types of power:

- \(P_{\text{idle}}\): static power consumed by the node.
- \(P_{\text{active}}\): additional dynamic power consumed during computation.

The total power consumption of one active node is:

$$
P_{\text{node}}=P_{\text{idle}}+P_{\text{active}}
$$

For a system containing \(P\) identical active nodes, total power consumption is:

$$
P_{\text{total}}(P)=P(P_{\text{idle}}+P_{\text{active}})
$$

This model assumes that every node remains active and consumes the same amount of power during the computation.

## 3. Energy Consumption on One Processor

Energy is the product of power and execution time:

$$
E=P_{\text{total}}\cdot T
$$

For one processor executing a workload over time \(T\):

$$
E_1=(P_{\text{idle}}+P_{\text{active}})T
$$

The total energy consists of two components:

$$
E_1=E_{\text{static}}+E_{\text{dynamic}}
$$

where:

$$
E_{\text{static}}=P_{\text{idle}}T
$$

and:

$$
E_{\text{dynamic}}=P_{\text{active}}T
$$

Therefore:

$$
\boxed{
E_1=(P_{\text{idle}}+P_{\text{active}})T
}
$$

## 4. Energy Consumption on P Processors

Under Gustafson's weak-scaling assumption, the workload increases with processor count while execution time remains approximately constant.

For \(P\) active processors, total power is:

$$
P_{\text{total}}(P)=P(P_{\text{idle}}+P_{\text{active}})
$$

If the execution time remains \(T\), total energy consumption is:

$$
E_P=P(P_{\text{idle}}+P_{\text{active}})T
$$

Therefore:

$$
\boxed{
E_P=P E_1
}
$$

The system consumes \(P\) times as much energy as the single-processor configuration under these assumptions.

## 5. Comparison of Static and Dynamic Energy

The total energy on \(P\) processors can be separated into static and dynamic components.

Static energy:

$$
E_{\text{static}}(P)=PP_{\text{idle}}T
$$

Dynamic energy:

$$
E_{\text{dynamic}}(P)=PP_{\text{active}}T
$$

Total energy:

$$
E_P=PP_{\text{idle}}T+PP_{\text{active}}T
$$

This shows that both static and dynamic energy increase linearly with processor count when all nodes remain active for the same execution time.

The static component is associated with keeping the hardware powered, while the dynamic component is associated with computational activity.

## 6. Numerical Example

Consider a hypothetical parallel system with the following power characteristics:

$$
P_{\text{idle}}=40\text{ W}
$$

$$
P_{\text{active}}=60\text{ W}
$$

Assume that execution time is:

$$
T=10\text{ seconds}
$$

The total power consumption of one active processor node is:

$$
P_{\text{node}}=40+60=100\text{ W}
$$

### One Processor

$$
E_1=100\cdot10=1000\text{ J}
$$

### Four Processors

$$
P_{\text{total}}(4)=4\cdot100=400\text{ W}
$$

$$
E_4=400\cdot10=4000\text{ J}
$$

### Eight Processors

$$
P_{\text{total}}(8)=8\cdot100=800\text{ W}
$$

$$
E_8=800\cdot10=8000\text{ J}
$$

### Comparison Table

| Active Nodes | Total Power | Execution Time | Total Energy |
|---:|---:|---:|---:|
| 1 | 100 W | 10 s | 1,000 J |
| 2 | 200 W | 10 s | 2,000 J |
| 4 | 400 W | 10 s | 4,000 J |
| 8 | 800 W | 10 s | 8,000 J |
| 16 | 1,600 W | 10 s | 16,000 J |

The example demonstrates that energy consumption increases linearly with the number of active nodes when execution time and per-node power remain constant.

## 7. Relationship Between Energy and Gustafson's Speedup

Gustafson's scaled speedup is:

$$
S_G(P)=P-\alpha(P-1)
$$

For a fixed parallel execution time, the workload increases while the number of active processors also increases.

Under the simplified power model, energy consumption is:

$$
E_P=P(P_{\text{idle}}+P_{\text{active}})T
$$

Energy per unit of useful scaled work can be compared using:

$$
\eta_E(P)=\frac{S_G(P)}{E_P}
$$

Substituting:

$$
\eta_E(P)=
\frac{P-\alpha(P-1)}
{P(P_{\text{idle}}+P_{\text{active}})T}
$$

Rearranging:

$$
\boxed{
\eta_E(P)=
\frac{1-\alpha+\frac{\alpha}{P}}
{(P_{\text{idle}}+P_{\text{active}})T}
}
$$

As \(P\) becomes large:

$$
\lim_{P\to\infty}\eta_E(P)=
\frac{1-\alpha}
{(P_{\text{idle}}+P_{\text{active}})T}
$$

This indicates that, under the assumed constant per-node power and constant execution time, energy efficiency per unit of scaled work approaches a constant value.

## 8. Energy Optimization Strategies

Although adding processors increases total power under this simplified model, several techniques can improve energy efficiency in real systems.

**1. Dynamic Voltage and Frequency Scaling (DVFS)**

Reducing processor voltage and frequency can lower dynamic power consumption when maximum performance is unnecessary.

**2. Power-aware scheduling**

Workloads can be assigned to processors in a way that reduces unnecessary activity and avoids keeping unused nodes fully powered.

**3. Dynamic power management**

Inactive processors or nodes can enter low-power states when they are not required.

**4. Communication optimization**

Reducing unnecessary communication can decrease network and processor energy consumption.

**5. Efficient load balancing**

Distributing computation efficiently can reduce idle waiting time and improve the amount of useful work completed per unit of energy.

These techniques can change the power characteristics assumed in the simplified model.

## 9. Assumptions and Limitations

The model makes several simplifying assumptions:

- All processor nodes are identical.
- Each node consumes the same static and dynamic power.
- All nodes remain active during execution.
- Execution time remains constant under weak scaling.
- Communication and network power consumption are not modeled separately.
- Cooling and power conversion losses are excluded.

In real systems, power consumption may vary with processor utilization, voltage, frequency, memory activity, network traffic, and hardware architecture.

Therefore, the numerical results represent an idealized comparison rather than a measurement of a real supercomputer.

## 10. Conclusion

Gustafson's Law allows a parallel system to increase computational workload while maintaining approximately constant execution time.

However, adding active processor nodes increases total power consumption.

Under the simplified model:

$$
E_1=(P_{\text{idle}}+P_{\text{active}})T
$$

and:

$$
E_P=P(P_{\text{idle}}+P_{\text{active}})T
$$

Therefore:

$$
\boxed{
E_P=P E_1
}
$$

The analysis demonstrates that a system can achieve higher scaled throughput while consuming proportionally more energy.

Consequently, parallel performance should not be evaluated using speedup alone. Energy consumption, power efficiency, and hardware utilization are also important factors when designing scalable computing systems.