# Q. What are the methods used for handling deadlocks? Explain deadlock prevention, avoidance, detection and recovery.

**Asked in:** June/July 2025 Q6a · Model Paper Q6a (recovery methods) · Dec 2023/Jan 2024 Q6a

## Definition

**Deadlock handling** means the set of techniques the OS uses so that deadlocks either never occur or are removed when they do occur. There are four approaches: **prevention, avoidance, detection with recovery, and (ignoring the problem)**.

## Points

1. **Prevention** removes one of the four necessary conditions *before* the deadlock can form.
2. **Avoidance** uses advance information (maximum needs) to keep the system always in a **safe state** (Banker's algorithm).
3. **Detection** allows deadlock to happen, then finds it (wait-for graph / resource allocation graph reduction).
4. **Recovery** breaks a detected deadlock by killing processes or preempting resources.
5. Most general-purpose OSs (Windows, Linux) use the **"do nothing / ostrich"** approach because deadlocks are rare and handling them costs more than it saves.

## 1. Deadlock prevention (break one condition)

| Condition to break | Technique | Drawback |
|---|---|---|
| Mutual exclusion | Make resources shareable (spooling for printers, read-only files) | Cannot be applied to truly non-shareable resources (a printer's paper path) |
| Hold and wait | Require all resources to be requested **at once** before execution, or release everything before asking for more | Low resource utilisation, starvation of processes needing many resources |
| No preemption | If a process requests a busy resource, **preempt** it: take away its held resources and make it wait | Feasible only for resources whose state can be saved (CPU registers, memory pages), not for printers |
| Circular wait | Impose a **total ordering** of resource types (R0 < R1 < ...); a process may request only in increasing order | Ordering must be chosen well; difficult with dynamic resources |

## 2. Deadlock avoidance

1. The OS needs the **maximum claim** of each process in advance.
2. A state is **safe** if there is at least one sequence in which all processes can finish.
3. **Banker's algorithm** checks each request: grant only if the resulting state is still safe.
4. **Resource-allocation-graph algorithm** (single instance of each resource): a request is granted only if it does not create a cycle in the graph.
5. Overhead: every request needs a safety check; processes must declare their maximum needs.

## 3. Deadlock detection

1. **Single instance per resource:** keep a **wait-for graph** with one node per process and an edge `Pi → Pj` if Pi waits for a resource held by Pj. Deadlock exists **if and only if** the graph has a cycle. Cycle detection is O(n²).
2. **Multiple instances per resource:** use a matrix algorithm similar to Banker's but with `Available` and `Request` matrices; it repeatedly finishes processes whose requests can be met. If some processes remain unfinished → deadlock, and those processes are the deadlocked set.
3. **When to detect:** either on **every request** (high overhead) or **periodically** when CPU utilisation drops (cheap, but the identity of the deadlocked processes is uncertain).

## 4. Recovery from deadlock

**A. Process termination**

1. **Abort all** deadlocked processes — sure but very costly (all progress is lost).
2. **Abort one process at a time** and re-run detection until the cycle is gone — less damage, more overhead.
3. Choice criteria: which has used the **least CPU time**, produced the **least output**, has the **most time left**, holds the **fewest resources**, or has the **lowest priority**.

**B. Resource preemption**

1. **Select a victim**: choose a process whose resources can be taken (lowest cost, fewest used resources).
2. **Rollback**: return the victim to a safe checkpoint from which it can restart (total rollback = start again; partial = checkpoint).
3. **Starvation**: the same process may always be chosen as victim. Fix it by limiting the number of times a process can be a victim (**aging** in the victim-selection cost).

## Diagram: the four approaches

```
        DEADLOCK HANDLING
                |
  +-------------+-------------+---------------+
  |             |             |               |
PREVENTION   AVOIDANCE    DETECTION      IGNORE
(break 1 of   (safe state  (wait-for      (ostrich
 4 conds)      check,       graph,          approach)
               Banker's)    matrices)
                                |
                            RECOVERY
                        (kill process /
                         preempt resource)
```

## Example

A system has three processes each needing two tape drives out of four. With hold-and-wait prevented (all resources requested together), two processes get their drives and finish; the third waits but never deadlocks. With Banker's algorithm, the request of the third is granted only when the resulting state remains safe.

## Real-time example

**Traffic at a narrow bridge:** prevention = allow only one direction at a time (break circular wait); avoidance = the traffic police check whether letting the next car in keeps the bridge clearable (safe state); detection = sensors notice two cars stuck nose to nose; recovery = send a tow truck to pull one car back (preemption/rollback).

## Conclusion

Deadlocks can be prevented by breaking one of the four necessary conditions, avoided by granting requests only in safe states, detected later with wait-for graphs or matrix algorithms, and finally recovered from by terminating processes or preempting resources. Practical OSs mostly ignore deadlock and rely on the programmer to lock resources in a consistent order.

---
