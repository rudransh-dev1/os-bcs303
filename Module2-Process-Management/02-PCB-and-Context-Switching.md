# Q. What is a Process Control Block (PCB)? Explain its fields with a diagram. Also explain context switching.

**Asked in:** Dec 2023/Jan 2024 Q3c (context switching) · Model Paper Q3a · Dec 2024/Jan 2025 Q3a · Dec 2025/Jan 2026 Q4b

## Definition

A **Process Control Block (PCB)**, also called a task control block, is a data structure maintained by the operating system kernel that stores all the information about a particular process. In Linux it is called `task_struct`.

> The PCB is the identity card of a process: it holds its state, registers, memory details and open files.

## Points

1. A PCB is created when the process enters the **New** state and is deleted when the process **Terminates**.
2. The OS keeps a PCB for every process; the collection of PCBs is stored in a **process table**.
3. It is used during a **context switch**, because the CPU state is saved in and restored from the PCB.
4. It allows the OS to stop a process at any point and resume it later from exactly the same place.
5. It also helps in scheduling decisions (priority, queue pointers) and memory protection (base and limit registers).

## Fields of a PCB

1. **Process state**: new, ready, running, waiting or terminated.
2. **Process ID (PID)**: unique number of the process; parent PID is also stored.
3. **Program counter**: address of the next instruction to be executed.
4. **CPU registers**: general registers, stack pointer, accumulator. These are saved on an interrupt so the process can continue correctly.
5. **CPU-scheduling information**: priority of the process, pointers to scheduling queues, other parameters.
6. **Memory-management information**: base and limit registers, page tables or segment tables.
7. **Accounting information**: CPU time used, start time, time limits, account numbers.
8. **I/O status information**: list of open files, allocated I/O devices, pending I/O requests.

## Diagram: PCB

```
   +-------------------------------------+
   | Entries in the Process Control Block |
   +-------------------------------------+
   | Process state        (Running/Ready) |
   | Process number / PID                 |
   | Program counter (next instruction)   |
   | CPU registers                        |
   | CPU-scheduling information           |
   | Memory-management information        |
   | Accounting information               |
   | I/O status information               |
   +-------------------------------------+
```

```
 Process Table
 +----------+     +---------------------+
 | PCB 1    |---->| state, PC, registers|
 +----------+     +---------------------+
 | PCB 2    |---->| state, PC, registers|   -> used on every context switch
 +----------+     +---------------------+
```

## Context switching

**Definition:** A **context switch** is the process of saving the context (CPU state) of the currently running process into its PCB and loading the saved context of the next process from its PCB.

### Points

1. "Context" means the contents of the CPU registers, the program counter and the process state.
2. It happens on an interrupt, a time-quantum expiry, an I/O request, or when a higher-priority process arrives.
3. Steps:
   1. The OS saves the running process's PCB (PC, registers, state → Ready/Waiting).
   2. The scheduler selects the next process.
   3. The OS loads that process's PCB values into the CPU.
   4. The CPU resumes execution of the new process.
4. Context switching is **pure overhead**; no useful user work is done while it happens.
5. Its cost depends on hardware support (e.g. multiple register sets in some CPUs reduce it).
6. If the time quantum is too small or switching is too frequent, throughput falls.

## Example

Two processes A and B run with round robin (quantum = 4 ms). At 4 ms, A is interrupted: the OS saves A's program counter and registers in PBC-A, marks A **Ready**, picks B, loads B's registers from PCB-B and marks B **Running**. When A is later scheduled again, it continues from exactly the instruction where it stopped.

## Real-time example

In a call centre, an agent (CPU) handles one customer at a time. When a customer is put on hold to fetch details (context switch), the agent writes the status on the customer's file (PCB), picks the next caller, and later returns to the first caller exactly from where they paused.

## Conclusion

The PCB is the central record of a process, and context switching uses the PCB to swap processes on the CPU. Together they give the OS multitasking: many processes can share one CPU safely and resume without losing data. The only cost is the small overhead of saving and loading the state.

---
