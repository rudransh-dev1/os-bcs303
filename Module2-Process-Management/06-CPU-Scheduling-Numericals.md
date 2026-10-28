# Q. CPU scheduling numericals: FCFS, SJF, SRTF, Round Robin and Priority (Gantt chart, average waiting time, average turnaround time)

**Note:** All Gantt charts and averages below were computed and cross-checked programmatically. Standard formulas:
`Turnaround time (TAT) = Completion time − Arrival time` and `Waiting time (WT) = TAT − Burst time`.

---

## Problem 1 (Dec 2023/Jan 2024 Q4b) — 14 marks

| Process | Arrival | Burst | Priority |
|---|---|---|---|
| P1 | 0 | 9 | 3 |
| P2 | 1 | 4 | 2 |
| P3 | 2 | 9 | 1 |
| P4 | 3 | 5 | 4 |

### (a) FCFS

`Gantt: P1: 0–9 | P2: 9–13 | P3: 13–22 | P4: 22–27`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 9 | 9 | 0 |
| P2 | 13 | 12 | 8 |
| P3 | 22 | 20 | 11 |
| P4 | 27 | 24 | 19 |

**Average WT = 38/4 = 9.5 ms, Average TAT = 65/4 = 16.25 ms**

### (b) SJF (non-preemptive)

`Gantt: P1: 0–9 | P2: 9–13 | P4: 13–18 | P3: 18–27`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 9 | 9 | 0 |
| P2 | 13 | 12 | 8 |
| P3 | 27 | 25 | 16 |
| P4 | 18 | 15 | 10 |

**Average WT = 34/4 = 8.5 ms, Average TAT = 61/4 = 15.25 ms**

### (c) SRTF (preemptive SJF)

`Gantt: P1: 0–1 | P2: 1–5 | P4: 5–10 | P1: 10–18 | P3: 18–27`

At t=1, P2 (4 ms) preempts P1 (8 ms left); at t=5, P4 (5 ms) is shortest; P1 resumes at t=10.

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 18 | 18 | 9 |
| P2 | 5 | 4 | 0 |
| P3 | 27 | 25 | 16 |
| P4 | 10 | 7 | 2 |

**Average WT = 27/4 = 6.75 ms, Average TAT = 54/4 = 13.5 ms**

### (d) Round Robin (q = 2 ms)

`Gantt: P1:0–2 | P2:2–4 | P3:4–6 | P1:6–8 | P4:8–10 | P2:10–12 | P3:12–14 | P1:14–16 | P4:16–18 | P3:18–20 | P1:20–22 | P4:22–23 | P3:23–25 | P1:25–26 | P3:26–27`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 26 | 26 | 17 |
| P2 | 12 | 11 | 7 |
| P3 | 27 | 25 | 16 |
| P4 | 23 | 20 | 15 |

**Average WT = 55/4 = 13.75 ms, Average TAT = 82/4 = 20.5 ms**

### (e) Priority (non-preemptive, smaller number = higher priority)

`Gantt: P1: 0–9 | P3: 9–18 | P2: 18–22 | P4: 22–27`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 9 | 9 | 0 |
| P2 | 22 | 21 | 17 |
| P3 | 18 | 16 | 7 |
| P4 | 27 | 24 | 19 |

**Average WT = 43/4 = 10.75 ms, Average TAT = 70/4 = 17.5 ms**

**Best average waiting time = SRTF (6.75 ms).**

---

## Problem 2 (Model Paper Q3c / Dec 2023 Q4b data) — FCFS and RR (q = 2 ms)

Same table as Problem 1 (P1 9, P2 4, P3 9, P4 5 with arrivals 0, 1, 2, 3).

- **FCFS:** average WT = **9.5 ms**, average TAT = **16.25 ms** (Gantt and table as in Problem 1a).
- **RR (q = 2):** average WT = **13.75 ms**, average TAT = **20.5 ms** (Gantt and table as in Problem 1d).

Observation: RR gives a much better **response** time (every process gets the CPU within 6 ms) but a higher average waiting time.

---

## Problem 3 (Model Paper Q4c) — SRTF and Priority

| Process | Arrival | Burst | Priority |
|---|---|---|---|
| P1 | 0 | 8 | 3 |
| P2 | 1 | 4 | 2 |
| P3 | 2 | 9 | 1 |
| P4 | 3 | 5 | 4 |

### SRTF

`Gantt: P1: 0–1 | P2: 1–5 | P4: 5–10 | P1: 10–17 | P3: 17–26`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 17 | 17 | 9 |
| P2 | 5 | 4 | 0 |
| P3 | 26 | 24 | 15 |
| P4 | 10 | 7 | 2 |

**Average WT = 26/4 = 6.5 ms, Average TAT = 52/4 = 13 ms**

### Priority (non-preemptive, smaller number = higher priority)

`Gantt: P1: 0–8 | P3: 8–17 | P2: 17–21 | P4: 21–26`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 8 | 8 | 0 |
| P2 | 21 | 20 | 16 |
| P3 | 17 | 15 | 6 |
| P4 | 26 | 23 | 18 |

**Average WT = 40/4 = 10 ms, Average TAT = 66/4 = 16.5 ms**

---

## Problem 4 (Dec 2024/Jan 2025 Q4b) — 14 marks, all arrive at t = 0

| Process | Burst | Priority (higher number = higher priority) |
|---|---|---|
| P1 | 24 | 1 |
| P2 | 3 | 2 |
| P3 | 3 | 3 |

### (a) FCFS

`Gantt: P1: 0–24 | P2: 24–27 | P3: 27–30`  →  order P1, P2, P3

| P | TAT | WT |
|---|---|---|
| P1 | 24 | 0 |
| P2 | 27 | 24 |
| P3 | 30 | 27 |

**Average WT = 51/3 = 17 ms, Average TAT = 27 ms**

### (b) SJF

`Gantt: P2: 0–3 | P3: 3–6 | P1: 6–30`

| P | TAT | WT |
|---|---|---|
| P1 | 30 | 6 |
| P2 | 3 | 0 |
| P3 | 6 | 3 |

**Average WT = 9/3 = 3 ms, Average TAT = 13 ms**

### (c) Round Robin (q = 4 ms)

`Gantt: P1: 0–4 | P2: 4–7 | P3: 7–10 | P1: 10–30`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 30 | 30 | 6 |
| P2 | 7 | 7 | 4 |
| P3 | 10 | 10 | 7 |

**Average WT = 17/3 = 5.67 ms, Average TAT = 47/3 = 15.67 ms**

### (d) Priority (higher number = highest priority → P3, P2, P1)

`Gantt: P3: 0–3 | P2: 3–6 | P1: 6–30`

| P | TAT | WT |
|---|---|---|
| P1 | 30 | 6 |
| P2 | 6 | 3 |
| P3 | 3 | 0 |

**Average WT = 9/3 = 3 ms, Average TAT = 13 ms**

---

## Problem 5 (June/July 2025 Q4a) — FCFS, SJF, Priority, RR (q = 1), all arrive at t = 0

| Process | Burst | Priority |
|---|---|---|
| P1 | 10 | 3 |
| P2 | 1 | 1 |
| P3 | 2 | 3 |
| P4 | 1 | 4 |
| P5 | 5 | 2 |

### (a) FCFS

`Gantt: P1: 0–10 | P2: 10–11 | P3: 11–13 | P4: 13–14 | P5: 14–19`

| P | TAT | WT |
|---|---|---|
| P1 | 10 | 0 |
| P2 | 11 | 10 |
| P3 | 13 | 11 |
| P4 | 14 | 13 |
| P5 | 19 | 14 |

**Average WT = 48/5 = 9.6 ms, Average TAT = 67/5 = 13.4 ms**

### (b) SJF

`Gantt: P2: 0–1 | P4: 1–2 | P3: 2–4 | P5: 4–9 | P1: 9–19`

| P | TAT | WT |
|---|---|---|
| P1 | 19 | 9 |
| P2 | 1 | 0 |
| P3 | 4 | 2 |
| P4 | 2 | 1 |
| P5 | 9 | 4 |

**Average WT = 16/5 = 3.2 ms, Average TAT = 35/5 = 7 ms**

### (c) Priority (non-preemptive, smaller number = higher priority)

`Gantt: P2: 0–1 | P5: 1–6 | P1: 6–16 | P3: 16–18 | P4: 18–19`

| P | TAT | WT |
|---|---|---|
| P1 | 16 | 6 |
| P2 | 1 | 0 |
| P3 | 18 | 16 |
| P4 | 19 | 18 |
| P5 | 6 | 1 |

**Average WT = 41/5 = 8.2 ms, Average TAT = 60/5 = 12 ms**

### (d) Round Robin (q = 1)

`Gantt: P1:0–1 | P2:1–2 | P3:2–3 | P4:3–4 | P5:4–5 | P1:5–6 | P3:6–7 | P5:7–8 | P1:8–9 | P5:9–10 | P1:10–11 | P5:11–12 | P1:12–13 | P5:13–14 | P1:14–19`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 19 | 19 | 9 |
| P2 | 2 | 2 | 1 |
| P3 | 7 | 7 | 5 |
| P4 | 4 | 4 | 3 |
| P5 | 14 | 14 | 9 |

**Average WT = 27/5 = 5.4 ms, Average TAT = 46/5 = 9.2 ms**

---

## Problem 6 (Dec 2025/Jan 2026 Q3c) — SRTF, 8 marks

| Process | Arrival | Burst |
|---|---|---|
| P1 | 0 | 9 |
| P2 | 1 | 4 |
| P3 | 2 | 9 |

`Gantt: P1: 0–1 | P2: 1–5 | P1: 5–13 | P3: 13–22`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 13 | 13 | 4 |
| P2 | 5 | 4 | 0 |
| P3 | 22 | 20 | 11 |

**Average WT = 15/3 = 5 ms, Average TAT = 37/3 = 12.33 ms**

---

## Problem 7 (Dec 2025/Jan 2026 Q4c) — Round Robin q = 3, 8 marks

| Process | Arrival | Burst |
|---|---|---|
| P1 | 5 | 5 |
| P2 | 4 | 6 |
| P3 | 3 | 7 |
| P4 | 1 | 9 |
| P5 | 2 | 2 |
| P6 | 6 | 3 |

`Gantt: P4:1–4 | P5:4–6 | P3:6–9 | P2:9–12 | P4:12–15 | P1:15–18 | P6:18–21 | P3:21–24 | P2:24–27 | P4:27–30 | P1:30–32 | P3:32–33`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 32 | 27 | 22 |
| P2 | 27 | 23 | 17 |
| P3 | 33 | 30 | 23 |
| P4 | 30 | 29 | 20 |
| P5 | 6 | 4 | 2 |
| P6 | 21 | 15 | 12 |

**Average WT = 96/6 = 16 ms, Average TAT = 128/6 = 21.33 ms**

---

## Problem 8 (June/July 2024 Supplementary Q3c) — FCFS, preemptive SJF, non-preemptive SJF

| Process | Arrival | Burst |
|---|---|---|
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 9 |
| P4 | 3 | 5 |

### FCFS

`Gantt: P1: 0–8 | P2: 8–12 | P3: 12–21 | P4: 21–26`

| P | TAT | WT |
|---|---|---|
| P1 | 8 | 0 |
| P2 | 11 | 7 |
| P3 | 19 | 10 |
| P4 | 23 | 18 |

**Average WT = 35/4 = 8.75 ms, Average TAT = 61/4 = 15.25 ms**

### Preemptive SJF (SRTF)

`Gantt: P1: 0–1 | P2: 1–5 | P4: 5–10 | P1: 10–17 | P3: 17–26`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 17 | 17 | 9 |
| P2 | 5 | 4 | 0 |
| P3 | 26 | 24 | 15 |
| P4 | 10 | 7 | 2 |

**Average WT = 26/4 = 6.5 ms, Average TAT = 52/4 = 13 ms**

### Non-preemptive SJF

`Gantt: P1: 0–8 | P2: 8–12 | P4: 12–17 | P3: 17–26`

| P | TAT | WT |
|---|---|---|
| P1 | 8 | 0 |
| P2 | 11 | 7 |
| P3 | 24 | 15 |
| P4 | 14 | 9 |

**Average WT = 31/4 = 7.75 ms, Average TAT = 57/4 = 14.25 ms**

---

## Problem 9 (June/July 2025 Makeup Q4b) — FCFS, 10 marks

| Process | Burst |
|---|---|
| P1 | 24 |
| P2 | 3 |
| P3 | 3 |

`Gantt: P1: 0–24 | P2: 24–27 | P3: 27–30`

| P | TAT | WT |
|---|---|---|
| P1 | 24 | 0 |
| P2 | 27 | 24 |
| P3 | 30 | 27 |

**Average WT = 51/3 = 17 ms, Average TAT = 27 ms**

This is the classic convoy effect: one long job delays all short jobs, which is why SJF is preferred when burst times are known.

---

## Conclusion (write this line in the exam)

SRTF/SJF gives the **minimum average waiting time**, FCFS is the **simplest but suffers from the convoy effect**, Round Robin gives the **best response time** for time-sharing systems, and Priority scheduling honours importance but risks **starvation** (solved by aging). Approximately: SRTF < SJF < Priority < FCFS < RR for average waiting time in the above problems.

---
