
# Introduction to Object-Oriented Programming

## 1. Class vs. Object: The Architect's Vision
### What is it?
* **Class:** A user-defined data type. It is the **Blueprint**.
* **Object:** A specific **Instance**. It is the "Real Thing" built from the blueprint.

### Why do we need it?
In procedural programming, data is "loose" (name, age, grade are just floating variables). In OOP, we bundle them. This makes code **scalable**. If you need a new feature for 1,000 students, you update the *Class* once, and all 1,000 *Objects* get the update automatically.

### Real-World Example: Architecture
The **Class** is the blue blueprints for a 3-bedroom house. The **Object** is the actual house built on 123 Main Street. The blueprint says "Every house has a front door color," but the actual house at 123 Main Street has a **Red** door.



---

## 2. Constructors (`__init__`) and the `self` Mystery
### What is it?
* **`__init__`**: The "Birth" of an object. It runs the moment you call `ClassName()`.
* **`self`**: It represents the **unique ID** of the object currently being handled.

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
Encapsulation is the practice of **hiding the internal workings** of an object and only exposing what is necessary.

### Why do we need it?
1.  **Security:** Protects data from being changed in a way that breaks the system (e.g., setting a `bank_balance` to a negative number manually).
2.  **Simplicity:** You don't need to know how an internal combustion engine works to drive a car; you just need the steering wheel and pedals (the Methods).

### Real-World Example: A Smart Phone
You use the screen (Interface) to send a text. You don't manually manipulate the voltage in the CPU or the radio waves in the antenna. The complexity is **encapsulated** inside the phone's case.

### Working Example: The "Bank Account"
```python
# WHY: We want to ensure money can only be removed via a 'Withdraw' method.
class BankAccount:
    def __init__(self, balance):
        self.balance = balance # This is the data

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
my_account.withdraw(1200) # The 'Method' protects the 'Data' from becoming negative.
```

---

## 4. Class Variables vs. Instance Variables
### The Distinction:
* **Instance Variables (`self.x`):** Data that is unique to that specific object (e.g., your Passport Number).
* **Class Variables:** Data that is shared by **every single object** created from that class (e.g., the name of the Country on the passport).

### Why do we need it?
Class variables save memory. If 1,000,000 students belong to "Masai School," we shouldn't store that string 1,000,000 times. We store it **once** at the class level.