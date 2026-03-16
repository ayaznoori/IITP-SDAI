# Implement String Data Types

## Valid string?
A) name = "Alice"
B) name = 'Alice'
C) name = `Alice`
D) Both A and B

<details><summary>Answer</summary>
D - Both single and double quotes work
</details>

## What prints?
```python
print("ha" * 3)
```
A) ha3
B) hahaha
C) ha ha ha

<details><summary>Answer</summary>
B - String repetition
</details>

## What's wrong?
```python
age = 25
msg = "Age: " + age
```
A) Nothing
B) Can't concatenate str and int
C) Wrong quotes

<details><summary>Answer</summary>
B - Need str(age) or use f-string
</details>

## What's len("Hello")?
A) 4
B) 5
C) 6

<details><summary>Answer</summary>
B - 5 characters
</details>

## What prints?
```python
name = "alice"
print(name.upper())
```
A) alice
B) ALICE
C) Alice

<details><summary>Answer</summary>
B - ALICE (all uppercase)
</details>


---

# Format Output with F-strings

## Valid f-string?
A) f"Hello {name}"
B) "Hello {name}"f
C) "f Hello {name}"
D) f{"Hello name"}

<details><summary>Answer</summary>
A - f goes before quotes
</details>

## What prints?
```python
x = 10
print(f"{x * 2}")
```
A) x * 2
B) 20
C) {20}
D) {x * 2}

<details><summary>Answer</summary>
B - Expressions are evaluated
</details>

## Format to 2 decimals?
```python
pi = 3.14159
```
A) f"{pi:.2f}"
B) f"{pi:2f}"
C) f"{pi.2f}"
D) f"{round(pi)}"

<details><summary>Answer</summary>
A - :.2f format specifier
</details>

## What's wrong?
```python
name = "Bob"
print("Hello {name}")
```
A) Missing f
B) Wrong brackets
C) Nothing
D) name should be in parentheses instead of braces

<details><summary>Answer</summary>
A - Need f before string
</details>

## Can you do math in {}?
A) Yes
B) No
C) Only addition
D) Only with integers

<details><summary>Answer</summary>
A - Any expression works
</details>


---

