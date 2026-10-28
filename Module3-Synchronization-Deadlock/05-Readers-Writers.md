# Q. Explain the Readers–Writers problem with semaphores and write its code.

**Asked in:** Dec 2023/Jan 2024 Q5b (12 marks) · Dec 2024/Jan 2025 Q5b · June/July 2025 Makeup Q5b (code)

## Definition

The **readers–writers problem** deals with a shared database (file or record) that may be read by many **reader** processes at the same time, but must be written by a **writer** process in **exclusive** access. Multiple readers should be allowed simultaneously; a writer must wait until no reader is active and no other writer is writing.

## Points

1. Reading does not modify data, so any number of readers may read concurrently.
2. A writer modifies data, so it needs **mutual exclusion** against all readers and writers.
3. **First readers–writers problem:** no reader is kept waiting because of a waiting writer (readers have priority). Problem: a writer can **starve** if readers keep arriving.
4. **Second readers–writers problem:** once a writer is waiting, no new reader may start. Problem: readers may **starve** if writers keep arriving.
5. The standard solution uses one binary semaphore `rw_mutex` (for writers and the first reader), one semaphore `mutex` (to protect `read_count`) and an integer `read_count`.
6. The first reader locks `rw_mutex` and the last reader unlocks it, so the writer sees the whole group of readers as one exclusive section.

## Semaphores used

| Variable | Initial value | Purpose |
|---|---|---|
`rw_mutex` (or `wrt`) | 1 | Allows a writer, or the first reader, exclusive access |
`mutex` | 1 | Protects the shared variable `read_count` |
`read_count` | 0 | Number of readers currently inside |

## Code (first readers–writers problem, readers priority)

```c
semaphore rw_mutex = 1;     // for writers, and for the first/last reader
semaphore mutex    = 1;     // protects read_count
int read_count = 0;

// ---------------- WRITER ----------------
do {
    wait(rw_mutex);              // exclusive access
        /* WRITING is performed */
        write_database();
    signal(rw_mutex);
} while (true);

// ---------------- READER ----------------
do {
    wait(mutex);                 // enter the read_count critical section
        read_count++;
        if (read_count == 1)     // first reader locks out the writers
            wait(rw_mutex);
    signal(mutex);
        /* READING is performed */
        read_data();
    wait(mutex);
        read_count--;
        if (read_count == 0)     // last reader releases the writers
            signal(rw_mutex);
    signal(mutex);
} while (true);
```

## Working (step by step)

1. Reader R1 arrives: `read_count` becomes 1, it calls `wait(rw_mutex)` and takes the lock. It reads.
2. Reader R2 arrives: `read_count` becomes 2, but since it is not 1, it does **not** touch `rw_mutex`, so it also reads — both read together (**concurrency achieved**).
3. Writer W arrives: it calls `wait(rw_mutex)` and **blocks**, because the first reader holds it.
4. The readers finish one by one; the **last** reader (`read_count` becomes 0) calls `signal(rw_mutex)`, which lets the writer in.
5. The writer writes with exclusive access, then `signal(rw_mutex)` and the next reader/writer continues.
6. `mutex` ensures two readers never update `read_count` at the same time.

## Diagram

```
              +----------------------------+
              |  Shared database / file    |
              +----------------------------+
                 ^          ^          ^
                 |          |          |
              R1 read   R2 read    R3 read      (all readers together: allowed)
                 
              W1 write  <-- allowed only when read_count == 0 and no other writer
                        <-- blocked while at least one reader is inside
```

## Example

Three readers R1, R2, R3 and one writer W. R1 enters (read_count = 1, holds `rw_mutex`). R2 and R3 enter and read concurrently (read_count = 3). W blocks. When R1, R2, R3 finish, the last one releases `rw_mutex`, and W writes. If W arrives first, it writes alone first, then all readers read together.

## Real-time example

A **Google Doc / bank passbook**:
- Many people can **view** the document (readers) at the same time.
- Only one person can **edit** it (writer) at a time.
- While the document is being edited, viewing is paused; while many people are viewing, the editor must wait.

Library book records: any number of students can read a catalogue card, but the librarian must have exclusive access to update it.

## Conclusion

The readers–writers problem requires many readers **or** one writer, never both. Using `rw_mutex` plus a `read_count` protected by `mutex` gives concurrent reading with exclusive writing. The **first** readers–writers problem favours readers (writer may starve), while the **second** favours writers (reader may starve); a fair solution must bound the waiting of both.

---
