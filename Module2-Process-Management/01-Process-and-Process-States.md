# Q. Define a process. Explain the different states of a process with a state diagram.

**Asked in:** Dec 2023/Jan 2024 Q3a · Model Paper Q3a · Dec 2024/Jan 2025 Q3a · June/July 2025 Q3a · June/July 2025 Makeup Q3a · June/July 2024 Supplementary Q3a · Dec 2025/Jan 2026 Q4b

## Definition

A **process** is a program in execution. A program is a passive entity (a file on the disk), while a process is an active entity that has a program counter, registers and a stack showing the next instruction to run.

> Process = Program code + Data + Program counter + Registers + Stack (current activity)

## Points

1. A program becomes a process when it is loaded into memory and given a PCB.
2. The same program can create many processes (opening a music player twice gives two processes).
3. Memory layout of a process has four parts: **Text** (code), **Data** (global variables), **Heap** (dynamic memory from `new`/`malloc`) and **Stack** (function calls, local variables, return addresses).
4. A process passes through five states during its lifetime: **New, Ready, Running, Waiting, Terminated**.
5. Only **one** process can be in the Running state on one CPU core at a time, but many can be Ready or Waiting.
6. The OS keeps track of the current state of every process in its PCB (Process Control Block).

## The five states

| State | Meaning |
|---|---|
| **New** | The process is being created. |
| **Ready** | The process is in main memory and waiting for the CPU. |
| **Running** | Instructions of the process are being executed by the CPU. |
| **Waiting** (Blocked) | The process is waiting for an event or I/O to complete. |
| **Terminated** (Exit) | The process has finished execution. |

## Diagram: Process state transition diagram

```
        admitted            scheduler dispatch
 NEW -----------> READY --------------------> RUNNING ----------> TERMINATED
                    ^  <-------------------------   |   exit
                    |        interrupt              |
                    |                               | I/O or event wait
                    +-------------------------------+
                            I/O or event completion
                                  WAITING
```

**Transitions (important for exams):**

1. **New → Ready**: the OS admits the process into memory.
2. **Ready → Running**: the short-term scheduler (dispatcher) gives it the CPU.
3. **Running → Waiting**: the process requests I/O or waits for an event.
4. **Waiting → Ready**: the I/O or event completes.
5. **Running → Ready**: the process is preempted (time quantum expired or higher-priority process arrived).
6. **Running → Terminated**: the process calls `exit()` and finishes.

## Example (VTU style)

When you double-click a `.class` file, the OS creates a process: it is **New** while loading, becomes **Ready** once in memory, moves to **Running** when the CPU is given, goes to **Waiting** while reading a file from disk, returns to **Ready** after the read finishes, and finally becomes **Terminated** when `main()` ends.

## Real-time example

A railway ticket counter:

| State | Real life |
|---|---|
| New | You join the queue and take a token |
| Ready | You are standing in the queue, waiting for your turn |
| Running | You are at the counter, the clerk is serving you |
| Waiting | The clerk asks you to wait while the server checks your details |
| Terminated | Your ticket is printed and you leave the counter |

## Conclusion

Thus a process is a program under execution, and its five-state model (New, Ready, Running, Waiting, Terminated) lets the OS keep the CPU busy and manage many processes efficiently. The state changes are recorded in the PCB, which makes the process the basic unit of work in an operating system.

---
