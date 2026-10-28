# Q. Explain the scheduling criteria. Discuss the multilevel queue scheduling algorithm. Explain short, medium and long-term schedulers.

**Asked in:** Model Paper Q4b (five criteria) · Dec 2025/Jan 2026 Q3b (multilevel queue) · June/July 2025 Q3b (schedulers)

## Part A: CPU scheduling criteria

**Definition:** CPU scheduling decides which process in the ready queue gets the CPU next. To compare algorithms we use five criteria.

| # | Criterion | Meaning | Goal |
|---|---|---|---|
| 1 | **CPU utilization** | Percentage of time the CPU is busy doing useful work | Maximize (keep CPU as busy as possible, 40%–90%) |
| 2 | **Throughput** | Number of processes completed per unit time | Maximize |
| 3 | **Turnaround time** | Total time from submission to completion: `TAT = Completion time − Arrival time` | Minimize |
| 4 | **Waiting time** | Total time spent waiting in the ready queue: `WT = TAT − Burst time` | Minimize |
| 5 | **Response time** | Time from submission to the **first** response (not completion) | Minimize (important for interactive systems) |

**Points**

1. Waiting time and turnaround time are the two used in numerical problems.
2. Response time matters in time-sharing systems where the user is typing at a terminal.
3. There is a trade-off: making the average waiting time small with SJF may increase the waiting time of long jobs (**starvation**).
4. Interactive systems care about **variance** in response time (predictability), not just the average.
5. Preemptive scheduling favours response time; non-preemptive favours low overhead.

## Part B: Multilevel Queue Scheduling

**Definition:** Multilevel queue scheduling divides the ready queue into **several separate queues**, each with its own priority and its own scheduling algorithm. A process is permanently assigned to one queue (e.g. based on priority, memory size or process type).

### Points

1. Example queues (highest to lowest priority): **system processes → interactive processes → interactive editing → batch processes → student processes**.
2. Each queue has its own algorithm: foreground queue = **RR** (good response time), background queue = **FCFS** (less overhead).
3. **Fixed preemptive priority** is used between queues: a process in a higher queue always runs before any process in a lower queue (a batch job runs only when the higher queues are empty).
4. **Problem: starvation.** If higher-priority queues are always full, low-priority processes may never run.
5. **Solution: time slicing** between queues: the CPU is divided, e.g. 80% to the foreground RR queue and 20% to the background FCFS queue.
6. **Multilevel feedback queue** is an improvement: a process can **move** between queues (e.g. a CPU-bound job is demoted, an I/O-bound job is promoted).

### Diagram

```
        +---------------------------+
        | System processes    (RR)  |  highest priority
        +---------------------------+
        | Interactive processes(RR) |
        +---------------------------+
        | Interactive editing (RR) |   -> each queue is served only
        +---------------------------+      when the ones above are empty
        | Batch processes   (FCFS)  |
        +---------------------------+
        | Student processes (FCFS)  |  lowest priority
        +---------------------------+
                    |
                    v
                  [ CPU ]
```

### Example

In a university server, faculty jobs (queue 1) are served before student print jobs (queue 4). A student print job waits until all faculty jobs are finished; with time slicing, the print queue still gets 20% of CPU time.

**Real-time example:** A hospital's OPD. **Emergency** patients are seen first, then **appointments**, then **walk-ins**, then **health check-ups**. Each desk uses its own rule, but a lower-priority patient is only called when the higher desks are free (and starvation is prevented by reserving some slots).

## Part C: Short, medium and long-term schedulers

| Basis | Long-term scheduler | Medium-term scheduler | Short-term scheduler |
|---|---|---|---|
| Also called | Job scheduler | Swapper | CPU scheduler |
| Selects | Jobs from the job pool / disk and loads them into memory (New → Ready) | Processes swapped out of memory and back (suspended states) | Process from ready queue to run on the CPU (Ready → Running) |
| Frequency | Rarely (seconds/minutes) | Only under memory pressure | Very frequently (milliseconds) |
| Speed | Very slow | Slow | Must be very fast |
| Controls | **Degree of multiprogramming** | Degree of multiprogramming (temporarily reduces it) | No control over multiprogramming |
| Effect if too slow | Fewer processes in memory | Thrashing avoided but slower response | High CPU overhead |

**Points**

1. The long-term scheduler should mix **I/O-bound** and **CPU-bound** processes so the CPU and I/O devices stay busy.
2. The medium-term scheduler performs **swapping** to reduce memory pressure (used in time-sharing systems).
3. The short-term scheduler must be fast; if it takes 10 ms to decide for a 10 ms quantum, 50% of CPU time is wasted.
4. The **dispatcher** is the module that actually gives the CPU to the chosen process (context switch, user mode switch, jump to the right location). **Dispatch latency** is its cost.

## Conclusion

The five scheduling criteria (utilization, throughput, turnaround, waiting, response time) give a standard way to compare algorithms. Multilevel queue scheduling gives different classes of processes different priorities and algorithms, with time slicing to avoid starvation, and the long/medium/short-term schedulers work at three different levels to keep the CPU busy and memory well used.

---
