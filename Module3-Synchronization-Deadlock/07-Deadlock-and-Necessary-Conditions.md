# Q. What is deadlock? What are the necessary conditions for a deadlock to occur?

**Asked in:** Dec 2023/Jan 2024 Q6a · Model Paper Q5a, Q6a · Dec 2024/Jan 2025 Q6a · June/July 2025 Q5b, Q6a · June/July 2025 Makeup Q6a · June/July 2024 Supplementary Q6a

## Definition

A **deadlock** is a situation in which a set of processes is blocked permanently because each process is holding a resource and waiting for a resource that is held by another process in the same set. No process can proceed, so no process can release anything.

Formally: a set of processes `{P0, P1, ..., Pn}` is in deadlock if `P0` waits for a resource held by `P1`, `P1` waits for a resource held by `P2`, ..., and `Pn` waits for a resource held by `P0`.

## Points

1. Deadlock needs **mutual exclusion + hold and wait + no preemption + circular wait** (all four together).
2. Deadlock is different from **starvation**: in starvation a process waits indefinitely but the system as a whole is still progressing.
3. If any one of the four conditions is broken, deadlock cannot occur — this is the basis of deadlock **prevention**.
4. Deadlocked processes hold resources forever, so memory and devices are wasted and throughput falls to zero for those processes.
5. Typical real causes: two processes locking two files in opposite order, or two transactions updating two bank rows in opposite order.

## The four necessary conditions (Coffman conditions)

| # | Condition | Meaning | How to break it |
|---|---|---|---|
| 1 | **Mutual exclusion** | At least one resource is held in a non-shareable mode; only one process can use it at a time | Make resources shareable (e.g. read-only files, spooling) — not always possible |
| 2 | **Hold and wait** | A process holds at least one resource and waits for additional resources held by other processes | Require the process to request all resources at once before starting |
| 3 | **No preemption** | A resource can be released only voluntarily by the process holding it | Allow the OS to preempt (take back) resources from a waiting process |
| 4 | **Circular wait** | A closed chain of processes exists, each waiting for a resource held by the next | Impose a total ordering of resource types and request in increasing order |

## Diagram: circular wait

```
   P0 holds R0, wants R1
        |
        v
   P1 holds R1, wants R2
        |
        v
   P2 holds R2, wants R0
        |
        +---------> back to P0   =>  CYCLE  =>  DEADLOCK
```

Text form: `P0 -> R1 -> P1 -> R2 -> P2 -> R0 -> P0`

## Example

Two processes and two files:

1. `P1` opens File A (holds A) and requests File B.
2. `P2` opens File B (holds B) and requests File A.
3. `P1` waits for B, which `P2` holds; `P2` waits for A, which `P1` holds.
4. Both wait forever → deadlock. All four conditions are satisfied: A and B are exclusive, both hold one and wait for another, neither can be preempted, and there is a cycle.

## Real-time example

Two cars at a narrow one-lane bridge from opposite sides — neither can move forward or reverse (mutual exclusion = single lane, hold and wait = each is on its part of the bridge, no preemption = cannot be removed, circular wait = each needs the other's space). Traffic police (OS) resolve it by allowing one direction at a time.

Another: two people each holding one page of a form that the other needs, refusing to hand it over until they get theirs.

## Conclusion

Deadlock is a permanent blocking of a set of processes, each holding a resource and waiting for another. It occurs only when mutual exclusion, hold and wait, no preemption and circular wait all hold simultaneously; breaking any one of these four Coffman conditions is the basis of every deadlock-handling technique.

---
