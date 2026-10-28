# Q. Explain the resource allocation graph (RAG) with an example. How is it used for deadlock detection and avoidance?

**Asked in:** Model Paper Q6b · Dec 2025/Jan 2026 Q6b · June/July 2025 Makeup Q6b

## Definition

A **resource allocation graph (RAG)** is a directed graph that shows the current allocation of resources to processes and the requests that are still pending. It has a set of vertices **V = P ∪ R** (processes and resource types) and a set of edges **E** (request and assignment edges).

## Points

1. **Circle** = process; **rectangle** = resource type; **dots inside the rectangle** = instances of that resource.
2. **Request edge `Pi → Rj`**: process `Pi` is waiting for an instance of `Rj`.
3. **Assignment edge `Rj → Pi`**: one instance of `Rj` has been allocated to `Pi`.
4. A request edge becomes an assignment edge when the request is granted (the edge is reversed).
5. **If the graph has no cycle → no deadlock.**
6. **If there is a cycle:** with **one instance** per resource type, a cycle means deadlock **for certain**; with **multiple instances**, a cycle means deadlock is **possible** (the cycle may be broken by a process releasing an instance).

## Legend

```
  ( )  = process            [ ]  = resource type with dots = instances
  Pi ---> Rj   request      Rj ---> Pi   allocation
```

## Example 1: cycle with one instance each (deadlock)

```
   P1 ---> R1        R1 ---> P2
   P2 ---> R2        R2 ---> P1
```

`P1` holds `R2`? Let us draw it properly:

```
    (P1) ---requests---> [R1]
      ^                     |
      |                  allocated
   allocated                  v
      |                    (P2)
    [R2] <---requests----- (P2)
```

`P1` holds `R2` and wants `R1`; `P2` holds `R1` and wants `R2`. This graph has a cycle `P1 → R1 → P2 → R2 → P1`, and since each resource has one instance, the system is **deadlocked**.

## Example 2: cycle with multiple instances (no deadlock)

```
   [R1 has 2 dots]   [R2 has 2 dots]
   P1 holds one R1, requests R2
   P2 holds one R2, requests R1
   P3 holds the other R2 (not part of the cycle)
```

There is a cycle, but `P3` will release its `R2`, which can then satisfy `P1`, so the system is **not deadlocked**. Hence with multiple instances a cycle is only a **necessary** condition, not a sufficient one.

## Example 3 (exam style, full drawing)

```
        (P1)                (P2)                (P3)
       /    \              /    \                 |
  req /      \ alloc  req /      \                | alloc
     v        v         v        v                v
   [R1]     [R3]      [R2]     [R1]             [R2]
   (1 dot)  (1 dot)   (1 dot)  (1 dot)          (1 dot)
```

Reading: `P1` holds `R3` and requests `R1`; `P2` holds `R1` and requests `R2`; `P3` holds `R2`. This creates `P1 → R1 → P2 → R2 → P3` — no cycle back to `P1`, so **no deadlock**; the chain will clear when `P3` finishes.

## Using the RAG for deadlock avoidance

1. If each resource type has **one instance**, use the **RAG algorithm**: a request is granted only if converting the request edge into an assignment edge **does not create a cycle**.
2. To test a request: pretend to grant it (reverse the edge) and run cycle detection. Cycle → do not grant (the process waits).
3. Cycles can be found in O(n²) time with a DFS or the resource-allocation-graph reduction algorithm.
4. For **multiple instances**, the graph alone is not enough; use the **Banker's algorithm** or the matrix-based detection algorithm instead.

## Real-time example

A **kitchen with one gas stove and one oven**. Cook A holds the stove and wants the oven; Cook B holds the oven and wants the stove. Draw the graph: A → Oven → B → Stove → A, a cycle with single instances, so the kitchen is deadlocked until someone intervenes. Adding a second stove would turn it into a multiple-instance graph, where the cycle alone would not guarantee a deadlock.

## Conclusion

The resource allocation graph is a compact visual model of process-resource relationships. No cycle always means no deadlock; a cycle with single instances proves deadlock, while with multiple instances it only indicates a possibility. It is therefore the primary tool for deadlock detection and, in the single-instance case, for deadlock avoidance.

---
