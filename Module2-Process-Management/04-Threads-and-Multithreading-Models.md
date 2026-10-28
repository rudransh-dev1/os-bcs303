# Q. Define thread. Explain multithreading and the multithreading models with neat diagrams. Compare user-level and kernel-level threads.

**Asked in:** Dec 2023/Jan 2024 Q4a · Model Paper Q4a · Dec 2024/Jan 2025 Q4a · June/July 2025 Q4b,c · June/July 2025 Makeup Q3c, Q4a · Dec 2025/Jan 2026 Q4a · June/July 2024 Supplementary Q4a

## Definition

A **thread** is the smallest unit of execution inside a process: a basic unit of CPU utilization that consists of a thread ID, a program counter, a register set and a stack. It shares the code section, data section and open files with the other threads of the same process.

> A **multithreaded process** has more than one thread of control, so it can do several tasks at the same time within one process.

## Points

1. Threads of one process share **code, data, heap and open files**, but each has its own **PC, registers and stack**.
2. A single-threaded process has one PC, one stack and one flow of control: tasks run one after another.
3. A multithreaded process runs tasks concurrently; on a multiprocessor, threads can run **in parallel**.
4. Context switching between threads of the same process is **cheaper** than between processes (no memory-map change).
5. Threads are "lightweight processes" because they do not have a separate address space.

## Benefits of multithreading (four standard benefits)

1. **Responsiveness**: the program stays interactive even if one thread blocks (a UI thread keeps working while a download thread waits).
2. **Resource sharing**: threads share memory and files by default, so no IPC or shared-memory setup is needed.
3. **Economy**: creating a thread is much cheaper than creating a process, and switching between threads is faster (Solaris: thread creation about 30 times cheaper than a process).
4. **Scalability**: threads can run on several cores at once, so the program uses a multiprocessor efficiently.

Extra benefit: **parallelism** on a multicore machine and simpler program structure (one thread per task).

## Single-threaded vs multithreaded process

```
 SINGLE-THREADED PROCESS            MULTITHREADED PROCESS
 +-------------------------+         +------------------------------------+
 | code  | data |  files   |         |  code  |  data  |  files          |
 +-------------------------+         +------------------------------------+
 | PC | Registers | Stack  |         | T1: PC|Reg|Stack |                 |
 +-------------------------+         | T2: PC|Reg|Stack |  (share code,   |
                                     | T3: PC|Reg|Stack |   data, files)  |
         one flow                  +------------------------------------+
                                     three flows of control
```

## Types of threads

| Basis | User-Level Threads (ULT) | Kernel-Level Threads (KLT) |
|---|---|---|
| Managed by | Thread library in user space (`pthread`, Java threads) | The OS kernel directly |
| Kernel knowledge | Kernel sees one process, not the threads | Kernel knows every thread |
| Context switch | Fast, no kernel mode switch | Slower, needs kernel call |
| Blocking call | One blocking thread blocks the whole process | Only that thread blocks |
| Multiprocessor use | Cannot run threads on two cores in parallel | Threads run in parallel on many cores |
| Example | Green threads (old Java) | Linux, Windows, Solaris threads |

## Multithreading models

1. **Many-to-One**: many user threads map to **one** kernel thread. Efficient (no kernel calls) but one blocking call blocks all threads and no parallelism. Example: old Solaris Green Threads.
2. **One-to-One**: each user thread maps to **one** kernel thread. True concurrency, but creating many threads costs kernel overhead. Example: Linux, Windows.
3. **Many-to-Many**: many user threads multiplexed onto a **smaller or equal** number of kernel threads. Combines the best of both; the OS can create enough kernel threads. Example: Solaris before Solaris 9, Windows with user-mode scheduling.
4. (Extra) **Two-level model**: like many-to-many, but a user thread may also be **bound** to a kernel thread (e.g. IRIX, HP-UX).

### Diagrams

```
 MANY-TO-ONE            ONE-TO-ONE            MANY-TO-MANY
 T1  T2  T3             T1  T2  T3            T1  T2  T3  T4
  \   |  /               |   |   |              \  |  /   /
   +--+--+                |   |   |               \ | /  /
       |                  |   |   |                \|/ /
   [ kernel ]          [K1][K2][K3]              [K1] [K2]
       |                  |   |   |                 |    |
     [ CPU ]            [  CPU(s)  ]             [  CPU(s)  ]
  blocks on one I/O    real parallelism        flexible mapping
```

## Thread libraries and threading issues

**Thread libraries** provide the API for creating and managing threads: POSIX **Pthreads** (user or kernel), **Windows threads**, and **Java threads** (managed by the JVM, mapped using one of the models above).

**Threading issues:**

1. **`fork()` and `exec()` semantics**: does `fork()` duplicate only the calling thread or all threads? (`exec()` replaces the whole process.)
2. **Signal handling**: which thread receives a signal — the one it is sent to, every thread, or some thread?
3. **Thread cancellation**: *asynchronous* cancellation may kill a thread in the middle of an update; *deferred* cancellation lets it clean up first.
4. **Thread-local storage (TLS)**: each thread needs its own copy of some data (e.g. `errno`).
5. **Scheduler activations**: kernel communicates with the thread library about upcalls.
6. **Data race/shared data**: shared data must be protected with locks or it corrupts.

**SMT architecture (Simultaneous Multithreading):** a CPU core keeps the execution units of several threads busy in the same cycle. Multiple hardware thread contexts share the core's ALUs; when one thread stalls on a cache miss, another supplies instructions. Intel calls it Hyper-Threading (2 logical CPUs per physical core).

## Example

A word processor uses one thread for the GUI, one for spell-checking, one for auto-save and one for printing. If auto-save blocks on the disk, typing continues because the GUI thread is still running.

## Real-time example

- **Single-threaded**: a tea stall with one worker who takes the order, boils water and serves; each customer waits for the previous one to finish.
- **Multithreaded**: a restaurant kitchen with one cook for rice, one for curry and one for chapati, all using the same kitchen (shared memory) but their own hands (registers/stack).

## Conclusion

A thread is a lightweight unit of execution inside a process, and multithreading gives responsiveness, resource sharing, economy and scalability. The many-to-one, one-to-one and many-to-many models balance efficiency against true parallelism, and user-level versus kernel-level threads decide how much the OS knows about them.

---
