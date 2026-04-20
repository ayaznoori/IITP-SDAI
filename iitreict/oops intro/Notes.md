# Introduction to Object-Oriented Programming

## 1. Class vs. Object: The Architect's Vision
### What is it?
* **Class:** A user-defined data type. It is the **Blueprint**.
* **Object:** A specific **Instance**. It is the "Real Thing" built from the blueprint.

### Why do we need it?
In procedural programming, data is "loose" (name, age, grade are just floating variables). In OOP, we bundle them. This makes code **scalable**. When you add or change **methods** on the class, every instance of that class can use the updated behavior. If you change what **data** each new object stores (for example by editing `__init__`), objects you already created earlier are **not** magically rebuilt—you only affect **new** objects from that point on.

### Real-World Example: Architecture
The **Class** is the blueprints for a 3-bedroom house. The **Object** is the actual house built on 123 Main Street. The blueprint says "Every house has a front door color," but the actual house at 123 Main Street has a **Red** door.



---

## 2. Constructors (`__init__`) and the `self` Mystery
### What is it?
* **`__init__`**: The "Birth" of an object. It runs the moment you call `ClassName()`.
* **`self`**: The **reference to the current instance**—the particular object whose method is running. Python passes it automatically as the first argument of normal instance methods. (If you need a numeric identity for debugging, use `id(some_object)`; that is different from `self`.)

### Why do we need it?
Without `self`, Python wouldn't know which data belongs to whom. If you have two `Dog` objects, `self` ensures that when you call `bark()`, the correct dog's name is printed.

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

## 3. Encapsulation: The "Black Box" Principle
### What is it?
Encapsulation is the practice of **hiding the internal workings** of an object and only exposing what is necessary. It also means **bundling** data (attributes) and the methods that work on that data into one class.

### Why do we need it?
1.  **Security:** Protects data from being changed in a way that breaks the system (e.g., setting a `bank_balance` to a negative number manually).
2.  **Simplicity:** You don't need to know how an internal combustion engine works to drive a car; you just need the steering wheel and pedals (the Methods).

### Real-World Example: A Smart Phone
You use the screen (Interface) to send a text. You don't manually manipulate the voltage in the CPU or the radio waves in the antenna. The complexity is **encapsulated** inside the phone's case.

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

Here the **behavior** (when a withdrawal is allowed) is encapsulated in methods. Stronger "hiding" (e.g. naming conventions or accessors) is a topic you can add as you advance.

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

## 5. Destructor (`__del__`)
### What is it?
`__del__` is a special method Python may call when an object is about to be destroyed (e.g. no more references to it, or at shutdown). You rarely need it for beginners; `__init__` is what you use day to day.

### Rules of thumb
* **`__init__`:** Initializes the object. It does **not** return a value (it must not use `return` with a value).
* **`__del__`:** Takes only `self` (plus what Python passes internally for the hook). Do not rely on `__del__` for critical cleanup; prefer explicit `close()` methods or context managers for files and resources.

---

## 6. Class Methods and Static Methods
### Class method (`@classmethod`)
* First parameter is **`cls`** (the class itself), not `self`.
* Use it when the logic belongs to the **class** (factory methods, counting instances, reading/writing class variables) rather than one instance.

### Static method (`@staticmethod`)
* No `self` and no `cls`. It is a plain function namespaced inside the class for organization (e.g. a small helper that does not need instance or class state).

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

## 7. Memory, Identity, and `id()`
Every object lives at an address in memory. **`id(obj)`** returns an integer identifier that is unique for that object **while it exists** (CPython often uses the memory address). **Immutable** values that compare equal might share one stored object; **mutable** objects (lists, dicts, your own class instances) are usually distinct even if their contents look similar. Knowing `id()` helps when debugging "are these the same object or just equal values?"

---

## 8. Dynamic Attributes: `getattr`, `setattr`, `delattr`
Sometimes the attribute name is only known at **runtime** (e.g. from user input). Built-ins:

* **`getattr(obj, "name", default)`** — read attribute; optional default if missing.
* **`setattr(obj, "name", value)`** — assign `obj.name = value`.
* **`delattr(obj, "name")`** — delete the attribute if it exists.

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
* OOP models entities with **classes** (blueprints) and **objects** (instances).
* **Encapsulation** bundles data and methods; **`self`** is the current instance in instance methods.
* **`__init__`** initializes new objects; **`__del__`** exists but is not the main tool for cleanup in most programs.
* **Instance variables** are per-object; **class variables** are shared—avoid **shadowing** by using the class name when you mean the shared field.
* **`@classmethod`** / **`@staticmethod`** organize behavior that is not tied to one instance the same way.
* **`id()`**, **getattr** / **setattr** / **delattr** help with identity and dynamic attribute access.
