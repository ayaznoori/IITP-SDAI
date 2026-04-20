# Python: Basic OOP Assessment

## Section A: Multiple Choice Questions

**1. A "Class" is best described as:**
- A) A physical folder on your hard drive.
- B) A blueprint or skeleton that defines attributes and behaviors.
- C) A specific person like "Alice" or "Bob."
- D) A list of numbers.
**Correct Option: B**

---

**2. What is an "Object" in the context of OOP?**
- A) The code written inside the editor.
- B) An error message.
- C) A real, concrete instance created from a class blueprint.
- D) A type of loop.
**Correct Option: C**

---

**3. What does the `self` keyword represent?**
- A) The Python interpreter.
- B) The next object to be created.
- C) The current instance of the object calling the method.
- D) A private variable that cannot be changed.
**Correct Option: C**

---

**4. Which special method is used to initialize an object's data when it is first created?**
- A) `__start__`
- B) `__init__`
- C) `__str__`
- D) `__create__`
**Correct Option: B**

---

**5. If you have a class `Person` and you want to create an instance of it named `p1`, which syntax is correct?**
- A) `p1 = new Person()`
- B) `p1 = Person`
- C) `p1 = Person()`
- D) `create p1 as Person`
**Correct Option: C**

---

**6. What is "Encapsulation"?**
- A) Running code line-by-line.
- B) Bundling data (attributes) and methods (functions) together into a class.
- C) Deleting old variables to save memory.
- D) Importing external libraries.
**Correct Option: B**

---

**7. Which of these is an example of an "Attribute" for a `Car` class?**
- A) `drive()`
- B) `stop()`
- C) `color`
- D) `park()`
**Correct Option: C**

---

**8. How do you access an attribute called `name` on an object named `student1`?**
- A) `student1->name`
- B) `student1[name]`
- C) `student1.name`
- D) `student1(name)`
**Correct Option: C**

---

## Section B: Practical Subjective Task

### Task: Simple Student Management System

**Scenario:**
You need to organize student data. Instead of using separate lists for names and ages, you will create a `Student` blueprint to keep the data bundled together.

**Requirements:**
1. **Define a Class:** Create a class named `Student`.
2. **Constructor:** Create an `__init__` method that accepts `name` and `grade`. Assign these to `self.name` and `self.grade`.
3. **Display Method:** Create a method named `show_details` that prints: `"Student: [name], Grade: [grade]"`.
4. **Logic:**
   - Create two different objects (e.g., `s1` and `s2`) with different names and grades.
   - Call the `show_details()` method for both objects to verify the data is stored correctly.
5. **Bonus Logic:** Create a method named `is_passing()` that returns `True` if the grade is 40 or higher, and `False` otherwise.

### Submission Guidelines:
- **File Name:** `basic_oop.py`.
- **Code Structure:** Keep it simple. Define the class at the top and the object creation at the bottom.
- **Comments:** Label your **Class**, your **Attributes**, and your **Methods**.

---
