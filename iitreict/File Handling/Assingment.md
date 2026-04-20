# Python: File & Exception Handling Assessment

## Section A: Multiple Choice Questions

**1. Which mode should be used to add new data to the end of an existing text file without deleting its current content?**
- A) `r`
- B) `w`
- C) `a`
- D) `r+`
**Correct Option: C**

---

**2. What happens if you try to open a non-existent file in `r` (read) mode?**
- A) A new file is automatically created.
- B) The program crashes with a `FileNotFoundError`.
- C) The file opens as an empty object.
- D) Python ignores the command and moves to the next line.
**Correct Option: B**

---

**3. Which method is used to move the file cursor to a specific position?**
- A) `tell()`
- B) `move()`
- C) `locate()`
- D) `seek()`
**Correct Option: D**

---

**4. Which block in an exception handling construct is guaranteed to execute regardless of whether an error occurred or not?**
- A) `try`
- B) `except`
- C) `finally`
- D) `else`
**Correct Option: C**

---

**5. What is the main difference between a Syntax Error and a Runtime Error?**
- A) Syntax errors are logical; Runtime errors are grammatical.
- B) Syntax errors are caught before execution; Runtime errors occur during execution.
- C) Syntax errors can be handled with `try-except`; Runtime errors cannot.
- D) There is no difference; both crash the program the same way.
**Correct Option: B**

---

**6. Which statement is used to manually trigger an exception for business logic validation (e.g., checking if a user's age is below 18)?**
- A) `throw`
- B) `error`
- C) `raise`
- D) `except`
**Correct Option: C**

---

When using the syntax for line in f:, how does Python process the file?

A) It reads the entire file into a single long string at once.

B) It reads the file line-by-line, allowing for string-level operations like find and replace.

C) It skips every other line to save time.

D) It automatically converts the text into a list of integers.

Correct Option: B

---

**8. When using multiple `except` blocks, which exception would catch an error caused by trying to access a dictionary key that doesn't exist?**
- A) `ValueError`
- B) `KeyError`
- C) `IndexError`
- D) `NameError`
**Correct Option: B**

---

## Section B: Practical Subjective Task

### Task: Robust Log File Processor

**Scenario:**
You are building a system that reads a list of numbers from a file named `data.txt`, calculates their reciprocal ($1/x$), and writes the results to a new file named `results.txt`. You must ensure the program does not crash even if the input file is missing or contains invalid data (like zeros or text).

**Requirements:**
1. **File Input:** Open `data.txt` in read mode. Use exception handling to catch a `FileNotFoundError` and print a user-friendly message if it doesn't exist.
2. **Processing:**
   - Read the file line by line.
   - Convert each line to an integer. Handle `ValueError` if a line contains a string or non-numeric character.
   - Calculate $1$ divided by that number. Handle `ZeroDivisionError` if a line contains $0$.
3. **File Output:** Append the valid results to `results.txt`. 
4. **Cleanup:** Use a `finally` block to ensure that all open file objects are properly closed, regardless of any errors encountered during processing.
5. **Custom Validation:** Use the `raise` keyword to trigger an exception if a number in the file is negative, as your system only supports positive values.

### Submission Guidelines:
- **File Name:** Submit your solution as `file_handler_task.py`.
- **Code Structure:** Ensure your code is modular. Use a clear `try-except-else-finally` structure.
- **Comments:** Include comments explaining which specific exception is being handled in each `except` block.
- **Verification:** Create a dummy `data.txt` with the following content to test your code:
  ```text
  10
  0
  abc
  -5
  20
  ```