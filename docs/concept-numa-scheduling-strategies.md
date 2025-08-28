# NUMA resource scheduling strategies

When scheduling high-performance workloads, the secondary scheduler can employ different strategies to determine which NUMA node within a chosen worker node will handle the workload.
The supported strategies in OpenShift Container Platform include `LeastAllocated`, `MostAllocated`, and `BalancedAllocation`.
Understanding these strategies helps optimize workload placement for performance and resource utilization.

When a high-performance workload is scheduled in a NUMA-aware cluster, the following steps occur: 

1. The scheduler first selects a suitable worker node based on cluster-wide criteria.
For example taints, labels, or resource availability.
2. After a worker node is selected, the scheduler evaluates its NUMA nodes and applies a scoring strategy to decide which NUMA node will handle the workload.
3. After a workload is scheduled, the selected NUMA node’s resources are updated to reflect the allocation.

!!! note
    The default strategy applied is the `LeastAllocated` strategy. 
    This assigns workloads to the NUMA node with the most available resources that is the least utilized NUMA node.
    The goal of this strategy is to spread workloads across NUMA nodes to reduce contention and avoid hotspots.

The following table summarizes the different strategies and their outcomes:

**Scoring strategy summary**

| Strategy | Description | Outcome |
| :------------ | :----------- | :----------- |
| `LeastAllocated` | Favors NUMA nodes with the most available resources. | Spreads workloads to reduce contention and ensure headroom for high-priority tasks. |
| `MostAllocated` | Favors NUMA nodes with the least available resources. | Consolidates workloads on fewer NUMA nodes, freeing others for energy efficiency. |
| `BalancedAllocation` | Favors NUMA nodes with balanced CPU and memory usage. | Ensures even resource utilization, preventing skewed usage patterns. |
