# Sorting as a Prerequisite

## The Core Constraint

Binary search requires **sorted order** because it eliminates halves based on comparison. If the array is unsorted, knowing arr[mid] < target tells you nothing about which half contains the target.

---

## Cost Analysis: Sort Once, Search Many

| Strategy | Cost for Q queries on N elements |
|---|---|
| Linear search × Q | O(Q × N) |
| Sort once + binary search | O(N log N) + O(Q × log N) |

**Break-even point:** Sort pays off when Q > log N (roughly — for large N, this is almost immediately).

For N=10⁶: log N ≈ 20. Even 21 queries make binary search cheaper after one sort.

---

## When Sorting Is Too Expensive

- **Single query, unsorted data:** Linear search costs O(N); sorting + binary search costs O(N log N). Linear wins.
- **Streaming data:** Maintaining sorted order dynamically costs O(log N) per insertion (e.g., balanced BST).
- **Complex keys:** Sometimes comparison is expensive, making sort overhead significant.

---

## Sort Stability and Binary Search

Binary search finds *a* matching index, not necessarily the first or last. For first/last occurrence, additional logic is needed (covered in LO 10.13.7).

---

## Further Reading

- [Sorting algorithms comparison](https://en.wikipedia.org/wiki/Sorting_algorithm#Comparison_of_algorithms)


---

# Classic Binary Search

## The Algorithm

Given a sorted array and a target, binary search repeatedly halves the search space until the target is found or the space is empty.

```
lo ← 0
hi ← len(arr) - 1
WHILE lo <= hi:
    mid ← lo + (hi - lo) // 2
    IF arr[mid] == target: RETURN mid
    IF arr[mid] < target:  lo ← mid + 1
    ELSE:                  hi ← mid - 1
RETURN -1
```

![Binary search trace on [2,5,8,12,16,23,38,56,72,91], target=23](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/binary_search_trace.png)

---

## The Mid Calculation

**Why `lo + (hi - lo) // 2` instead of `(lo + hi) // 2`?**

`lo + hi` can overflow a 32-bit integer when both are large positive numbers. The safe formula avoids overflow while computing the same value.

---

## Loop Termination Condition

`lo <= hi`: the search space has at least one element. When `lo > hi`, the space is empty — target not found.

---

## Further Reading

- [Binary Search — Wikipedia](https://en.wikipedia.org/wiki/Binary_search_algorithm)


---

# O(log N) Complexity

## Why Halving Gives Logarithmic Time

Each iteration of binary search halves the search space. Starting from N elements:

| Iteration | Search space size |
|---|---|
| 0 | N |
| 1 | N/2 |
| 2 | N/4 |
| k | N/2ᵏ |

The loop stops when N/2ᵏ < 1, i.e., k > log₂(N). So at most **⌈log₂(N)⌉ + 1** iterations.

![Log N halving: N elements → N/2 → N/4 → ... → 1](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/log_n_halving.png)

---

## Practical Numbers

| N | log₂(N) | Max iterations |
|---|---|---|
| 10 | 3.3 | 4 |
| 1,000 | 10 | 11 |
| 1,000,000 | 20 | 21 |
| 1,000,000,000 | 30 | 31 |

A billion-element sorted array requires at most 31 comparisons.

---

## Space Complexity

Iterative binary search: O(1) — only `lo`, `hi`, `mid` variables.

---

## Further Reading

- [Logarithms in algorithm analysis](https://en.wikipedia.org/wiki/Time_complexity#Logarithmic_time)


---

# Boundary Index Trace

## Tracking lo and hi

The clearest way to understand binary search is to trace `lo`, `hi`, and `mid` at each iteration.

Example: `arr = [1, 3, 5, 7, 9, 11, 13]`, target = `7`

![lo/hi boundary pointer movement through sorted array for target=7](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/boundary_pointer_trace.png)

| Iteration | lo | hi | mid | arr[mid] | Action |
|---|---|---|---|---|---|
| 1 | 0 | 6 | 3 | 7 | Found! Return 3 |

Single iteration for this case. Harder example with `target = 11`:

| Iteration | lo | hi | mid | arr[mid] | Action |
|---|---|---|---|---|---|
| 1 | 0 | 6 | 3 | 7 | 7 < 11 → lo = 4 |
| 2 | 4 | 6 | 5 | 11 | Found! Return 5 |

---

## The Invariant

At every iteration: **if target exists, it is within arr[lo..hi]**.

This invariant is established initially (lo=0, hi=N-1 contains everything) and preserved by each step (we only eliminate halves that cannot contain the target).

---

## Off-by-One Pitfalls

- `mid + 1` and `mid - 1` (not `mid`): prevents infinite loops when lo=hi.
- `lo <= hi` (not `lo < hi`): handles single-element arrays.


---

# Iterative vs Recursive Binary Search

## Two Implementations, Same Logic

**Iterative:**
```
FUNCTION binary_search_iter(arr, target):
    lo, hi ← 0, len(arr)-1
    WHILE lo <= hi:
        mid ← lo + (hi - lo) // 2
        IF arr[mid] == target: RETURN mid
        IF arr[mid] < target:  lo ← mid + 1
        ELSE:                  hi ← mid - 1
    RETURN -1
```

**Recursive:**
```
FUNCTION binary_search_rec(arr, target, lo, hi):
    IF lo > hi: RETURN -1
    mid ← lo + (hi - lo) // 2
    IF arr[mid] == target: RETURN mid
    IF arr[mid] < target:
        RETURN binary_search_rec(arr, target, mid+1, hi)
    RETURN binary_search_rec(arr, target, lo, mid-1)
```

![Iterative call stack (flat) vs recursive call stack (O(log N) deep)](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/iterative_vs_recursive.png)

---

## Stack Space Comparison

| Aspect | Iterative | Recursive |
|---|---|---|
| Time | O(log N) | O(log N) |
| Space | O(1) | O(log N) call stack |
| Stack overflow risk | None | For N=10⁹: ~30 frames — safe in practice |
| Code clarity | Slightly more explicit | More elegant |

---

## When to Use Each

- **Iterative:** Production code, space-constrained environments.
- **Recursive:** Divide-and-conquer problems where recursion maps naturally to subproblem structure.


---

