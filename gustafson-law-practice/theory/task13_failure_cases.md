# Task 13: Boundary Case, When Gustafson Fails

## 1. Introduction

Gustafson's Law assumes that a parallel application can increase its workload as more processors become available while maintaining approximately constant execution time.

The scaled speedup is:

$$
S(P)=P-\alpha(P-1)
$$

where:

- \(P\) is the number of processors.
- \(\alpha\) is the serial fraction measured during parallel execution.
- \(1-\alpha\) is the parallel fraction.

This model is effective when the workload can be distributed efficiently and communication overhead remains limited.

However, not all algorithms satisfy these assumptions. Some algorithms experience increasing synchronization costs, irregular workloads, communication bottlenecks, or changes in computational complexity as the problem grows.

This analysis examines two algorithms where the constant-runtime assumption becomes difficult to maintain.

## 2. Failure Case 1: Breadth-First Search (BFS) on Irregular Graphs

### 2.1 Algorithm Overview

Breadth-First Search explores a graph level by level, starting from a selected source vertex.

At each iteration, the algorithm processes the current frontier and discovers neighboring vertices that have not yet been visited.

A simplified BFS procedure is:

1. Initialize the source vertex.
2. Process all vertices in the current frontier.
3. Examine their outgoing edges.
4. Add newly discovered vertices to the next frontier.
5. Synchronize and continue until the frontier becomes empty.

For a graph with \(V\) vertices and \(E\) edges, a standard sequential BFS has complexity:

$$
T_{\text{BFS}}=O(V+E)
$$

### 2.2 Why Weak Scaling Fails

BFS is difficult to scale efficiently on irregular graphs, especially graphs with highly uneven vertex degrees.

When the graph becomes larger, the number of edges connected to individual vertices can vary significantly.

This creates several problems.

**1. Load imbalance**

Some processors may receive vertices with many outgoing edges, while others receive vertices with very few edges.

The processor handling the largest amount of work determines when the next synchronization step can begin.

Consequently, other processors may remain idle while waiting.

**2. Synchronization overhead**

BFS requires coordination between frontier-processing iterations.

Processors must exchange information about newly discovered vertices and ensure that vertices are not processed multiple times.

As the graph expands, synchronization and coordination can become increasingly expensive.

**3. Irregular memory access**

Graph traversal frequently accesses memory locations that are not adjacent.

This reduces cache efficiency and can create additional memory latency.

**4. Communication overhead**

When neighboring vertices are stored on different processors, discovering new vertices requires communication between nodes.

Larger distributed graphs can increase the amount of data exchanged between processors.

### 2.3 Impact on Gustafson's Model

Under ideal weak scaling, each processor receives approximately the same amount of work.

For BFS, this assumption can fail because graph connectivity is not necessarily distributed evenly.

The actual execution time can be represented conceptually as:

$$
T(P)=T_{\text{compute}}(P)+T_{\text{communication}}(P)+T_{\text{sync}}(P)
$$

where:

- \(T_{\text{compute}}\) is useful graph-processing time.
- \(T_{\text{communication}}\) is the time spent exchanging graph data.
- \(T_{\text{sync}}\) is synchronization and coordination time.

Even if computational work per processor remains approximately constant, communication and synchronization can increase.

Therefore:

$$
T(P)>T(1)
$$

may occur when the additional overhead is sufficiently large.

The constant parallel execution time assumed by ideal weak scaling is no longer maintained.

### 2.4 Conclusion for BFS

BFS demonstrates that increasing the graph size and processor count proportionally does not guarantee constant execution time.

Irregular graph structure, uneven vertex degrees, memory access patterns, and synchronization requirements can prevent efficient weak scaling.

Gustafson's Law therefore provides an optimistic prediction when these costs are significant.

## 3. Failure Case 2: Floyd-Warshall All-Pairs Shortest Path Algorithm

### 3.1 Algorithm Overview

The Floyd-Warshall algorithm calculates the shortest paths between every pair of vertices in a weighted graph.

It uses a distance matrix and repeatedly considers whether an intermediate vertex provides a shorter path between two vertices.

The main update is:

$$
D[i][j]=\min(D[i][j],D[i][k]+D[k][j])
$$

where:

- \(D[i][j]\) is the current shortest known distance from vertex \(i\) to vertex \(j\).
- \(k\) is the intermediate vertex being considered.

For a graph containing \(N\) vertices, the computational complexity is:

$$
T_{\text{FW}}=O(N^3)
$$

The distance matrix requires:

$$
M_{\text{FW}}=O(N^2)
$$

memory.

### 3.2 Why Weak Scaling Fails

Floyd-Warshall has a highly structured computational workload, but its iterations contain dependencies.

The algorithm processes intermediate vertices sequentially. Parallel implementations can distribute matrix updates across processors, but they must coordinate access to the required rows and columns.

Several problems appear as the matrix grows.

**1. Global synchronization**

Each iteration over intermediate vertex \(k\) depends on the distance information associated with that vertex.

Processors must ensure that the required updates are available before proceeding to the next iteration.

This creates repeated synchronization points.

**2. Communication growth**

In a distributed implementation, matrix partitions are stored across different nodes.

Processors may need to exchange rows, columns, or blocks of the distance matrix.

As the matrix becomes larger, the amount of information transferred between nodes can increase.

**3. Memory pressure**

The matrix requires \(O(N^2)\) storage.

Increasing the number of vertices increases memory requirements quadratically.

If available memory or memory bandwidth becomes insufficient, the application may experience slower execution or require additional data movement.

**4. Increasing computational work**

The algorithm performs \(O(N^3)\) operations.

Although parallel processors can distribute these operations, the total amount of computation increases rapidly as the number of vertices grows.

### 3.3 Impact on Gustafson's Model

Suppose the matrix dimension increases according to:

$$
N(P)=N_0P^q
$$

where:

- \(N_0\) is the initial matrix dimension.
- \(q\) describes how quickly the matrix grows with processor count.

The total computational work becomes:

$$
W(P)=O(N(P)^3)
$$

Substituting:

$$
W(P)=O(N_0^3P^{3q})
$$

The memory requirement becomes:

$$
M(P)=O(N(P)^2)
$$

Therefore:

$$
M(P)=O(N_0^2P^{2q})
$$

This demonstrates that increasing the matrix dimension can cause both computational and memory requirements to grow rapidly.

Even when computational work is distributed across processors, communication and synchronization may prevent execution time from remaining constant.

A more realistic execution-time model is:

$$
T(P)=T_{\text{compute}}(P)+T_{\text{communication}}(P)+T_{\text{sync}}(P)
$$

The additional terms can become significant as the matrix expands.

### 3.4 Conclusion for Floyd-Warshall

Floyd-Warshall demonstrates that regular computational work does not automatically guarantee ideal weak scaling.

Its repeated dependencies, distributed matrix communication, synchronization requirements, and quadratic memory growth can limit performance.

As a result, increasing the problem size with processor count may increase execution time instead of maintaining a constant runtime.

## 4. Comparative Analysis

| Characteristic | BFS | Floyd-Warshall |
|---|---|---|
| Main workload | Graph traversal | All-pairs shortest paths |
| Sequential complexity | \(O(V+E)\) | \(O(N^3)\) |
| Memory complexity | \(O(V+E)\) | \(O(N^2)\) |
| Main scaling problem | Irregular workload distribution | Repeated synchronization and matrix communication |
| Memory behavior | Irregular graph access | Large distance matrix |
| Communication challenge | Frontier and neighbor exchange | Row, column, or block exchange |
| Weak-scaling limitation | Load imbalance and synchronization | Communication, dependencies, and memory growth |

Both algorithms illustrate different reasons why ideal weak scaling can fail.

BFS is limited primarily by irregular graph structure and uneven work distribution.

Floyd-Warshall is limited by dependencies between iterations, communication requirements, and increasing memory consumption.

## 5. General Conditions Where Gustafson's Assumption Breaks Down

The two examples reveal several general conditions that make ideal weak scaling difficult:

1. **Increasing communication overhead:** More processors require additional data exchange.

2. **Synchronization contention:** Processors spend increasing amounts of time waiting for shared results or coordination.

3. **Load imbalance:** The workload is not distributed evenly among processors.

4. **Memory limitations:** Larger workloads exceed available memory or increase memory access costs.

5. **Algorithmic dependencies:** Some operations cannot proceed until earlier operations have completed.

6. **Changing execution characteristics:** Increasing problem size may introduce new bottlenecks that were insignificant in smaller configurations.

When these effects become substantial, the serial fraction and effective parallel execution time may no longer remain constant.

## 6. Conclusion

Gustafson's Law provides a useful theoretical model for applications that can expand their workload while maintaining approximately constant parallel execution time.

However, BFS and Floyd-Warshall demonstrate that this assumption is not universally valid.

BFS experiences irregular workload distribution, communication overhead, and synchronization difficulties. Floyd-Warshall experiences repeated dependencies, matrix communication, and rapidly increasing computational and memory requirements.

In both cases, additional processors do not guarantee proportional performance improvements.

The practical lesson is that weak scaling depends not only on processor count and workload size but also on algorithmic structure, communication patterns, memory behavior, and synchronization costs.

Gustafson's Law should therefore be treated as an ideal scaling model rather than a guarantee of real-world performance.