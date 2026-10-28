# Q. What is the Dining Philosophers problem? Explain the structure and its solution using semaphores.

**Asked in:** Dec 2024/Jan 2025 Q5c · June/July 2025 Q5b · Model Paper Q6c · June/July 2025 Makeup Q5c · Dec 2025/Jan 2026 Q6a

## Definition

The **dining philosophers problem** is a classic synchronisation problem: five philosophers sit around a circular table with five chopsticks (one between each pair). A philosopher alternates between **thinking** and **eating**, and to eat he needs **both** the chopstick on his left and the chopstick on his right. Each chopstick can be held by only one philosopher at a time.

## Points

1. There are **5 philosophers** and **5 single chopsticks** (not 5 pairs).
2. A philosopher needs **both** adjacent chopsticks to eat.
3. The problem models the allocation of **multiple resources among processes** without deadlock or starvation.
4. **Naive solution:** each philosopher picks up his left chopstick, then his right. If all five pick up their left chopstick at the same time, each waits forever for the right one → **deadlock**.
5. Possible remedies:
   - allow at most **four** philosophers at the table at a time;
   - allow a philosopher to pick up chopsticks only if **both** are free (inside a critical section);
   - use an **asymmetric** order (odd philosophers pick left first, even pick right first);
   - use a monitor / lock ordering.

## Structure of a philosopher

```c
semaphore chopstick[5] = {1,1,1,1,1};   // one binary semaphore per chopstick

// philosopher i  (the chopsticks are i and (i+1) % 5)
do {
    wait(chopstick[i]);                 // pick up LEFT chopstick
    wait(chopstick[(i+1) % 5]);         // pick up RIGHT chopstick
        /* EAT */
        eat();
    signal(chopstick[i]);               // put down LEFT
    signal(chopstick[(i+1) % 5]);       // put down RIGHT
        /* THINK */
        think();
} while (true);
```

**Problem with the above code:** if all five philosophers execute `wait(chopstick[i])` at the same moment, each holds one chopstick and all wait for the right one → **deadlock**.

## Correct solution using semaphores (limit to 4 philosophers)

```c
semaphore chopstick[5] = {1,1,1,1,1};
semaphore room = 4;          // at most 4 philosophers may try to eat

// philosopher i
do {
    wait(room);                        // only 4 can compete at a time
        wait(chopstick[i]);            // left
        wait(chopstick[(i+1) % 5]);    // right
            eat();
        signal(chopstick[i]);
        signal(chopstick[(i+1) % 5]);
    signal(room);
        think();
} while (true);
```

**Why this works:** with at most four philosophers competing, at least one of them can always get both chopsticks, so the system never deadlocks. Since a philosopher puts the chopsticks down in finite time, every philosopher eventually eats (`progress`), and with fair semaphores the waiting is bounded.

## Alternative correct solution (both chopsticks or none)

```c
semaphore mutex = 1;                     // protects the "picking up" step
semaphore chopstick[5] = {1,1,1,1,1};

do {
    wait(mutex);                         // enter critical section
        wait(chopstick[i]);              // pick both while holding mutex
        wait(chopstick[(i+1) % 5]);
    signal(mutex);
        eat();
    signal(chopstick[i]);
    signal(chopstick[(i+1) % 5]);
        think();
} while (true);
```

Here the two `wait()` calls are inside a critical section, so no two neighbours can each take one chopstick and wait.

## Diagram

```
              P0
        C0          C1
   P4                  P1
        C4          C2
              P3  C3  P2

   P0 needs C0 + C1
   P1 needs C1 + C2
   P2 needs C2 + C3
   P3 needs C3 + C4
   P4 needs C4 + C0     -> circular wait = risk of deadlock
```

## Example

Suppose all five philosophers pick up their left chopsticks at the same instant: P0 holds C0, P1 holds C1, P2 holds C2, P3 holds C3, P4 holds C4. Now each needs the chopstick held by his neighbour — a **circular wait**, which is one of the four necessary conditions for deadlock, so the system hangs.

## Real-time example

Five programmers sharing **five shared desks with two chairs each** in a study room: each needs two chairs to work. If everyone grabs one chair at the same time, nobody can sit. Allowing only four people in the room at a time guarantees that someone always finds a second chair.

Another classic analogy: five cars at a narrow crossing where each needs the lane the other is holding.

## Conclusion

The dining philosophers problem models resource allocation among competing processes. The naive "pick up left then right" solution can deadlock, and it is solved by limiting the number of philosophers (at most four) or by acquiring both chopsticks inside a critical section. It is therefore the standard illustration of **deadlock avoidance** in synchronisation design.

---
