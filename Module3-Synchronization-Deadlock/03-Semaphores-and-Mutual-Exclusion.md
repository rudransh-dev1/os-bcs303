# Q. Define semaphore. Explain how mutual exclusion can be implemented using semaphores.

**Asked in:** Dec 2023/Jan 2024 Q5b (readers-writers with semaphores) · Dec 2024/Jan 2025 Q5c · June/July 2025 Q5a · June/July 2024 Supplementary Q5b · Model Paper Q6c

## Definition

A **semaphore** is an integer synchronisation variable that is accessed only through two atomic operations, `wait(S)` (also called `P`) and `signal(S)` (also called `V`). It was proposed by Dijkstra.

```
wait(S):   while (S <= 0) ;    S = S - 1;      // acquire
signal(S): S = S + 1;                          // release
```

## Points

1. The value of a semaphore decides how many processes may enter at the same time.
2. `wait()` blocks (decrements) and `signal()` releases (increments). Both are **atomic** — no two processes can execute them simultaneously.
3. **Types of semaphores:**
   - **Binary semaphore (mutex)**: values only 0 and 1. Used for mutual exclusion. Also called a mutex lock.
   - **Counting semaphore**: values from 0 to N. Used to control access to a resource with N instances (e.g. 5 printers).
4. Semaphores can also be used for **ordering** (process A must run before B) and for **resource counting**.
5. **Busy waiting problem:** the simple version spins, wasting CPU. The real implementation keeps a **waiting queue** for each semaphore; `wait()` blocks the process and the kernel wakes it on `signal()` (blocking semaphore).
6. **Deadlock and starvation risk:** if processes acquire semaphores in different orders, they can deadlock; with an unfair wait queue a process may starve.
7. Semaphores can also be used **for synchronisation between threads/processes**.

## Implementing mutual exclusion with a semaphore

```c
semaphore mutex = 1;          // binary semaphore, initially free

// Process Pi
do {
    wait(mutex);              // entry section: if mutex=0, Pi blocks
        /* CRITICAL SECTION */
    signal(mutex);            // exit section: release so another may enter
        /* REMAINDER SECTION */
} while (true);
```

### Why it works (step by step)

1. `mutex = 1` initially, so the **first** process executes `wait()` and makes `mutex = 0`, then enters.
2. A second process executes `wait()`, finds `mutex = 0`, and is **blocked** (or spins) — so **mutual exclusion** holds.
3. When the first process finishes, it executes `signal()`, which sets `mutex = 1` and wakes one blocked process.
4. That woken process decrements `mutex` to 0 and enters.
5. Because `wait()` and `signal()` are atomic, two processes can never both see `mutex = 1`.

### Diagram

```
  mutex = 1
  --------              wait()             wait() finds 0
 | P1 in CS |  ------> mutex = 0   ------>  P2 blocked (queue)   -> mutual exclusion
  --------
        |
        | signal()  -> mutex = 1 -> one blocked process wakes up
        v
   P2 in CS
```

## Example: ordering problem with semaphores

Suppose `S1` (in P1) must execute before `S2` (in P2). Use a semaphore `sync = 0`:

```c
P1: S1; signal(sync);        P2: wait(sync); S2;
```

P2 blocks at `wait(sync)` until P1 finishes S1, giving the required order.

## Real-time example

A library with **3 study rooms** uses a counting semaphore `rooms = 3`. Each student calls `wait(rooms)` before entering and `signal(rooms)` after leaving; the fourth student waits until someone leaves. A **single** washroom with one key is a binary semaphore (`mutex = 1`).

## Conclusion

A semaphore is an integer variable handled only by atomic `wait()` and `signal()` operations. A binary semaphore initialised to 1 gives mutual exclusion in a simple, standard pattern around the critical section, counting semaphores control multiple identical resources, and using a real waiting queue removes the busy-waiting drawback of earlier software solutions.

---
