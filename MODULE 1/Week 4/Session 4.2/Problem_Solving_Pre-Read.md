# Problem Solving using Sorting and Searching

This pre-read will help you revise previously learned concepts and prepare for applying them in real DSA problems.

In this session, you will **not learn new algorithms from scratch**.  
Instead, you will focus on **how to use known techniques to solve problems efficiently**.

---

# 1 What is Problem Solving in DSA?

Problem solving in DSA means:

* Understanding the problem carefully
* Choosing the correct approach
* Writing a correct solution
* Improving the solution using efficient techniques

A correct answer is important, but an **efficient answer** is even more important in DSA.

---

# 2 Concepts You Already Know

Before this session, you have already learned:

* Insertion Sort
* Binary Search
* Recursive Binary Search
* Why Binary Search needs sorted data
* Time complexity of searching and sorting
* Python’s `sorted()` and `list.sort()`

This session will focus on **applying these concepts together**.

---

# 3 Key Problem Solving Questions

Before writing code, always ask:

1. Is the array already sorted?
2. Can I directly use Binary Search?
3. Do I need to sort first?
4. Should I use Insertion Sort or built-in sorting?
5. Is recursion useful here?
6. What is the most efficient approach?

---

# 4 Why Sorting and Searching Are Often Connected

Searching becomes faster when data is sorted.

Example:

```text
Unsorted Array: [7, 2, 9, 1, 5]
Target: 5
```

You cannot directly apply Binary Search here.

You must first sort the array:

```text
[1, 2, 5, 7, 9]
```

Now Binary Search becomes possible.

This is a common DSA problem-solving pattern:

### Sort first → Search efficiently later

--
# 5 Efficiency Thinking

Suppose you want to search for only **one target** in an unsorted array.

A simple scan may be enough.

But if you need to search **many times**, sorting once and then applying Binary Search repeatedly may be a better idea.

This is how problem solving works:

- not just “Can I solve it?”  
- but also “Can I solve it efficiently?”

---

# 6 What You Should Be Ready For

In class, you will solve problems like:

* Sort an unsorted array and then search for a target
* Decide whether Binary Search is possible
* Compare iterative and recursive Binary Search
* Choose between Insertion Sort and Python built-in sorting
* Compare brute-force and optimized thinking

---

# 7 Think Before Class

Reflect on these:

1. Why is Binary Search not possible on unsorted data?
2. When is sorting first worth the extra cost?
3. When would recursion be a good choice?

---

By the end of this session, you should be able to:

* Choose the right approach for a problem
* Combine sorting and searching effectively
* Use recursion where appropriate
* Justify your solution using time complexity
