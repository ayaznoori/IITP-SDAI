# Lecture Script: Searching Algorithms — Linear Search & Binary Search
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Linear Search — Implementation and Tracing | 30 min |
| 3 | Why Binary Search Requires Sorted Data | 15 min |
| 4 | Implementing and Tracing Binary Search | 30 min |
| 5 | Comparing Linear vs Binary Search — Big O Intuition | 20 min |
| 6 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience knows Python basics — variables, lists, loops, conditionals, functions. This is likely their first formal exposure to analyzing an algorithm's efficiency, not just its correctness. The hook should make "how" you search matter just as much as "whether" you find something. Open with a size-based scenario. Wait after the opening question.

**[Script:]**

"You already know how to check if something is in a list — you have used the `in` keyword, or written a loop that checks each item. That works. Here is the question today is really about: does it work well, at any size?

Imagine you have a phone book with a hundred names. Looking for one name by checking every entry from the start is mildly annoying but fast enough that you would not think twice about it. Now imagine a database with one hundred million records — user IDs, product codes, transaction records. If your only strategy is 'check every single one starting from the beginning,' and the item you want happens to be near the end, or does not exist at all, you check all one hundred million before you know the answer. At some point, the difference between a search that takes a fraction of a second and one that takes minutes is not about faster hardware — it is about a fundamentally different algorithm.

Today we look at two searching algorithms that represent two different philosophies. Linear search: check everything, one at a time, in order — simple, always works, but slow at scale. Binary search: exploit the fact that your data is sorted to eliminate half of the remaining possibilities with every single check — dramatically faster, but it only works if a specific condition is met first.

By the end of today, you will be able to implement both, trace through exactly what each one does step by step on a real example, and explain — not just recite, but actually reason through — why one of these algorithms scales so much better than the other as data grows. This reasoning about efficiency, not just correctness, is the foundation for almost everything else in algorithm design going forward."

---

## Block 2 — Linear Search: Implementation and Tracing

### 2A — What Linear Search Does

**[Script:]**

"Linear search is the most straightforward way to find something in a collection: start at the beginning, look at each item one at a time, and stop the moment you find a match — or, if you reach the end without finding it, conclude it is not there.

The word 'linear' describes the pattern of movement — straight through the list, one position after another, in order, with no skipping and no assumptions about how the data is arranged."

> 🎯 **Instructor Note:** Draw this data set on the board and keep it visible for the entire block.

```
numbers = [34, 7, 23, 32, 5, 62, 78, 45, 91, 12]
          index:  0   1   2   3  4   5   6   7   8   9
```

---

### 2B — Implementing Linear Search with a Loop

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I search this list for the number 23, roughly how many items do you expect the search to check before finding it?" Answer: three — indices 0, 1, then 2, where 23 lives. Let learners count it out loud on the board using the data set above before running any code.

**Demo 1 — Linear search with a loop (whiteboard-friendly)**

```python
def linear_search(items, target):
    for index in range(len(items)):
        if items[index] == target:
            return index
    return -1

numbers = [34, 7, 23, 32, 5, 62, 78, 45, 91, 12]
result = linear_search(numbers, 23)
print(result)
```

**[Script:]**

"`range(len(items))` produces every valid index in the list, in order, from 0 to the last index. The loop checks `items[index] == target` at each position. The moment it matches, `return index` exits the function immediately — we do not keep checking after finding what we wanted.

If the loop finishes entirely without ever matching, execution reaches `return -1` after the loop — a common convention meaning 'not found', since -1 is never a valid index into a list.

Running this on our list searching for 23 returns 2 — matching exactly what we predicted by counting through the list by hand."

> 🎯 **Instructor Note:** Ask: "Why does the function return immediately with `return index` inside the loop, instead of finishing the loop first and returning afterward?" Answer: once a match is found, there is no reason to keep checking the remaining items — returning immediately avoids wasted work. This is a small detail, but it is the first hint of the efficiency thinking this entire session builds toward.

---

### 2C — Linear Search Using Membership Checks

**[Script:]**

"Python's built-in `in` operator performs exactly this same linear search under the hood, when used on a list. `if target in items` checks membership by scanning the list from the start, exactly like our loop, and returns `True` or `False`."

**Demo 2 — Membership check (whiteboard-friendly)**

```python
numbers = [34, 7, 23, 32, 5, 62, 78, 45, 91, 12]

print(23 in numbers)   # True
print(99 in numbers)   # False
```

**[Script:]**

"This is convenient when you only need to know whether something exists, not where it is located. Our `linear_search` function from Demo 1 is more useful when you need the actual position — for instance, to then modify or remove that specific item. Both approaches do fundamentally the same work: checking items one at a time, in order, until a match is found or the list is exhausted."

> 🎯 **Instructor Note:** Ask: "If you only need a yes/no answer about whether an item exists, is there ever a reason to prefer writing the loop yourself instead of using `in`?" Answer: generally no for plain existence checks — `in` is clearer and does the same work. The loop version becomes useful specifically when you need the index, or when you want to customize what counts as a match, such as case-insensitive text comparison.

---

### 2D — Tracing Linear Search Step by Step

**[Script:]**

"Tracing means walking through exactly what the algorithm does, one step at a time, writing down the state at each point — this is a skill you will use throughout algorithms work, not just today. Let us trace a search that does not find anything, since that is the case that reveals the algorithm's full behavior."

> 🎯 **Instructor Note:** Build this trace live on the board, one row at a time, asking the room to predict each row before you write it, rather than presenting the finished table all at once.

**Demo 3 — Tracing a linear search for a missing value (whiteboard-friendly)**

```
numbers = [34, 7, 23, 32, 5, 62, 78, 45, 91, 12]
Searching for: 99

Step  Index  items[index]  Match?
1     0      34            No
2     1      7             No
3     2      23            No
4     3      32             No
5     4      5             No
6     5      62            No
7     6      78            No
8     7      45            No
9     8      91            No
10    9      12            No

Loop ends — no match found — return -1
```

**[Script:]**

"Ten steps, checking every single item, because the value we searched for is not in the list at all. This is the worst case for linear search — it cannot know the item is absent until it has ruled out every position. Compare this to searching for 34, which sits at index 0 — that trace would be a single step. The number of steps linear search takes depends entirely on where the target is, or whether it exists at all."

**Recap of Block 2 before moving on:**

- Linear search checks items one at a time, in order, from the start, stopping immediately on a match
- A loop-based implementation returns the index on a match, or -1 after exhausting the list without one
- Python's `in` operator performs the same linear scan for a simple existence check
- Tracing an execution means recording the algorithm's state at each step; a missing value forces linear search through its full worst case — checking every item

---

## Block 3 — Why Binary Search Requires Sorted Data

### 3A — The Core Idea: Eliminating Half the Possibilities

**[Script:]**

"Binary search takes a completely different approach. Instead of checking items one at a time from the start, it starts in the middle of the data, and uses a single comparison to eliminate half of the remaining possibilities immediately.

Think about how you would look up a word in a printed dictionary. You do not start at the letter A and flip through every page. You open somewhere in the middle, see which half of the alphabet you landed in, and narrow your search to that half — then repeat, narrowing further, until you land on the word. That is binary search."

> 🎯 **Instructor Note:** Ask: "In a dictionary with roughly a thousand pages, if you open to the middle and see you need a word earlier in the alphabet, how many pages have you just ruled out in that one action?" Answer: about five hundred — half the entire dictionary, eliminated with a single comparison. This is the entire intuition for why binary search is fast, before any code appears.

---

### 3B — Why This Requires Sorted Data

**[Script:]**

"Here is the critical requirement, and it is not optional: binary search only works on data that is already sorted. The dictionary example only works because the words are in alphabetical order — that is precisely what lets you know which half to eliminate. If the dictionary's words were in random order, glancing at the middle page would tell you nothing useful about where your word might be.

The same logic applies to a sorted list of numbers. If you check the middle value and it is smaller than your target, you know — with certainty — that your target, if it exists, must be somewhere to the right, because everything to the left is guaranteed to be even smaller. That guarantee only holds because the data is sorted. Without it, the middle value tells you nothing about where to look next."

> 🎯 **Instructor Note:** This is the most important conceptual point in the entire lecture. Write it on the board and keep it visible through Block 4.

```
Binary search's core requirement:

Sorted data → comparing against the middle value tells you 
              which half the target must be in (if it exists at all)

Unsorted data → the middle value gives no information about 
                where to look next; binary search cannot work
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I tried to run binary search on the unsorted list [34, 7, 23, 32, 5], checking the middle value first, could I safely rule out one entire half based on that comparison?" Answer: no — because the list is not sorted, a value smaller than the middle could be anywhere in the list, not necessarily confined to one side. The entire elimination strategy breaks down without sorted order.

**[Script:]**

"This means, in practice, if your data is not already sorted, you must sort it first before binary search becomes usable — and sorting itself takes time. Binary search is not a free upgrade over linear search in every situation; it is a trade you make deliberately, and it pays off specifically when you will be searching the same sorted data many times, so the one-time cost of sorting is worth it across many searches."

> 🎯 **Instructor Note:** Ask: "If you only need to search a list one single time and then discard it, is binary search still worth using?" Answer: not necessarily — sorting the list first costs time on its own, so for a single one-off search, linear search might actually be more efficient overall. Binary search earns its advantage when the sorted data will be searched repeatedly.

**Recap of Block 3 before moving on:**

- Binary search starts in the middle of the data and eliminates half of the remaining possibilities with each comparison
- This elimination strategy is only valid because sorted order guarantees which half the target must fall into
- On unsorted data, comparing against the middle value provides no reliable information about where to search next
- Sorting has its own cost; binary search's advantage is strongest when the same sorted data will be searched repeatedly, not just once

---

## Block 4 — Implementing and Tracing Binary Search

### 4A — Setting Up: Low, High, and Middle

**[Script:]**

"Binary search tracks the boundaries of the region still being searched using two pointers: `low`, the leftmost index still under consideration, and `high`, the rightmost index still under consideration. At each step, it calculates the middle index between them, checks the value there, and narrows the boundaries based on that comparison."

> 🎯 **Instructor Note:** Draw this sorted data set on the board and keep it visible for the rest of the block.

```
sorted_numbers = [3, 8, 15, 22, 34, 41, 56, 67, 78, 90]
                  index: 0   1   2   3   4   5   6   7   8  9
```

---

### 4B — Implementing Iterative Binary Search

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before showing code, ask: "Searching for 67 in this list — where would binary search check first, and what would it learn from that check?" Guide through: middle index is (0+9)//2 = 4, value there is 34. Since 67 > 34, the target must be in the right half, indices 5 through 9. Let the room reason through this before the code appears.

**Demo 4 — Iterative binary search (whiteboard-friendly)**

```python
def binary_search(items, target):
    low = 0
    high = len(items) - 1

    while low <= high:
        mid = (low + high) // 2
        if items[mid] == target:
            return mid
        elif items[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return -1

sorted_numbers = [3, 8, 15, 22, 34, 41, 56, 67, 78, 90]
result = binary_search(sorted_numbers, 67)
print(result)
```

**[Script:]**

"`low` and `high` start at the boundaries of the entire list. The `while low <= high` loop continues as long as there is still a valid region to search — once `low` passes `high`, there is nothing left to check, and the target is not present.

`mid = (low + high) // 2` finds the middle index of the current region, using integer division so we always get a valid index. If `items[mid]` matches the target, we are done — return immediately. If `items[mid]` is less than the target, the target must be to the right, so we move `low` to `mid + 1`, discarding the entire left half including `mid` itself, since we already know it is not the target. If `items[mid]` is greater than the target, we move `high` to `mid - 1` instead, discarding the right half.

Notice this is 'iterative' — it uses a `while` loop that repeats, updating `low` and `high` each pass, rather than a function calling itself. This word appears in the objective for a reason: binary search can also be written recursively, but the iterative version, using a loop, is what we are building today."

> 🎯 **Instructor Note:** Ask: "Why do we set `low = mid + 1` instead of `low = mid` when moving right? What would go wrong if we used `mid` instead of `mid + 1`?" Answer: `items[mid]` was already checked and confirmed not to be the target — including it again in the new search region could cause the loop to check the same index repeatedly and never terminate. Moving past it with `+1` or `-1` guarantees the search region actually shrinks every single iteration.

---

### 4C — Tracing Binary Search Step by Step

**[Script:]**

"Let us trace the full execution of the search for 67 from Demo 4, tracking `low`, `high`, and `mid` at every step — this makes the halving behavior completely visible."

> 🎯 **Instructor Note:** Build this trace live on the board, asking the room to predict the next row each time before revealing it — same technique as the linear search trace, so the contrast in step count is felt directly, not just told.

**Demo 5 — Tracing binary search (whiteboard-friendly)**

```
sorted_numbers = [3, 8, 15, 22, 34, 41, 56, 67, 78, 90]
Searching for: 67

Step  low  high  mid  items[mid]  Comparison
1     0    9     4    34          34 < 67 → search right half
2     5    9     7    67          67 == 67 → FOUND at index 7

Found in 2 steps.
```

**[Script:]**

"Two steps. Compare this to a linear search for the same value — 67 sits at index 7, so linear search would take eight steps, checking every item from index 0 through index 7 before finding it. Binary search found the same value in a fraction of the work, by eliminating half the list with the very first comparison.

Let us also trace a search for a value that does not exist, to see how binary search recognizes 'not found' — this is the direct counterpart to the linear search worst-case trace from Block 2."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If we search this same list for 50, which does not appear anywhere in it, roughly how many steps do you expect before the algorithm concludes it is missing — compared to the ten steps linear search needed for a similar missing-value case?" Let learners estimate before revealing the trace.

**Demo 6 — Tracing a binary search for a missing value (whiteboard-friendly)**

```
sorted_numbers = [3, 8, 15, 22, 34, 41, 56, 67, 78, 90]
Searching for: 50

Step  low  high  mid  items[mid]  Comparison
1     0    9     4    34          34 < 50 → search right half
2     5    9     7    67          67 > 50 → search left half
3     5    6     5    41          41 < 50 → search right half
4     6    6     6    56          56 > 50 → search left half

Now low = 6, high = 5. Since low > high, loop condition fails.
Return -1 — not found, in 4 steps.
```

**[Script:]**

"Four steps to conclusively determine a value is missing from a list of ten items — compared to ten steps for linear search to determine the same thing on this size of list. As the list grows larger, this gap does not just stay the same — it grows dramatically wider, which is exactly what Block 5 examines directly."

**Recap of Block 4 before moving on:**

- Binary search tracks `low` and `high` boundaries, calculating `mid` at each step and comparing the value there against the target
- A match at `mid` returns immediately; otherwise, the search region is halved by moving `low` or `high` past `mid`, never including it again
- The iterative version uses a `while` loop to repeat this halving until `low` exceeds `high`, at which point the target is confirmed absent
- Tracing shows binary search resolving both found and not-found cases in dramatically fewer steps than linear search on the same data

---

## Block 5 — Comparing Linear vs Binary Search: Big O Intuition

### 5A — Counting Worst-Case Steps as Data Grows

**[Script:]**

"We have now directly observed the difference on a list of ten items — linear search took up to ten steps, binary search took at most four. The real value of this comparison is not in this one specific case; it is in understanding how each algorithm's worst case scales as the data grows much larger. This is where Big O notation comes in — a way of describing that scaling pattern, not a specific step count for one specific list."

> 🎯 **Instructor Note:** Build this table on the board, growing the list size by a factor of ten each row, and have the room predict the binary search column before revealing it — the doubling pattern is the entire intuition to land here.

```
List size (n)    Linear search worst case    Binary search worst case
10               10 steps                    ~4 steps
100              100 steps                   ~7 steps
1,000            1,000 steps                 ~10 steps
1,000,000        1,000,000 steps             ~20 steps
```

**[Script:]**

"Linear search's worst case grows in direct proportion to the size of the data — ten times more data means ten times more steps, in the worst case. This is what we call O(n) — the number of steps scales linearly with the input size, n.

Binary search's worst case barely grows at all, even as the data grows enormously. Going from ten items to a million items — a hundred-thousand-fold increase in data — only takes the worst case from about four steps to about twenty. This is what we call O(log n) — the number of steps scales with the logarithm of the input size, because each step eliminates half of what remains."

> 🎯 **Instructor Note:** Ask directly: "Why does doubling the list size only add roughly one extra step to binary search's worst case, rather than doubling the step count too?" Answer: each additional step in binary search doubles the amount of data it can handle, since each step's job is to cut the remaining region in half. Going from handling 500 items to handling 1000 items only requires one more halving step, not five hundred more comparisons.

---

### 5B — Intuition for O(n) vs O(log n)

**[Script:]**

"Here is the intuitive way to hold onto this distinction, without needing to memorize a formal mathematical definition today. O(n) means: if the data doubles, the work roughly doubles too — a straight-line relationship between size and effort. O(log n) means: if the data doubles, the work only grows by about one additional step — because each step is doing something fundamentally more powerful than checking one item; it is cutting the entire remaining problem in half."

> 🎯 **Instructor Note:** Write this compact comparison on the board as the final visual takeaway of the block.

```
Linear search  — O(n):      work grows in direct proportion to data size
Binary search  — O(log n):  work grows by about one step each time 
                             the data size doubles

This is why binary search remains fast even on enormous datasets, 
while linear search's cost keeps climbing right alongside data growth.
```

**[Script:]**

"This does not mean linear search is a bad algorithm to be avoided entirely. It is simpler to implement, it works on any data regardless of order, and on small lists the difference is genuinely irrelevant to real-world performance. The choice matters specifically when data is large, sorted, and searched repeatedly — exactly the conditions under which binary search's advantage compounds into something that actually matters in practice."

> 🎯 **Instructor Note:** Close with a synthesis question: "Given everything today, if you had an unsorted list of one hundred items that you would only ever search a single time, which algorithm would you actually choose, and why?" Answer: linear search — the list is small enough that the difference is negligible, it is not sorted, and sorting purely to enable one single binary search would cost more than the search itself saves. This question checks that learners are applying the tradeoff, not just reciting "binary search is always better."

**Recap of Block 5 before moving on:**

- Linear search's worst case is O(n) — the number of steps grows in direct proportion to the size of the data
- Binary search's worst case is O(log n) — the number of steps grows by roughly one additional step each time the data size doubles
- This gap is invisible on small data and becomes dramatic on large data, since each binary search step eliminates half of what remains
- Linear search remains the right choice for small, unsorted, or one-off searches, where binary search's setup cost is not worth paying

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What does linear search's loop actually do at each step? What single condition must be true before binary search can work at all, and why? What happens to `low` and `high` after each comparison in binary search? What is the intuitive difference between O(n) and O(log n)?"

**Linear Search**

- Linear search checks items one at a time, in order, from the start, returning the index on a match or -1 after exhausting the list
- Python's `in` operator performs the same underlying scan for a simple existence check
- Tracing a missing-value search shows linear search's full worst case — checking every single item

**Why Binary Search Requires Sorted Data**

- Binary search eliminates half of the remaining possibilities with each comparison against the middle value
- This elimination is only valid because sorted order guarantees which half the target must fall into if it exists
- Sorting has its own cost, so binary search pays off most clearly when the same sorted data will be searched repeatedly

**Implementing and Tracing Binary Search**

- `low` and `high` track the boundaries of the region still under consideration; `mid` is the midpoint checked at each step
- A match returns immediately; otherwise the region halves by moving `low` or `high` past `mid`, never including it again
- The loop continues while `low <= high`; once `low` exceeds `high`, the target is confirmed absent
- Tracing shows binary search resolving searches in dramatically fewer steps than linear search on the same sorted data

**Comparing Linear vs Binary Search — Big O Intuition**

- Linear search is O(n) — worst-case steps grow in direct proportion to data size
- Binary search is O(log n) — worst-case steps grow by roughly one step each time the data size doubles
- The performance gap is negligible on small data and dramatic on large data
- Linear search remains the right practical choice for small, unsorted, or one-off searches

**Why All of This Matters Together**

- Linear and binary search are the first clear demonstration that two algorithms can solve the exact same problem correctly while behaving completely differently as data grows — and that the right choice depends on real constraints: is the data sorted, how large is it, and how many times will it be searched; this way of reasoning about tradeoffs, not just "does the code work," is the foundation every later algorithm and data structure in this course will build on

---

*End of script.*