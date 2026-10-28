# Q. What is Inter-Process Communication (IPC)? Explain shared memory and message passing. Explain direct and indirect communication.

**Asked in:** Dec 2023/Jan 2024 Q3b · Model Paper Q3b · Dec 2024/Jan 2025 Q3b · Dec 2025/Jan 2026 Q3a · June/July 2025 Makeup Q3b · June/July 2024 Supplementary Q4b · June/July 2025 Q3c

## Definition

**Inter-Process Communication (IPC)** is the mechanism that allows cooperating processes to exchange data and synchronize their actions. Processes that cooperate need IPC because one process's output may become another process's input.

## Points

1. A process can be **independent** (cannot affect others) or **cooperating** (shares data with others). IPC is needed only for cooperating processes.
2. Cooperation is needed for **information sharing, computation speed-up, modularity and convenience**.
3. The two fundamental IPC models are **shared memory** and **message passing**.
4. IPC also needs **synchronization**, otherwise a race condition can corrupt shared data.

## Model 1: Shared memory

1. A region of memory is created and shared; both processes attach it to their address space.
2. Processes read and write directly into the shared region, like ordinary memory access.
3. **Fastest** IPC: no kernel involvement after the region is set up, only memory copies.
4. Needs **synchronization** (semaphores/mutex) so that two processes do not write at the same time.
5. The OS sets up the mapping only once; after that data is not copied through the kernel.
6. System calls: `shmget()`, `shmat()`, `shmdt()` in Unix; `CreateFileMapping()` in Windows.

## Model 2: Message passing

1. Processes exchange messages (fixed or variable size) through the kernel.
2. No shared address space is needed, so it works across machines in a distributed system.
3. It is **slower** than shared memory because every message is copied twice (to kernel and to receiver).
4. The OS handles synchronization itself; the programmer need not write semaphores.
5. A message has a **header** (type, size, destination/source ID, control info) and a **body** (data).
6. System calls: `send(message)` and `receive(message)`; `pipe()`, sockets, `msgsnd()`.

## Direct vs indirect communication

| Basis | Direct communication | Indirect communication |
|---|---|---|
| Naming | Send/receive to a **process ID**: `send(P2, msg)`, `receive(P1, msg)` | Send/receive to a **mailbox (port)**: `send(A, msg)`, `receive(A, msg)` |
| Link | Exactly one link between one pair of processes | A mailbox can be shared by many processes |
| Number of pairs | One link per pair | One link per mailbox (can be used by many) |
| Symmetric form | Both sender and receiver name the other | Only the mailbox is named |
| Flexibility | Rigid, less flexible | More flexible, easier to implement |
| Example | UNIX pipe between parent and child | Message queues, ports in distributed systems |

## Synchronous (blocking) vs asynchronous (non-blocking)

| Basis | Synchronous | Asynchronous |
|---|---|---|
| Blocking send | Sender blocks until the message is received | Sender sends and continues immediately |
| Blocking receive | Receiver blocks until a message is available | Receiver gets an invalid/null message or checks later |
| Advantage | Simple, correct, easy to reason about | High concurrency, faster sender |
| Disadvantage | May cause longer waiting, deadlock if both wait | Buffer management becomes complex |
| Example | Pipe read/write, RPC | Email-like queues, `send()` with buffering |

## Diagram

```
   SHARED MEMORY                       MESSAGE PASSING
   +-----------+                       +-----------+     +-----------+
   | Process A |                       | Process A |     | Process B |
   +-----+-----+                       +-----+-----+     +-----+-----+
         |                                   |                 ^
         v                                   v                 |
   +-----------+  no kernel copy        +-----------------------------+
   |  Shared   |  (fastest)            |     Kernel message queue    |
   |  Region   |                       |  send() ------------->      |
   +-----------+                       +-----------------------------+
         ^                             (2 copies, slower but safe)
         |
   +-----+-----+
   | Process B |
   +-----------+
```

## Example

**Shared memory:** Two processes (producer and consumer) share a buffer array. The producer writes into `buffer[in]`, the consumer reads from `buffer[out]`, and `in`/`out` are updated with a mutex held.

**Message passing:** A browser passes a URL to a web-server process as a message; the server replies with the page as another message.

## Real-time example

- **Shared memory**: WhatsApp chat and its notification service on your phone share one database region in memory.
- **Message passing**: Posting a parcel. You do not go into the other person's house (no shared memory); the courier company (kernel/mailbox) carries the parcel from sender to receiver.

## Conclusion

IPC enables cooperating processes to exchange data. Shared memory is fastest but needs explicit synchronization, while message passing is slower but safer and works across machines. Direct/indirect naming and synchronous/asynchronous behaviour decide which model fits the application.

---
