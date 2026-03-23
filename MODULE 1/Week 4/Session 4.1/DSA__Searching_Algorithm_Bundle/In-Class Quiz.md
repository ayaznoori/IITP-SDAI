# In-class Quiz — 10.13.2: Sorting as a Prerequisite

### Question 1
For N = 1,000,000 and Q queries, at approximately what value of Q does sorting once and using binary search become faster than repeated linear search?

A. Q = 1
B. Q = 20
C. Q = 1000
D. Q = N

**Correct Answer: B**

---

### Question 2
You receive a stream of integers one by one and need to answer search queries between arrivals. Why can't you use binary search?

A. Streaming data is always unsorted
B. You cannot sort data that is still arriving
C. Binary search requires O(N²) preprocessing
D. Search queries would change the stream

**Correct Answer: B**

---

### Question 3
What is the total cost of sorting an array once and then answering Q binary search queries?

A. O(N log N + Q)
B. O(N + Q log N)
C. O(N log N + Q log N)
D. O(QN log N)

**Correct Answer: C**


---

# In-class Quiz — 10.13.3: Classic Binary Search

### Question 1
In binary search on arr = [1, 3, 5, 7, 9], target = 7, what is the value of `mid` in the first iteration?

A. 0
B. 2
C. 3
D. 4

**Correct Answer: B**

---

### Question 2
Why is `lo + (hi - lo) // 2` preferred over `(lo + hi) // 2` for computing mid?

A. It runs faster
B. It avoids integer overflow when lo and hi are large
C. It always picks an even index
D. It works for negative indices

**Correct Answer: B**

---

### Question 3
After binary search exits with `lo > hi`, what does this indicate?

A. The target was found at index lo
B. The array is not sorted
C. The target is not present in the array
D. An overflow occurred

**Correct Answer: C**


---

# In-class Quiz — 10.13.4: O(log N) Complexity

### Question 1
How many iterations does binary search need in the worst case for an array of 1,024 elements?

A. 5
B. 10
C. 11
D. 1024

**Correct Answer: C**

---

### Question 2
What is the space complexity of iterative binary search?

A. O(N)
B. O(log N)
C. O(1)
D. O(N log N)

**Correct Answer: C**

---

### Question 3
A sorted database of 1 billion records. At most how many comparisons does binary search need to find any record?

A. 1,000,000
B. 1000
C. 31
D. 10

**Correct Answer: C**


---

# In-class Quiz — 10.13.5: Boundary Index Visualization

### Question 1
After the first iteration of binary search on arr = [2, 4, 6, 8, 10, 12, 14] with target = 10 (lo=0, hi=6, mid=3), what are the new values of lo and hi?

A. lo=0, hi=2
B. lo=4, hi=6
C. lo=3, hi=6
D. lo=0, hi=3

**Correct Answer: B**

---

### Question 2
Binary search exits when:

A. mid == 0
B. lo > hi
C. arr[mid] > target
D. hi == len(arr) - 1

**Correct Answer: B**

---

### Question 3
If the loop condition is `lo < hi` instead of `lo <= hi`, which case will binary search incorrectly miss?

A. Target at index 0
B. Target at the midpoint
C. Target at the only remaining element (lo == hi)
D. Target not present in the array

**Correct Answer: C**


---

# In-class Quiz — 10.13.6: Iterative vs Recursive

### Question 1
What is the space complexity of recursive binary search due to call stack usage?

A. O(1)
B. O(log N)
C. O(N)
D. O(N log N)

**Correct Answer: B**

---

### Question 2
For N = 1,000,000,000 (1 billion), how many recursive call stack frames does recursive binary search use at most?

A. 1,000,000,000
B. 1,000
C. 30
D. 1

**Correct Answer: C**

---

### Question 3
What property of recursive binary search makes it convertible to iteration by some compilers/interpreters?

A. It uses only integer arithmetic
B. It is tail-recursive (the recursive call is the last operation)
C. It never exceeds O(log N) depth
D. It does not use global variables

**Correct Answer: B**


---

