# Q. Memory partitions: first-fit, best-fit and worst-fit allocation

**Asked in:** June/July 2024 Supplementary Q6c

## Definition

**Dynamic memory allocation** gives memory to a process from a list of free blocks (holes). To select which hole to use, the OS uses a placement algorithm:

- **First-fit**: allocate the **first** hole that is big enough.
- **Best-fit**: allocate the **smallest** hole that is big enough (leaves the smallest leftover).
- **Worst-fit**: allocate the **largest** hole (leaves the largest leftover).

## Problem

Memory partitions (in order): **100 KB, 500 KB, 200 KB, 300 KB, 600 KB**
Processes (in order): **212 KB, 417 KB, 112 KB, 426 KB**

## First-fit

| Process | Partition chosen | Space left |
|---|---|---|
| 212 KB | 500 KB (first hole ≥ 212) | 288 KB |
| 417 KB | 600 KB (500 KB block now only 288) | 183 KB |
| 112 KB | 288 KB block (first fit) | 176 KB |
| 426 KB | **none — must wait** (holes left: 100, 176, 200, 300, 183) | — |

**Total internal fragmentation:** 100 + 176 + 200 + 300 + 183 KB stays free, and one process is not allocated.

## Best-fit

| Process | Partition chosen | Space left |
|---|---|---|
| 212 KB | 300 KB (smallest hole ≥ 212) | 88 KB |
| 417 KB | 500 KB | 83 KB |
| 112 KB | 200 KB | 88 KB |
| 426 KB | 600 KB | 174 KB |

**All four processes are allocated.** Leftover holes: 100, 88, 83, 88, 174 KB.

## Worst-fit

| Process | Partition chosen | Space left |
|---|---|---|
| 212 KB | 600 KB (largest hole) | 388 KB |
| 417 KB | 500 KB | 83 KB |
| 112 KB | 388 KB | 276 KB |
| 426 KB | **none — must wait** (largest remaining hole is 300 KB) | — |

## Comparison

| Basis | First-fit | Best-fit | Worst-fit |
|---|---|---|---|
| Rule | First hole that fits | Smallest hole that fits | Largest hole |
| Speed | Fastest (search stops early) | Slowest (search whole list) | Slow (search whole list) |
| Leftover | May split big holes | Leaves tiny useless holes | Leaves big reusable holes |
| Result here | 3 allocated, 1 waits | **All 4 allocated** | 3 allocated, 1 waits |

**Which algorithm makes the most efficient use of memory? → Best-fit**, because it allocated all four processes by using the holes that were just large enough, leaving no process waiting. (First-fit is generally the fastest and is often preferred in practice for its speed, but for this data set best-fit is the most efficient.)

## Real-time example

Parking a bike in a parking lot: **first-fit** = take the first empty slot you see; **best-fit** = park in the tightest slot that fits your bike (keeps big slots for cars); **worst-fit** = park in the biggest empty area (leaves a large space that is still usable by a car).

## Conclusion

First-fit is fastest but can waste large holes; best-fit wastes the least total space for this problem and allocated every process; worst-fit leaves the largest usable leftover but failed here. Fragmentation (external between holes, internal inside a partition) is the main drawback of all three, and compaction or paging is used to reduce it.

---
