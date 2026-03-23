# Lecture Notes — 10.13.2: Sorting as a Prerequisite

## Why Sorting Is Required

Binary search eliminates halves based on comparison. Without sorted order, `arr[mid] < target` tells you nothing about which half contains the target.

## Cost Analysis

| Strategy | Cost |
|---|---|
| Linear search, Q queries | O(Q × N) |
| Sort once + binary search | O(N log N) + O(Q × log N) |

**Sort pays off when Q ≥ log N** (approximately).

![Sort-then-search cost: break-even curve and amortised per-query cost](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/sort_then_search_cost.png)

## When Not to Sort

- Single query → linear search is O(N), sorting is O(N log N) → linear wins
- Streaming data → cannot sort a stream
- Order-dependent queries → sorting destroys original order
- Linked lists → binary search needs O(1) random access

## Key Insight

The sort cost O(N log N) is paid once and amortised across all Q queries. With many queries, the per-query cost drops to O(log N).

## Practical Example

Database index: sorted at write time, queried millions of times. The sort cost per query approaches zero.


---

# Lecture Notes — 10.13.3: Classic Iterative Binary Search

## Algorithm

```
FUNCTION binary_search(arr, target):
    lo ← 0
    hi ← len(arr) - 1
    WHILE lo <= hi:
        mid ← lo + (hi - lo) // 2
        IF arr[mid] == target: RETURN mid
        IF arr[mid] < target:  lo ← mid + 1
        ELSE:                  hi ← mid - 1
    RETURN -1
```

![Binary search trace: step-by-step pointer movements on a 10-element array](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/binary_search_trace.png)

## Loop Invariant

If target exists in arr, it is located at some index in [lo, hi].

## Key Points

- **Safe mid formula:** `lo + (hi - lo) // 2` prevents integer overflow
- **`mid + 1` / `mid - 1`:** prevents infinite loop when lo == hi (mid already checked)
- **`lo <= hi`:** handles single-element arrays correctly

## Complexity

- Time: O(log N)
- Space: O(1)

## Common Mistakes

| Mistake | Effect |
|---|---|
| `(lo + hi) // 2` | Integer overflow for large lo, hi |
| `hi = mid` when arr[mid] > target | Infinite loop |
| `lo < hi` loop condition | Misses target at last element |


---

# Lecture Notes — 10.13.4: O(log N) Complexity

## Derivation

Each iteration halves the search space:
- After k iterations: N / 2ᵏ elements remain
- Loop terminates when N / 2ᵏ < 1 → k > log₂(N)
- Maximum iterations: ⌈log₂(N)⌉ + 1 = **O(log N)**

![O(log N) halving pyramid and N vs log N comparison](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/log_n_halving.png)

## Practical Numbers

| N | Max comparisons |
|---|---|
| 1,000 | 11 |
| 1,000,000 | 21 |
| 1,000,000,000 | 31 |

## Complexity Summary

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Worst | O(log N) | O(1) |
| Average | O(log N) | O(1) |

## Why O(1) Space

Only three variables: `lo`, `hi`, `mid`. Independent of N.

## Comparison with Linear Search

| Algorithm | Worst case | Sorted required? |
|---|---|---|
| Linear search | O(N) | No |
| Binary search | O(log N) | Yes |


---

# Lecture Notes — 10.13.5: Boundary Index Visualization

## The Window

`[lo, hi]` is the active search window. Each iteration shrinks it by ~half.

## Pointer Update Rules

| Condition | Update |
|---|---|
| arr[mid] == target | Return mid |
| arr[mid] < target | lo ← mid + 1 |
| arr[mid] > target | hi ← mid - 1 |

![Boundary pointer trace: lo/hi window shrinking across iterations](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/boundary_pointer_trace.png)

## Loop Invariant

If target exists, it is within arr[lo..hi] at all times.

## Termination

When lo > hi: empty window, target not present → return -1.

## Off-by-One Pitfalls

| Bug | Effect |
|---|---|
| `lo < hi` instead of `lo <= hi` | Misses single-element window |
| `hi = mid` instead of `hi = mid - 1` | Infinite loop when lo == hi == mid |
| `lo = mid` instead of `lo = mid + 1` | Infinite loop |

## Trace Template

| Iter | lo | hi | mid | arr[mid] | Action |
|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | ... |


---

# Lecture Notes — 10.13.6: Iterative vs Recursive Binary Search

## Both Implementations

**Iterative:**
```
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
FUNCTION bs_rec(arr, target, lo, hi):
    IF lo > hi: RETURN -1
    mid ← lo + (hi - lo) // 2
    IF arr[mid] == target: RETURN mid
    IF arr[mid] < target:
        RETURN bs_rec(arr, target, mid+1, hi)
    RETURN bs_rec(arr, target, lo, mid-1)
```

## Comparison

| Aspect | Iterative | Recursive |
|---|---|---|
| Time | O(log N) | O(log N) |
| Space | O(1) | O(log N) call stack |
| Stack overflow risk | None | Minimal (log N ≈ 30 for N=10⁹) |
| Preferred for | Production code | Recursive subproblems |

![Iterative O(1) flat loop vs recursive O(log N) call stack frames](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/iterative_vs_recursive.png)

## Tail Recursion

Binary search recursion is tail-recursive. Languages with TCO (Tail Call Optimisation) convert it to iteration automatically. Python does NOT perform TCO.


---

