# Lecture Script: Hands-on DSA Problem Solving & Logic Building
**Duration:** 110 minutes | **Tool:** One Compiler | **Language:** Python

---

## Session Opening (5 min)

**[Script:]** "You have the tools. Today we build with them. No new syntax marathon — this is a problem-solving gym. Think first, code second, verify always."

**Problem hook:** Given a list of transaction amounts, find how many are above average. That needs sum, length, loop, and conditionals — all in one flow.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "When your code fails, is it usually syntax or logic?" Steer toward logic — today's focus.

**[Script:]** "Industry work is rarely one isolated concept. You combine data structures, loops, and decisions. Interviews test exactly this integration."

- **Real-world use:** Report generation, anomaly detection, small automation scripts
- **Pain if misunderstood:** Coding before planning, wrong algorithm order, missing edge cases

---

## What Is the Concept?

### Problem Decomposition

**Definition:** Breaking a problem into inputs, ordered steps, expected outputs, and edge cases.

**Mental model:** Recipe card — ingredients, steps, final dish, what if oven is off?

### Integrating Prior Python Foundations

| Building block | Role in problems |
|----------------|------------------|
| `list`, `dict`, `str` | Hold and organize data |
| `if/elif/else` | Branch on conditions |
| `for`/`while` | Iterate |
| Search/sort | Find or order efficiently |

### Search and Sort in Context

- Need sorted data for binary search? Sort first — but know the cost.
- Need only maximum? Loop once — do not sort unnecessarily.

🎯 **Instructor Note:** Whiteboard "sort vs single pass" decision tree.

### Nested Conditionals and Loops

**Common mistake:** Off-by-one in inner loop start index (`j = i + 1` for pairs).

**Common mistake:** Infinite while when forget to update counter.

---

## How Do We Apply It?

### LO 1: Decompose a problem into inputs, logic steps, and expected outputs

**Problem:** Return the most frequent number in a list.

**Translate logic (board first):**
- Input: list of integers
- Steps: count each → track max count → return that key
- Output: integer
- Edge: tie? return any or first found (state rule)

**Write code:**
```python
nums = [1, 2, 2, 3, 2]
freq = {}
for n in nums:
    freq[n] = freq.get(n, 0) + 1
best = max(freq, key=freq.get)
print(best)
```

**Predict before running:** `2`

**Explain result:** Dictionary counts occurrences; `max` with `key` picks highest frequency.

---

### LO 2: Solve structured problems using lists, strings, dictionaries, conditionals, and loops

**Problem:** Count vowels in a string.

**Write code:**
```python
text = "masai"
vowels = "aeiou"
count = 0
for ch in text:
    if ch in vowels:
        count += 1
print(count)
```

**Predict:** `3`

---

### LO 3: Apply sorting and searching techniques in the correct order

**Problem:** Check if `target` exists using binary search approach (list is sorted).

**Write code:**
```python
data = [2, 5, 8, 12, 16]
target = 8
found = False
low, high = 0, len(data) - 1
while low <= high:
    mid = (low + high) // 2
    if data[mid] == target:
        found = True
        break
    elif data[mid] < target:
        low = mid + 1
    else:
        high = mid - 1
print(found)
```

**Predict:** `True`

🎯 **Instructor Note:** Contrast with unsorted list — must use linear search instead.

---

### LO 4: Use nested conditionals and loops for multi-step DSA exercises

**Problem:** Print all pairs in a list that sum to `target`.

**Write code:**
```python
nums = [2, 7, 3, 4]
target = 9
for i in range(len(nums)):
    for j in range(i + 1, len(nums)):
        if nums[i] + nums[j] == target:
            print(nums[i], nums[j])
```

**Predict:** `2 7` and `3`? — actually `2 7` only for target 9. `3+4=7` no. So `2 7`.

---

### LO 5: Solve guided DSA problems in One Compiler with step-by-step mentor support

**Guided lab (25 min):** Rotate through 3 problems (easy → medium):
1. Second largest in a list (sort or two-pass)
2. Two sum pair count
3. Is list palindrome? (string or list indices)

**Process enforced:** Paper plan → code → test edge case → mentor review.

---

## Live Demo Block (15 min)

Full walkthrough: "Above average count" — decompose on board, code live, break with empty list edge case, fix.

---

## Recap (10 min)

🎯 **Instructor Note:** "Name the four decomposition questions." Chorus response.

---

## Lecture Summary

- **Decomposition** turns scary problems into clear steps
- **Lists, dicts, strings, conditionals, and loops** work together in real solutions
- **Search and sort** must be applied in the right order for efficiency
- **Nested loops** handle pair and multi-element comparisons
- **Guided practice** with mentor checkpoints builds exam and interview stamina
- **Practical value:** This is how engineers approach unknown problems every day

**[Script:]** "Next sessions we leave One Compiler and set up professional local tools — VS Code, modules, and virtual environments. Today's thinking skills travel with you."
