# Operating Systems (BCS303) — Module 2 & Module 3 Exam Answers

Exam-ready answers for VTU 3rd semester **Operating Systems (BCS303)**, covering **Module 2
(Process Management, Threads and CPU Scheduling)** and **Module 3 (Process Synchronization
and Deadlock)**.

Source material: 7 readable BCS303 question papers (Dec 2023/Jan 2024 → Dec 2025/Jan 2026,
including the Model paper, the June/July 2025 Makeup paper and the June/July 2024 Supplementary
paper) + the Module 1–3 study notes and the Silberschatz textbook notes. Every Module 2 and
Module 3 question that appears in those papers is answered here.

Module 1 answers already exist in the companion repo
[`t23050288-cmyk/os-bcs303`](https://github.com/t23050288-cmyk/os-bcs303). This repo adds the
missing **Module 2 and Module 3**.

## How every answer is structured (same as the Module 1 notes)

1. **Definition** — one or two exam-safe lines.
2. **Points** — 5 to 10 numbered points.
3. **Diagram** — ASCII diagram wherever the question says "with a neat diagram".
4. **Example** — a small worked/step-by-step example.
5. **Real-time example** — an everyday analogy (ticket counter, tea stall, washroom key...).
6. **Conclusion** — the closing line to write at the end of a 20-mark answer.

Every numerical (CPU scheduling, Banker's algorithm, memory fit) was **computed and
cross-checked programmatically**, so the Gantt charts, Need matrices, safe sequences and
averages are verified, not guessed.

## Module 2 — Process Management, Threads and CPU Scheduling

| # | Topic | File |
|---|---|---|
| 1 | Process, process states and state diagram | [01-Process-and-Process-States.md](Module2-Process-Management/01-Process-and-Process-States.md) |
| 2 | PCB, PCB fields and context switching | [02-PCB-and-Context-Switching.md](Module2-Process-Management/02-PCB-and-Context-Switching.md) |
| 3 | Inter-process communication: shared memory, message passing, direct/indirect, sync/async | [03-IPC.md](Module2-Process-Management/03-IPC.md) |
| 4 | Threads, multithreading benefits, ULT vs KLT, models, libraries, issues, SMT | [04-Threads-and-Multithreading-Models.md](Module2-Process-Management/04-Threads-and-Multithreading-Models.md) |
| 5 | Scheduling criteria, multilevel queue, short/medium/long-term schedulers | [05-Scheduling-Criteria-and-Multilevel-Queue.md](Module2-Process-Management/05-Scheduling-Criteria-and-Multilevel-Queue.md) |
| 6 | **All CPU scheduling numericals** (FCFS, SJF, SRTF, RR, Priority) | [06-CPU-Scheduling-Numericals.md](Module2-Process-Management/06-CPU-Scheduling-Numericals.md) |

## Module 3 — Process Synchronization and Deadlock

| # | Topic | File |
|---|---|---|
| 1 | Critical-section problem, three requirements, Peterson's solution | [01-Critical-Section-and-Petersons.md](Module3-Synchronization-Deadlock/01-Critical-Section-and-Petersons.md) |
| 2 | Synchronization hardware: TestAndSet, compare_and_swap, atomic variables | [02-Synchronization-Hardware.md](Module3-Synchronization-Deadlock/02-Synchronization-Hardware.md) |
| 3 | Semaphores and mutual exclusion | [03-Semaphores-and-Mutual-Exclusion.md](Module3-Synchronization-Deadlock/03-Semaphores-and-Mutual-Exclusion.md) |
| 4 | Producer–consumer (bounded buffer) with algorithm | [04-Producer-Consumer.md](Module3-Synchronization-Deadlock/04-Producer-Consumer.md) |
| 5 | Readers–writers problem with code | [05-Readers-Writers.md](Module3-Synchronization-Deadlock/05-Readers-Writers.md) |
| 6 | Dining philosophers: structure and semaphore solution | [06-Dining-Philosophers.md](Module3-Synchronization-Deadlock/06-Dining-Philosophers.md) |
| 7 | Deadlock and the four necessary conditions | [07-Deadlock-and-Necessary-Conditions.md](Module3-Synchronization-Deadlock/07-Deadlock-and-Necessary-Conditions.md) |
| 8 | Deadlock handling: prevention, avoidance, detection, recovery | [08-Deadlock-Handling-and-Recovery.md](Module3-Synchronization-Deadlock/08-Deadlock-Handling-and-Recovery.md) |
| 9 | Resource allocation graph | [09-Resource-Allocation-Graph.md](Module3-Synchronization-Deadlock/09-Resource-Allocation-Graph.md) |
| 10 | **All Banker's algorithm numericals** (Need matrix, safe sequence, requests) | [10-Bankers-Algorithm-Numericals.md](Module3-Synchronization-Deadlock/10-Bankers-Algorithm-Numericals.md) |
| 11 | First-fit, best-fit, worst-fit numerical | [11-Fit-Algorithms-Numerical.md](Module3-Synchronization-Deadlock/11-Fit-Algorithms-Numerical.md) |

## Paper-wise index

See [Papers-Module2-3-Question-Map.md](Papers-Module2-3-Question-Map.md) — every Module 2 and
Module 3 question from every paper, with the file that answers it. Each answer file also lists
the exact papers and question numbers where that question was asked.

## Note on source papers

- The two files named "June/July 2025" (`BCS303-june-july-2025.pdf` and `BCS303-june-july-20251.pdf`)
  are **identical duplicates** — counted once.
- Two PDFs were **scanned images without a text layer** (`BCS3032.pdf` and `BCS303JULY2024.pdf`),
  so their questions could not be extracted here. If you share clear photos/screenshots of them,
  they can be added.
