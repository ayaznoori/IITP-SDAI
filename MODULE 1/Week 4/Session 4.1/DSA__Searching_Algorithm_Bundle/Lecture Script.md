# Lecture Script — 10.13.2: Sorting as a Prerequisite

---

## Opening (2 min)

"Binary search needs O(log N) per query. But it needs sorted input. Sorting costs O(N log N). Is binary search actually faster?

The answer: depends on how many times you need to search. Today we work out the break-even and see when sorting pays off."

---

## The Single Query Case (3 min)

"Scenario: one unsorted array, one search query.

Option A — linear search: O(N). Done.
Option B — sort then binary search: O(N log N) + O(log N) = O(N log N).

*[Write on board: Single query: Linear O(N) vs Sort+BS O(N log N)]*

Option A wins by a factor of log N. For N = 10⁶: linear does 10⁶ operations; sort+BS does ≈ 20 million. Linear is 20× faster here."

---

## The Multiple Query Case (4 min)

"Now: same array, Q search queries.

Option A — Q linear searches: Q × O(N) = O(QN).
Option B — sort once, Q binary searches: O(N log N) + Q × O(log N).

Break-even: Q × N ≈ N log N + Q × log N
→ Q(N - log N) ≈ N log N
→ Q ≈ log N (for large N where N >> log N)

*[Write on board: break-even at Q ≈ log N queries]*

For N = 10⁶: log₂(10⁶) ≈ 20. After just 20 queries, binary search wins. After 1000 queries, it's 1000/20 = 50× faster."

---

## The Cost of Sorting (3 min)

"Sorting has to happen before ANY binary search can run. This is an upfront cost you pay once.

The right question: is this a one-time setup or repeated access pattern?

*[Display image: sort_then_search_cost.png]*

![Sort cost amortized over Q queries, showing break-even point](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/sort_then_search_cost.png)

Database indices work exactly like this. The index (sorted structure) is built once at write time and queried millions of times at read time. The sort cost is amortised to near-zero per query."

---

## When You Cannot Sort (2 min)

"Sometimes sorting is not allowed:

- **Order matters:** 'Find the 3rd occurrence by insertion order' — sorting destroys the original order.
- **Streaming data:** Elements arrive one by one; you cannot sort a stream.
- **Linked list:** Binary search requires O(1) random access by index.

In these cases, linear search is the correct choice — not a pessimisation."

---

## Closing (1 min)

"Sorting is a prerequisite for binary search. It costs O(N log N) once. That cost is justified when you have many queries, or when sorted order enables other benefits (range queries, rank queries, etc.).

Questions?"

*[Pause for questions.]*


---

# Lecture Script — 10.13.3: Classic Iterative Binary Search

---

## Opening (2 min)

"Binary search is one of those algorithms you'll implement dozens of times in your career. Get it right once; the pattern sticks.

Today: the canonical iterative implementation, the safe mid calculation, and the exact loop invariant that guarantees correctness."

---

## The Setup (3 min)

"Input: sorted array arr, target value. Output: index where target lives, or -1.

We maintain two pointers: lo (leftmost candidate index) and hi (rightmost candidate index).

```
lo ← 0
hi ← len(arr) - 1
```

Invariant: if target exists, it is at some index in [lo, hi].

Initially this invariant holds — the target is definitely in [0, N-1] if it exists anywhere."

---

## The Loop (5 min)

"Each iteration: compute mid, check arr[mid], eliminate half.

```
WHILE lo <= hi:
    mid ← lo + (hi - lo) // 2
    IF arr[mid] == target:
        RETURN mid
    IF arr[mid] < target:
        lo ← mid + 1
    ELSE:
        hi ← mid - 1
RETURN -1
```

*[Write on board each line as you explain it]*

**arr[mid] == target:** found it. Return immediately.

**arr[mid] < target:** target is to the right. Set lo = mid+1. Why mid+1 and not mid? We already checked mid — it's not the target. No need to revisit.

**arr[mid] > target:** target is to the left. Set hi = mid-1. Same reasoning.

**Loop exits (lo > hi):** empty search space. Target not present. Return -1."

---

## The Safe Mid Formula (3 min)

"Why `lo + (hi - lo) // 2` instead of `(lo + hi) // 2`?

*[Write on board: (lo + hi) can overflow!]*

In a 32-bit integer world: if lo = 2×10⁹ and hi = 2×10⁹, then lo+hi = 4×10⁹ — overflows.

`lo + (hi - lo) // 2`:
- hi - lo ≤ 2×10⁹ (fits)
- lo + (hi-lo)//2 ≤ hi (fits)

Safe. Always use this formula in practice."

---

## Full Trace (4 min)

"arr = [2, 5, 8, 12, 16, 23, 38, 56, 72, 91], target = 23

*[Display image: binary_search_trace.png]*

![Binary search trace on 10-element array, target=23, finding index 5 in 3 iterations](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/binary_search_trace.png)

| Iter | lo | hi | mid | arr[mid] | Action |
|---|---|---|---|---|---|
| 1 | 0 | 9 | 4 | 16 | 16 < 23 → lo=5 |
| 2 | 5 | 9 | 7 | 56 | 56 > 23 → hi=6 |
| 3 | 5 | 6 | 5 | 23 | Found! Return 5 |

3 iterations for N=10. log₂(10) ≈ 3.3 ✓"

---

## Closing (1 min)

"The pattern: lo/hi bracket the candidates, mid bisects, one half eliminated per step. The safe mid formula prevents overflow.

Questions?"

*[Pause for questions.]*


---

# Lecture Script — 10.13.4: O(log N) Complexity

---

## Opening (2 min)

"Binary search is O(log N). You've heard that. But can you derive it? And do you know what that means practically for billion-element datasets?

Today: the derivation from first principles, and the numbers that make log N extraordinary."

---

## The Derivation (5 min)

"Each iteration of binary search halves the search space. Start with N elements.

After iteration 1: N/2 elements remain.
After iteration 2: N/4 elements remain.
After iteration k: N/2ᵏ elements remain.

*[Write on board: N → N/2 → N/4 → ... → 1]*

The loop terminates when the remaining space is less than 1 element: N/2ᵏ < 1.

Solving: 2ᵏ > N → k > log₂(N).

So the loop runs at most **⌈log₂(N)⌉ + 1** times. This is O(log N).

*[Display image: log_n_halving.png]*

![Tree diagram: N splits to N/2, N/4, ..., 1. Depth = log N.](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/log_n_halving.png)"

---

## The Numbers (3 min)

"Here's what O(log N) looks like in practice:

| N | log₂(N) | Max comparisons |
|---|---|---|
| 100 | 7 | 8 |
| 100,000 | 17 | 18 |
| 1,000,000,000 | 30 | 31 |

A billion elements, 31 comparisons. Think about that.

A million-song music library, sorted by ID. Finding any song in 21 comparisons. That's what log N buys you."

---

## Best, Worst, Average Case (3 min)

"**Best case:** target is at mid on the first iteration. O(1). Rare.

**Worst case:** target not present or at the very last element examined. O(log N) iterations.

**Average case:** O(log N). Even in the average case, we don't save more than a constant factor versus worst case.

*[Write on board: Best O(1), Worst O(log N), Average O(log N)]*

Compare to linear search: Best O(1), Worst O(N), Average O(N/2) = O(N).

Binary search is O(log N) even on average. That's the real win."

---

## Space Complexity (2 min)

"Iterative binary search uses only `lo`, `hi`, `mid` — three integers regardless of N. Space complexity: **O(1)**.

This is why the iterative form is preferred in production — no call stack growth, no risk of overflow for large N."

---

## Closing (1 min)

"The derivation: halving gives log N iterations. The numbers: log N is so small that even for 10⁹ elements, 31 comparisons suffice.

Questions?"

*[Pause for questions.]*


---

# Lecture Script — 10.13.5: Boundary Index Visualization

---

## Opening (2 min)

"Binary search errors are almost always off-by-one errors. Wrong loop condition, wrong mid adjustment, wrong update direction.

The cure: trace lo and hi explicitly for every iteration. Visualise the shrinking window. Today we do exactly that."

---

## The Window Metaphor (2 min)

"Think of [lo, hi] as a window over the array. Each iteration, we move one window boundary inward, cutting the window in half.

*[Write on board: [lo ........ hi] → either [lo .. mid-1] or [mid+1 .. hi]]*

The window contracts until it's empty (lo > hi) or until we find the target."

---

## Trace 1 — Target Present (5 min)

"arr = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19], target = 11

*[Display image: boundary_pointer_trace.png]*

![lo/hi boundaries converging on target=11 through multiple iterations](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/boundary_pointer_trace.png)

| Iter | lo | hi | mid | arr[mid] | Comparison | Update |
|---|---|---|---|---|---|---|
| 1 | 0 | 9 | 4 | 9 | 9 < 11 | lo ← 5 |
| 2 | 5 | 9 | 7 | 15 | 15 > 11 | hi ← 6 |
| 3 | 5 | 6 | 5 | 11 | 11 == 11 | Return 5 |

Window: [0..9] → [5..9] → [5..6] → found."

---

## Trace 2 — Target Absent (4 min)

"Same array, target = 6.

| Iter | lo | hi | mid | arr[mid] | Comparison | Update |
|---|---|---|---|---|---|---|
| 1 | 0 | 9 | 4 | 9 | 9 > 6 | hi ← 3 |
| 2 | 0 | 3 | 1 | 3 | 3 < 6 | lo ← 2 |
| 3 | 2 | 3 | 2 | 5 | 5 < 6 | lo ← 3 |
| 4 | 3 | 3 | 3 | 7 | 7 > 6 | hi ← 2 |
| 5 | 3 | 2 | — | — | lo > hi | Return -1 |

Window: [0..9] → [0..3] → [2..3] → [3..3] → empty."

---

## Common Off-by-One Mistakes (3 min)

"**Mistake 1:** `WHILE lo < hi` instead of `lo <= hi`.
Effect: when lo == hi (one element left), the loop exits without checking that element. Misses targets at the last surviving index.

**Mistake 2:** `hi = mid` instead of `hi = mid - 1`.
Effect: if arr[mid] > target, we set hi = mid. Next iteration: same mid calculated, infinite loop when lo == hi == mid.

**Mistake 3:** `lo = mid` instead of `lo = mid + 1`.
Similar infinite loop."

---

## Closing (1 min)

"Trace lo, hi, mid explicitly when debugging. The window shrinks deterministically — any infinite loop or missed element is an off-by-one in how boundaries are updated.

Questions?"

*[Pause for questions.]*


---

# Lecture Script — 10.13.6: Iterative vs Recursive Binary Search

---

## Opening (2 min)

"Binary search can be written iteratively or recursively. Both are correct. But they differ in space usage, and that difference matters at scale.

Today we implement both, compare them side by side, and decide which to use when."

---

## Side-by-Side Implementation (6 min)

"*[Display image: iterative_vs_recursive.png]*

![Iterative (flat call stack, O(1) space) vs recursive (log N deep call stack)](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/dsa/10.13/iterative_vs_recursive.png)

**Iterative:**
```
FUNCTION bs_iter(arr, target):
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

The logic is identical. The difference is where state lives: in loop variables (iterative) vs call stack frames (recursive)."

---

## Stack Space Analysis (4 min)

"Each recursive call adds a stack frame. How deep does the recursion go?

At most log₂(N) calls — the same as the number of loop iterations.

For N = 10⁹: log₂(10⁹) ≈ 30. That's 30 stack frames — each maybe 50 bytes → 1.5 KB total. Completely safe.

*[Write on board: Recursive BS stack depth = log N — safe in practice]*

Contrast with naive recursive problems: factorial(N) uses O(N) stack — 10⁶ frames for N=10⁶. Binary search recursion is O(log N) — fundamentally different.

**Space summary:**
| | Time | Space |
|---|---|---|
| Iterative | O(log N) | O(1) |
| Recursive | O(log N) | O(log N) |

Recursive is fine for all practical N. But iterative is the professional default."

---

## Tail Recursion (2 min)

"Notice the recursive binary search has a special property: the recursive call is the LAST operation in each branch. This is **tail recursion**.

Tail-recursive functions can be automatically converted to iteration by the compiler/interpreter (tail call optimisation, TCO).

In Python: TCO is NOT performed. Each recursive call still uses a stack frame.
In some functional languages (Haskell, Scala): TCO converts this to iteration automatically."

---

## When to Use Recursive (2 min)

"Recursive binary search shines when the problem itself is naturally recursive:
- Divide-and-conquer algorithms where binary search is a subroutine
- Problems where the recursive structure clarifies the code

For standalone binary search implementation: use iterative."

---

## Closing (1 min)

"Same algorithm, two styles. Iterative = O(1) space, preferred in production. Recursive = O(log N) stack, safe in practice, elegant for recursive contexts.

Questions?"

*[Pause for questions.]*


---

