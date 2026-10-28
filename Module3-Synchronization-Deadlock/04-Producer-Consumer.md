# Q. Explain the producer–consumer (bounded buffer) problem and write its algorithm using semaphores.

**Asked in:** Dec 2025/Jan 2026 Q5a (critical section problem + producer-consumer algorithm)

## Definition

The **producer–consumer problem** (also called the bounded-buffer problem) is a classic synchronisation problem in which a **producer** process produces items and inserts them into a buffer of fixed size **N**, and a **consumer** process removes items from the same buffer and consumes them. The producer must not insert when the buffer is full, and the consumer must not remove when the buffer is empty.

## Points

1. The buffer has `N` slots; `in` is the index for the next item to insert and `out` for the next item to remove.
2. Two correctness conditions are needed: **no item is lost** and **no item is read twice**.
3. Three semaphores solve it: `mutex = 1` (mutual exclusion on the buffer), `full = 0` (number of filled slots), `empty = N` (number of free slots).
4. The producer does `wait(empty)` then `wait(mutex)`; the consumer does `wait(full)` then `wait(mutex)`.
5. **Order of waits matters:** the resource semaphore (`empty`/`full`) must be acquired **before** `mutex`. If reversed, the producer can hold `mutex` while waiting for an empty slot and the consumer cannot enter to make space → **deadlock**.
6. It is an example of a **cooperation synchronisation** problem (a race condition on the shared buffer).

## Algorithm using semaphores

```c
#define N 5               // buffer size
int  buffer[N];
int  in = 0, out = 0;

semaphore mutex = 1;      // protects the buffer indices
semaphore empty = N;      // number of empty slots
semaphore full  = 0;      // number of filled slots

// ---------------- PRODUCER ----------------
do {
    produce_item(next);            // create a data item
    wait(empty);                   // wait for at least one free slot
    wait(mutex);                   // enter critical section
        buffer[in] = next;         // insert item
        in = (in + 1) % N;         // circular buffer
    signal(mutex);                 // leave critical section
    signal(full);                  // one more item available
} while (true);

// ---------------- CONSUMER ----------------
do {
    wait(full);                    // wait for at least one item
    wait(mutex);                   // enter critical section
        next = buffer[out];        // remove item
        out = (out + 1) % N;
    signal(mutex);                 // leave critical section
    signal(empty);                 // one more free slot
    consume_item(next);
} while (true);
```

## Working (step by step)

1. **Initially** `empty = N`, `full = 0`, `mutex = 1`.
2. Producer arrives: `wait(empty)` makes it `N−1`, `wait(mutex)` makes it 0, inserts at `buffer[in]`, then `signal(mutex)` and `signal(full)` → `full = 1`.
3. Consumer arrives: `wait(full)` makes it 0 (if no item, the consumer blocks here), `wait(mutex)`, removes the item, `signal(mutex)`, `signal(empty)`.
4. When the buffer is full, `empty = 0`, so the producer blocks until the consumer frees a slot.
5. When the buffer is empty, `full = 0`, so the consumer blocks until the producer adds an item.

## Diagram

```
  PRODUCER                                CONSUMER
  produce_item()                          wait(full)   <-- blocks if empty
      |                                       |
  wait(empty)  <-- blocks if full              v
      |                                  wait(mutex)
      v                                       |
  wait(mutex)                             remove buffer[out]
      |                                       |
  insert buffer[in]                       signal(mutex)
      |                                       |
  signal(mutex)                           signal(empty) --> wakes producer
      |
  signal(full) --> wakes consumer        consume_item()

        +---------------------------------------+
        |  buffer: [ ][ ][ ][ ][ ]   N slots    |
        |  in -> next write    out -> next read |
        +---------------------------------------+
```

## Example

With `N = 3`: producer inserts A, B, C (`empty = 0`, `full = 3`). Now the producer blocks. The consumer removes A (`out` moves), `signal(empty)` wakes the producer, which inserts D at the freed slot. No item is lost and none is read twice.

## Real-time example

A **tea stall** with a tray that holds 5 cups. The tea-maker (producer) keeps filling cups; if the tray is full, he waits. The waiter (consumer) carries cups to tables; if the tray is empty, he waits. The tray is the shared buffer, `mutex` ensures only one person touches the tray at a time, `full` counts filled cups and `empty` counts free places.

Another example: a **YouTube upload queue**. The uploader adds videos, the encoder removes and processes them, and the queue length is fixed.

## Conclusion

The producer–consumer problem is solved with three semaphores: a binary `mutex` for mutual exclusion on the buffer and two counting semaphores (`full` and `empty`) for the number of items and free slots. Acquiring the counting semaphore before the mutex prevents deadlock, giving a correct, starvation-free and race-free solution.

---
