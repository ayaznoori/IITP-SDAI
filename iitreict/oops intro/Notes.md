# Introduction to Object-Oriented Programming

## 0. The Four Pillars of OOP (Theory)
Object-oriented programming is often summarized around four ideas:
* **Data abstraction** — Work with meaningful types and interfaces, not raw scattered data.
* **Encapsulation** — Keep data and the methods that use it **together** in one class (see Section 3).
* **Inheritance** — Derive new classes from existing ones (covered in later sessions).
* **Polymorphism** — The same interface (e.g. a method name) can behave differently for different types (covered later).

This set gives the **theoretical foundation** for why OOP is structured the way it is.

---

## 1. Class vs. Object: The Architect's Vision
### What is it?
* **Class:** A user-defined data type. It is the **Blueprint**.
* **Object:** A specific **Instance**. It is the "Real Thing" built from the blueprint.

### Why do we need it?
In procedural programming, data is "loose" (name, age, grade are just floating variables). In OOP, we bundle them. This makes code **scalable**. When you add or change **methods** on the class, every instance of that class can use the updated behavior. If you change what **data** each new object stores (for example by editing `__init__`), objects you already created earlier are **not** magically rebuilt—you only affect **new** objects from that point on.

### Real-World Example: Architecture
The **Class** is the blueprints for a 3-bedroom house. The **Object** is the actual house built on 123 Main Street. The blueprint says "Every house has a front door color," but the actual house at 123 Main Street has a **Red** door.



---

## 2. Constructors (`__init__`) and the `self` Convention
### What is it?
* **`__init__`**: The "Birth" of an object. It runs the moment you call `ClassName()`.
* **`self`**: The **first parameter of an instance method**, bound to the **object instance** whose method was called. In practice it behaves like a **reference** to that object (in CPython you can think of it as tied to the object's identity in memory). The **`id()`** built-in returns a separate numeric identifier for an object—do **not** confuse `id(obj)` with `self`.
* **`self` is not a Python keyword.** It is only a **naming convention**; you *could* name the first parameter something else, but everyone uses `self` so code stays readable.

### Why do we need it?
Without `self`, Python wouldn't know which data belongs to whom. If you have two `Dog` objects, `self` ensures that when you call `bark()`, the correct dog's name is printed.

### Default parameters in `__init__` (and any function)
Parameters **without** default values must come **before** parameters **with** defaults. Otherwise Python raises a **`SyntaxError`**.

```python
# Valid
def __init__(self, name, grade=0):
    ...

# Invalid — SyntaxError
def __init__(self, grade=0, name):
    ...
```

### Working Example: The "Game Character"
```python
# WHY: To create a reusable template for characters in a game.
class Hero:
    def __init__(self, name, health):
        self.name = name     # Instance Variable: Unique to this hero
        self.health = health # Instance Variable

    def take_damage(self, amount):
        self.health -= amount
        print(f"{self.name} now has {self.health} HP remaining.")

# REAL WORLD USAGE
warrior = Hero("Aragon", 100)
mage = Hero("Gandalf", 60)

warrior.take_damage(20) # Only Aragon's health drops, Gandalf stays at 60.
```

---

## 3. Encapsulation: Bundling Data and Methods
### What is it?
**Encapsulation** means **bundling** data (attributes) and the methods (functions) that operate on that data **inside one class**—"mixing all those things together" into a single unit.

### How this relates to "hiding" and abstraction
When you use a class, you often interact through a small **interface** (the methods you call). The **internal** details can stay private or at least organized inside the class. That **black-box** / information-hiding style is a **benefit** that builds on encapsulation and overlaps with **data abstraction**; it is **not** a separate replacement for the definition above. **Hiding alone** (without bundling) would be incomplete as a definition of encapsulation.

### Why do we need it?
1. **Organization:** State and behavior live in one place instead of loose global variables.
2. **Safety and rules:** Methods can enforce valid changes (e.g. no negative balance after withdrawal).
3. **Simplicity for callers:** Callers use simple methods without managing every internal detail.

### Real-World Example: A Smart Phone
You use the screen (Interface) to send a text. You don't manually manipulate the voltage in the CPU or the radio waves in the antenna. The complexity is organized **inside** the phone's case.

### Working Example: The "Bank Account"
```python
# WHY: We want withdrawal rules (no overdrawing) enforced in one place: methods.
class BankAccount:
    def __init__(self, balance):
        self.balance = balance  # The data; still a normal attribute in this intro example

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            print(f"Deposited {amount}. New Balance: {self.balance}")

    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            print(f"Withdrew {amount}. Remaining: {self.balance}")
        else:
            print("Action Denied: Insufficient Funds.")

# USAGE
my_account = BankAccount(1000)
my_account.withdraw(1200)  # The method enforces the rule; balance does not go negative.
```

Here data and behavior are **bundled** in one class; the **withdraw** method is the controlled **interface** for changing balance.

---

## 4. Class Variables vs. Instance Variables
### The Distinction:
* **Instance Variables (`self.x`):** Data that is unique to that specific object (e.g., your Passport Number).
* **Class Variables:** Data that is shared by **every single object** created from that class (e.g., the name of the Country on the passport). They are defined **on the class body**, not inside `__init__`, unless you intentionally set them there for all instances.

### Why do we need it?
Class variables save memory. If 1,000,000 students belong to "Masai School," we shouldn't store that string 1,000,000 times. We store it **once** at the class level.

### Pitfall: shadowing
Read or update a class variable **through the class name** when you mean the shared value (e.g. `Student.school_name`). If you assign `student1.school_name = "Other"` on an instance, Python may create an **instance** attribute with that name instead of changing the class variable for everyone—easy to confuse. Prefer `ClassName.attribute` for the shared field unless you mean something per-object.

---

## 5. Destructor (`__del__`) and Object Lifetime
### What is it?
`__del__` is a special method Python **may** call when an object is about to be destroyed—typically when there are **no more references** to it, or during interpreter shutdown.

### Manual vs automatic cleanup
* You can **remove a reference** with **`del obj`** (if nothing else points to that object, it may become eligible for destruction).
* You do **not** call `__del__` yourself like a normal method; Python invokes it as part of teardown when appropriate.
* Compared to **`__init__`**, which runs in a predictable way for every new object, relying on **`__del__`** for important cleanup (closing files, releasing locks) is **fragile**. Prefer explicit **`close()`** methods or **`with`** / context managers for resources.

### Rules of thumb
* **`__init__`:** Initializes the object. It does **not** return a value (it must not use `return` with a value).
* **`__del__`:** Defined like `def __del__(self): ...`. Do not depend on it for critical cleanup.

---

## 6. `id()` and Object Identity
Along with **state** (attributes) and **behavior** (methods), every object has **identity**.
* **`id(x)`** returns an integer that is unique for the **lifetime** of that object (in CPython it is often the memory address).
* If **`a = b`** and both refer to the **same** mutable object, **`id(a) == id(b)`**.
* Two **separate** lists with the same contents are still **two objects**: **`id`** will differ.

Use **`is`** (same object) vs **`==`** (same value) accordingly.

---

## 7. Class Methods and Static Methods
### Class method (`@classmethod`)
* First parameter is **`cls`** (the class itself), not `self`.
* Use it when the logic belongs to the **class** (factory methods, counting instances, reading/writing class variables) rather than one instance.

### Static method (`@staticmethod`)
* No `self` and no `cls`. It is a plain function namespaced inside the class for organization (e.g. a small helper that does not need instance or class state). It can avoid passing unused `self` and keeps helpers near the class.

```python
class Person:
    species = "Homo sapiens"

    def __init__(self, name):
        self.name = name

    @classmethod
    def get_species(cls):
        return cls.species

    @staticmethod
    def is_adult(age):
        return age >= 18
```

---

## 8. Dynamic Attributes: `getattr`, `setattr`, `hasattr`, `delattr`
Sometimes the attribute name is only known at **runtime** (e.g. from user input). Built-ins:

* **`getattr(obj, "name", default)`** — read attribute; optional default if missing.
* **`setattr(obj, "name", value)`** — assign `obj.name = value`.
* **`hasattr(obj, "name")`** — `True` if the object has that attribute.
* **`delattr(obj, "name")`** — delete the attribute on **that object** (first argument is the instance or the class object, depending on what you are modifying; for a **class-level** attribute you pass the **class** as the first argument, e.g. `delattr(MyClass, "shared")`).

Use with care: typos in names become runtime errors instead of editor-checked code.

---

## 9. Example: A Simple `Complex` Number Class
Model a number with real and imaginary parts; add two complex numbers and show the result.

```python
class Complex:
    def __init__(self, real, imag):
        self.real = real
        self.imag = imag

    def add(self, other):
        return Complex(self.real + other.real, self.imag + other.imag)

    def display(self):
        sign = "+" if self.imag >= 0 else "-"
        print(f"{self.real} {sign} {abs(self.imag)}i")

a = Complex(1, 2)
b = Complex(3, 4)
c = a.add(b)
c.display()  # 4 + 6i
```

---

## Summary of Key Points
* OOP theory often lists **abstraction, encapsulation, inheritance, polymorphism**; this session focuses on encapsulation (bundling) and related ideas.
* **Encapsulation** = **bundling** attributes and methods in a class; a clean **interface** and hiding detail are important **benefits**.
* **`self`** is the conventional **first parameter** for instance methods, bound to the **instance**; it is **not** a reserved keyword. **`id()`** reports identity.
* **`__init__`** initializes objects; follow **default-parameter ordering** rules. **`__del__`** exists but is not your main cleanup tool.
* **Instance variables** are per-object; **class variables** are shared—watch **shadowing**; use **`ClassName.attr`** when you mean the shared field.
* **`@classmethod`** / **`@staticmethod`** organize class-level or helper logic.
* **`getattr` / `setattr` / `hasattr` / `delattr`** support **dynamic** attribute access and deletion (on an instance or on the class for class attributes).
