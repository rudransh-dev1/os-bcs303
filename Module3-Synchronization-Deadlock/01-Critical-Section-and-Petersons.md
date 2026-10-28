# Q. What is the critical-section problem? Explain the three requirements of its solution and Peterson's solution.

**Asked in:** Dec 2023/Jan 2024 Q5a · Model Paper Q5b · Dec 2024/Jan 2025 Q5a · June/July 2025 Makeup Q5a · Dec 2025/Jan 2026 Q5a · June/July 2024 Supplementary Q5a

## Definition

A **critical section** is the part of a program in which a process accesses and updates shared data (shared variables, common tables, files). The **critical-section problem** is to design a protocol such that, when one process is executing in its critical section, no other process is allowed to execute in its critical section.

A **race condition** is the situation where two or more processes access and modify shared data at the same time and the final result depends on the order of execution.

## Points

1. Cooperating processes share data; concurrent access can leave the data inconsistent.
2. The general structure of any process `Pi` is: **entry section → critical section → exit section → remainder section**.
3. The **entry section** asks permission to enter; the **exit section** releases it.
4. Solutions may be **software** (Peterson's, Dekker's, Lamport's bakery) or **hardware** (TestAndSet, compare_and_swap, interrupts disabled).

## Three requirements of a valid solution

| Requirement | Meaning |
|---|---|
| **Mutual exclusion** | If process Pi is in its critical section, no other process can be in its critical section. |
| **Progress** | If no process is in its critical section and some processes want to enter, only those processes that are not in their remainder section can participate in the decision, and the decision cannot be postponed indefinitely. |
| **Bounded waiting** | There is a limit on the number of times other processes can enter their critical sections before a waiting process is allowed in (no starvation). |

### Diagram: general structure of a process

```
do {
      //  ENTRY SECTION   -> request permission
      //  CRITICAL SECTION -> access shared data
      //  EXIT SECTION    -> release permission
      //  REMAINDER SECTION
} while (true);
```

### Diagram: race condition on a shared counter

```
counter = 5        Producer: counter++        Consumer: counter--
                     T0: R1 = counter (5)       T0: R2 = counter (5)
                     T1: R1 = R1 + 1 (6)        T1: R2 = R2 - 1 (4)
                     T2: counter = R1 (6)       T2: counter = R2 (4)
   Final value = 4, but correct answer is 5  ->  RACE CONDITION
```

## Peterson's solution (2 processes)

**Assumption:** only two processes `P0` and `P1` (or `Pi` and `Pj`). Two shared variables:

- `int turn;` → whose turn it is to enter.
- `boolean flag[2];` → `flag[i] = true` means process `i` wants to enter.

### Code

```c
// Process Pi  (the other process is Pj)
do {
    flag[i] = true;          // I want to enter
    turn = j;                // but give the turn to the other process
    while (flag[j] && turn == j)   // busy wait while j wants it and it is j's turn
        ;                    // do nothing (spin)
    /* CRITICAL SECTION */
    flag[i] = false;         // I am done
    /* REMAINDER SECTION */
} while (true);
```

### Points

1. If both set `flag` to true, `turn` decides who enters — only one wins.
2. If only one process is interested, the other's flag is false, so it enters immediately (**progress** satisfied).
3. Because each process sets `flag[i] = false` right after its critical section, the other process can enter soon (**bounded waiting** satisfied).
4. No special hardware is needed; it is a pure **software** solution.
5. It uses **busy waiting** (spin lock), which wastes CPU cycles and is unsuitable on a single-CPU system (the waiting process may keep the CPU).
6. It does not work for more than two processes (Lamport's bakery algorithm extends it).

### Working example

| Step | P0 | P1 | Who enters |
|---|---|---|---|
| 1 | flag[0]=true, turn=1 | — | P0 checks flag[1]=false → **P0 enters** |
| 2 | flag[1]=true, turn=0 | (P1 wants in) | P1 checks flag[0]=true but turn=0 → **P1 waits** |
| 3 | finishes, flag[0]=false | | P1 now sees flag[0]=false → **P1 enters** |

## Example (student-style writing for the exam)

Two processes add and subtract from a shared bank balance. Peterson's solution forces one of them into its critical section at a time, so the balance is always either correctly increased or correctly decreased, never a mixed value.

## Real-time example

A single-stall washroom with one key. `flag[i] = true` means "I need the washroom", `turn = j` means "I am letting you go first if we both arrive together". Only one person can hold the key (mutual exclusion), a waiting person is not delayed forever when the washroom is free (progress), and nobody is made to wait behind unlimited newcomers (bounded waiting).

## Conclusion

The critical-section problem is about protecting shared data from race conditions, and any solution must satisfy mutual exclusion, progress and bounded waiting. Peterson's solution satisfies all three for two processes using only `flag` and `turn`, but it suffers from busy waiting, which is why semaphores and hardware instructions are preferred in real systems.

---
