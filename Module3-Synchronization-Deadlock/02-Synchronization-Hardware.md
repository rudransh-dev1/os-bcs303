# Q. Explain synchronization hardware (TestAndSet, compare_and_swap and atomic instructions).

**Asked in:** June/July 2025 Makeup Q5a (with Peterson's solution)

## Definition

**Synchronization hardware** means special CPU instructions that read and modify a memory word **atomically** (as one uninterruptible step), so that two processes cannot interleave inside the instruction. They are used to build locks for the critical-section problem.

## Points

1. Software solutions (Peterson's) are slow and work for only two processes.
2. Atomic hardware instructions work for **n** processes and are fast.
3. On a uniprocessor, simply **disabling interrupts** during the critical section works (no process can preempt), but it is dangerous on multiprocessors, where interrupts must be disabled on every CPU, and it reduces system responsiveness.
4. Two common atomic instructions are **TestAndSet()** and **compare_and_swap()**.
5. Both return the **old value** of the lock and set the new value in one step.
6. They are used with a boolean `lock` (mutex) or an integer `lock` variable.

## 1. TestAndSet instruction

```c
boolean test_and_set(boolean *target) {
    boolean rv = *target;     // read the old value
    *target = true;           // set lock
    return rv;                // return old value (all atomic)
}
```

Usage:

```c
do {
    while (test_and_set(&lock))    // spin until it returns false
        ;                          // busy wait
    /* CRITICAL SECTION */
    lock = false;                  // release
    /* REMAINDER SECTION */
} while (true);
```

**Working:** the first process finds `lock == false`, sets it to `true` and returns `false`, so it enters. Any other process gets `true` back and keeps spinning. Mutual exclusion is guaranteed because the test and the set happen as one atomic step.

**Limitation:** this simple version does **not** guarantee bounded waiting (a process may starve). The bounded-waiting version uses a `waiting[]` array and hands the lock over in a fixed order.

## 2. compare_and_swap (CAS) instruction

```c
int compare_and_swap(int *value, int expected, int new_value) {
    int temp = *value;
    if (*value == expected)
        *value = new_value;      // swap only if unchanged
    return temp;                 // old value (atomic)
}
```

Usage:

```c
do {
    while (compare_and_swap(&lock, 0, 1) != 0)   // spin while lock is 1
        ;
    /* CRITICAL SECTION */
    lock = 0;                                    // release
    /* REMAINDER SECTION */
} while (true);
```

**Working:** the lock is acquired only if it was `0` when the instruction ran. Because the comparison and assignment are atomic, two processes cannot both see `0`.

## 3. Atomic variables

Java and C provide `AtomicInteger`, `AtomicBoolean` with methods like `getAndSet()`, `compareAndSet()`, `incrementAndGet()` which use CAS internally. Atomic variables are lock-free and useful for counters, but they are **not** a replacement for a lock when several shared variables must be updated together.

## Comparison table

| Basis | TestAndSet | compare_and_swap |
|---|---|---|
| Operation | Sets lock to `true` unconditionally, returns old value | Sets to `new_value` **only if** the current value equals `expected` |
| Returns | Old value | Old value |
| Use | Simple spin lock | Spin lock plus lock-free data structures, reference counting |
| Bounded waiting | Needs an extra `waiting[]` array | Needs a separate mechanism |

## Diagram: spin lock with TestAndSet

```
        Process P1                    Process P2
   test_and_set(lock)           test_and_set(lock)
        |  returns false              |  returns true (keeps spinning)
        v                              v
   [ CRITICAL SECTION ]            (busy wait loop)
        |
   lock = false  -----------------> now returns false, P2 enters
```

## Example

With `lock = false` initially, three processes try to enter at the same time. Only one instruction execution can set `lock` from `false` to `true`; the other two receive `true` and spin. When the winner sets `lock = false` after its critical section, one of the waiters wins the next race.

## Real-time example

A single key kept at a counter that can only be picked up by one hand at a time (atomic grab). TestAndSet is "grab the key and see whether it was already taken"; CAS is "put my hand on the key only if the counter says it is free".

## Conclusion

Atomic hardware instructions provide the low-level primitive on which locks, semaphores and monitors are built. TestAndSet and compare_and_swap guarantee mutual exclusion with very little code, but they use busy waiting, so the OS normally wraps them in higher-level tools (semaphores, mutexes) that block the process instead of spinning.

---
