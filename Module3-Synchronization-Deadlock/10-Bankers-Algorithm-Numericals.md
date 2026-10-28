# Q. Banker's algorithm numericals: Need matrix, safe state, safe sequence and request checking

**Formulas used in every problem**

- `Need[i][j] = Max[i][j] − Allocation[i][j]`
- `Available = Total instances − Sum of all Allocation` (when total instances are given)
- A process can finish if `Need[i] ≤ Work` for all resources; then `Work = Work + Allocation[i]`.
- If all processes can finish → system is in a **safe state** and the order of finishing is the **safe sequence**.

All results below were computed and cross-checked programmatically.

---

## Problem 1 (Dec 2023/Jan 2024 Q6b) — 14 marks, four resources

| Process | Allocation A B C D | Max A B C D | Available A B C D |
|---|---|---|---|
| P0 | 2 0 0 1 | 4 2 1 2 | 3 3 2 1 |
| P1 | 3 1 2 1 | 5 2 5 2 | |
| P2 | 2 1 0 3 | 2 3 1 6 | |
| P3 | 1 3 1 2 | 1 4 2 4 | |
| P4 | 1 4 3 2 | 3 6 6 5 | |

### (i) Need matrix

| Process | Need A B C D |
|---|---|
| P0 | 2 2 1 1 |
| P1 | 2 1 3 1 |
| P2 | 0 2 1 3 |
| P3 | 0 1 1 2 |
| P4 | 2 2 3 3 |

### (ii) Is the system safe? Safe sequence

| Step | Work before | Process chosen (Need ≤ Work) | Work after = Work + Allocation |
|---|---|---|---|
| 1 | 3 3 2 1 | **P0** (2 2 1 1) | 5 3 2 2 |
| 2 | 5 3 2 2 | **P3** (0 1 1 2) | 6 6 3 4 |
| 3 | 6 6 3 4 | **P4** (2 2 3 3) | 7 10 6 6 |
| 4 | 7 10 6 6 | **P1** (2 1 3 1) | 10 11 8 7 |
| 5 | 10 11 8 7 | **P2** (0 2 1 3) | 12 12 8 10 |

**Yes, the system is in a safe state. Safe sequence: P0 → P3 → P4 → P1 → P2.**
(Other valid sequences exist, e.g. P0 → P3 → P1 → P2 → P4; any one safe sequence is a full-marks answer.)

### (iii) Can P2's request (0, 1, 1, 3) be granted immediately?

1. `Request ≤ Need` → (0,1,1,3) ≤ (0,2,1,3) → **true**.
2. `Request ≤ Available` → (0,1,1,3) ≤ (3,3,2,1)? The **D** component needs 3 but only 1 is available → **false**.

**Answer: the request cannot be granted immediately; P2 must wait.**

---

## Problem 2 (Model Paper Q5c · Dec 2025/Jan 2026 Q6c · June/July 2025 Q6b) — four resources

| Process | Allocation A B C D | Max A B C D | Available A B C D |
|---|---|---|---|
| P0 | 0 0 1 2 | 0 0 1 2 | 1 5 2 0 |
| P1 | 1 0 0 0 | 1 7 5 0 | |
| P2 | 1 3 5 4 | 2 3 5 6 | |
| P3 | 0 6 3 2 | 0 6 5 2 | |
| P4 | 0 0 1 4 | 0 6 5 6 | |

### (a) Need matrix

| Process | Need A B C D |
|---|---|
| P0 | 0 0 0 0 |
| P1 | 0 7 5 0 |
| P2 | 1 0 0 2 |
| P3 | 0 0 2 0 |
| P4 | 0 6 4 2 |

### (b) Safe state and safe sequence

| Step | Work before | Process chosen | Work after |
|---|---|---|---|
| 1 | 1 5 2 0 | **P0** (0 0 0 0) | 1 5 3 2 |
| 2 | 1 5 3 2 | **P2** (1 0 0 2) | 2 8 8 6 |
| 3 | 2 8 8 6 | **P3** (0 0 2 0) | 2 14 11 8 |
| 4 | 2 14 11 8 | **P4** (0 6 4 2) | 2 14 12 12 |
| 5 | 2 14 12 12 | **P1** (0 7 5 0) | 3 14 12 12 |

**Yes, safe. Safe sequence: P0 → P2 → P3 → P4 → P1.**
(P0 → P2 → P1 → P3 → P4 is also valid.)

### (c) Request from P1 for (0, 4, 2, 0) — grant immediately?

1. `Request ≤ Need` → (0,4,2,0) ≤ (0,7,5,0) → **true**.
2. `Request ≤ Available` → (0,4,2,0) ≤ (1,5,2,0) → **true**.
3. Pretend to grant: `Available = (1,5,2,0) − (0,4,2,0) = (1,1,0,0)`;
   `Allocation(P1) = (1,4,2,0)`; `Need(P1) = (0,3,3,0)`.

Safety check after granting:

| Step | Work | Process | Work after |
|---|---|---|---|
| 1 | 1 1 0 0 | P0 (0 0 0 0) | 1 1 1 2 |
| 2 | 1 1 1 2 | P2 (1 0 0 2) | 2 4 6 6 |
| 3 | 2 4 6 6 | P1 (0 3 3 0) | 3 8 8 6 |
| 4 | 3 8 8 6 | P3 (0 0 2 0) | 3 14 11 8 |
| 5 | 3 14 11 8 | P4 (0 6 4 2) | 3 14 12 12 |

Safe sequence after granting: P0 → P2 → P1 → P3 → P4. **The request can be granted immediately.**

---

## Problem 3 (Dec 2024/Jan 2025 Q6b · Dec 2025/Jan 2026 Q5b · June/July 2025 Makeup Q6c · June/July 2024 Supplementary Q5c) — three resources

Resource instances present in the system: **A = 10, B = 5, C = 7**.

| Process | Allocation A B C | Max A B C | Available A B C |
|---|---|---|---|
| P0 | 0 1 0 | 7 5 3 | 3 3 2 |
| P1 | 2 0 0 | 3 2 2 | |
| P2 | 3 0 2 | 9 0 2 | |
| P3 | 2 1 1 | 2 2 2 | |
| P4 | 0 0 2 | 4 3 3 | |

**Check of Available:** allocated totals = A 7, B 2, C 5; so available = (10−7, 5−2, 7−5) = **(3, 3, 2)** ✓

### (i) Need matrix

| Process | Need A B C |
|---|---|
| P0 | 7 4 3 |
| P1 | 1 2 2 |
| P2 | 6 0 0 |
| P3 | 0 1 1 |
| P4 | 4 3 1 |

### (ii) Safe state / safe sequence

| Step | Work before | Process chosen | Work after |
|---|---|---|---|
| 1 | 3 3 2 | **P1** (1 2 2) | 5 3 2 |
| 2 | 5 3 2 | **P3** (0 1 1) | 7 4 3 |
| 3 | 7 4 3 | **P4** (4 3 1) | 7 4 5 |
| 4 | 7 4 5 | **P0** (7 4 3) | 7 5 5 |
| 5 | 7 5 5 | **P2** (6 0 0) | 10 5 7 |

**Yes, the system is safe. Safe sequence: P1 → P3 → P4 → P0 → P2.**
(P1 → P3 → P0 → P2 → P4 is also valid.)

### (iii) Request from P1 for (1, 0, 2) — grant immediately?

1. `Request ≤ Need` → (1,0,2) ≤ (1,2,2) → **true**.
2. `Request ≤ Available` → (1,0,2) ≤ (3,3,2) → **true**.
3. Pretend to grant: `Available = (2,3,0)`; `Allocation(P1) = (3,0,2)`; `Need(P1) = (0,2,2)`.

| Step | Work | Process | Work after |
|---|---|---|---|
| 1 | 2 3 0 | P1 (0 2 2) | 5 3 2 |
| 2 | 5 3 2 | P3 (0 1 1) | 7 4 3 |
| 3 | 7 4 3 | P4 (4 3 1) | 7 4 5 |
| 4 | 7 4 5 | P0 (7 4 3) | 7 5 5 |
| 5 | 7 5 5 | P2 (6 0 0) | 10 5 7 |

**The request can be granted immediately** (system stays safe, sequence P1 → P3 → P4 → P0 → P2).

---

## Method to write in the exam (5 steps)

1. Write the **Need matrix** = Max − Allocation.
2. Write `Work = Available`, `Finish[i] = false` for all i.
3. Find an `i` with `Finish[i] = false` and `Need[i] ≤ Work`; set `Work = Work + Allocation[i]`, `Finish[i] = true`; repeat.
4. If all `Finish[i] = true` → **safe**, and the order is the **safe sequence**. Otherwise → **unsafe** (no safe sequence).
5. For a request: check `Request ≤ Need`, then `Request ≤ Available`, then pretend to grant it and repeat step 3. If safe → grant, else → wait (and tell the requester to wait).

## Conclusion

Banker's algorithm keeps the system in a safe state by granting a request only when the pretend-granted state still allows every process to finish. The Need matrix plus the Work table gives the safe sequence, and the three checks (need, availability, safety after granting) answer every request question.

---
