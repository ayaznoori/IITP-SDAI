## Post-class Quiz: Creating and Initializing Dictionaries

---

### 
Which of the following correctly creates a dictionary?

A) `d = ['name': 'Alice']`
B) `d = {'name': 'Alice'}`
C) `d = ('name': 'Alice')`
D) `d = {name: Alice}`

**Correct Answer: B**

*Explanation: Dictionaries use curly braces `{}` with key-value pairs separated by colons. Option A uses list brackets, C uses tuple parentheses, and D is missing quotes around the strings.*

---

### 
What is the output?

```python
d = {'a': 1, 'b': 2, 'a': 3}
print(d)
```

A) `{'a': 1, 'b': 2, 'a': 3}`
B) `{'a': 1, 'b': 2}`
C) `{'a': 3, 'b': 2}`
D) Error — duplicate keys

**Correct Answer: C**

*Explanation: Dictionary keys must be unique. When the same key 'a' is used twice, the last value (3) overwrites the first (1). No error is raised — it's silently overwritten.*

---

### 
Which CANNOT be used as a dictionary key?

A) `'hello'` (string)
B) `(1, 2)` (tuple)
C) `[1, 2]` (list)
D) `42` (integer)

**Correct Answer: C**

*Explanation: Dictionary keys must be hashable (immutable). Lists are mutable, so they cannot be used as keys. Strings, tuples, and integers are all immutable and work fine as keys.*

---

### 
What does `dict.fromkeys(['x', 'y', 'z'], 0)` return?

A) `{'x': 0, 'y': 0, 'z': 0}`
B) `{0: ['x', 'y', 'z']}`
C) `[('x', 0), ('y', 0), ('z', 0)]`
D) Error

**Correct Answer: A**

*Explanation: `fromkeys()` creates a dictionary where all the specified keys share the same value. It takes a list of keys and a default value, producing a dict with each key mapped to that value.*

---

### 
What is the output?

```python
d = {x: x**2 for x in range(4)}
print(d)
```

A) `[0, 1, 4, 9]`
B) `{0: 0, 1: 1, 2: 4, 3: 9}`
C) `{0, 1, 4, 9}`
D) `{(0,0), (1,1), (2,4), (3,9)}`

**Correct Answer: B**

*Explanation: This is a dictionary comprehension. For each x in range(4) (0, 1, 2, 3), it creates a key-value pair where the key is x and the value is x squared.*

---


---

## Post-class Quiz: Accessing Dictionary Values

---

### 
What happens when you access a non-existent key with `dict[key]`?

A) Returns `None`
B) Returns `0`
C) Raises `KeyError`
D) Returns an empty string

**Correct Answer: C**

*Explanation: Using square bracket notation to access a key that doesn't exist raises a `KeyError` exception, which crashes your program unless caught with try/except.*

---

### 
What is the output?

```python
d = {'a': 1, 'b': 2}
print(d.get('c', 99))
```

A) `None`
B) `99`
C) `KeyError`
D) `0`

**Correct Answer: B**

*Explanation: `.get('c', 99)` looks for key 'c'. Since it doesn't exist, it returns the default value 99. Without the second argument, it would return None.*

---

### 
What does `'hello' in {'hello': 'world'}` return?

A) `True` — checks if 'hello' is a key
B) `False` — checks if 'hello' is a value
C) `True` — checks if 'hello' is a value
D) Error

**Correct Answer: A**

*Explanation: The `in` operator for dictionaries checks **keys**, not values. Since 'hello' is a key in the dictionary, it returns True.*

---

### 
What is the output?

```python
data = {'x': {'y': {'z': 42}}}
result = data.get('x', {}).get('y', {}).get('z', 0)
print(result)
```

A) `0`
B) `42`
C) `None`
D) `KeyError`

**Correct Answer: B**

*Explanation: Each `.get()` safely accesses the next level. 'x' exists → returns the nested dict. 'y' exists → returns the inner dict. 'z' exists → returns 42. If any key were missing, the `{}` default would prevent errors.*

---

### 
What does `.get()` return when the key exists?

A) Always returns the default value
B) Returns the actual value associated with the key
C) Returns `None`
D) Returns both the key and value

**Correct Answer: B**

*Explanation: When the key exists, `.get()` returns the actual value — exactly like `dict[key]`. The default value is only used when the key is NOT found.*

---


---

## Post-class Quiz: Adding and Modifying Dictionary Entries

---

### 
What is the output?

```python
d = {'a': 1}
d['b'] = 2
d['a'] = 10
print(d)
```

A) `{'a': 1, 'b': 2}`
B) `{'a': 10, 'b': 2}`
C) `{'a': 1, 'a': 10, 'b': 2}`
D) Error

**Correct Answer: B**

*Explanation: `d['b'] = 2` adds a new key. `d['a'] = 10` overwrites the existing value for key 'a'. The result is `{'a': 10, 'b': 2}`. Dictionaries cannot have duplicate keys.*

---

### 
What does `setdefault()` do when the key already exists?

A) Raises an error
B) Overwrites the existing value
C) Does nothing — keeps the existing value
D) Deletes the key

**Correct Answer: C**

*Explanation: `setdefault()` only sets the value if the key doesn't already exist. If the key is present, it leaves the existing value unchanged and returns it.*

---

### 
What is the output?

```python
d = {'x': 5}
d.update({'x': 10, 'y': 20})
print(d)
```

A) `{'x': 5, 'y': 20}`
B) `{'x': 10, 'y': 20}`
C) `{'x': 5, 'x': 10, 'y': 20}`
D) Error — cannot update existing keys

**Correct Answer: B**

*Explanation: `update()` overwrites existing keys and adds new ones. Key 'x' is updated from 5 to 10, and key 'y' is added with value 20.*

---

### 
What pattern correctly counts occurrences?

A) `counts[item] = counts[item] + 1`
B) `counts[item] = counts.get(item, 0) + 1`
C) `counts.add(item)`
D) `counts.append(item)`

**Correct Answer: B**

*Explanation: `.get(item, 0)` returns 0 for new keys (avoiding KeyError), then adds 1. Option A would crash with KeyError for new keys. Options C and D are list/set methods, not dict methods.*

---

### 
What is the output?

```python
d1 = {'a': 1, 'b': 2}
d2 = {'b': 3, 'c': 4}
merged = {**d1, **d2}
print(merged)
```

A) `{'a': 1, 'b': 2, 'c': 4}`
B) `{'a': 1, 'b': 3, 'c': 4}`
C) `{'a': 1, 'b': 2, 'b': 3, 'c': 4}`
D) Error

**Correct Answer: B**

*Explanation: `{**d1, **d2}` merges both dicts. When keys overlap, the later dict wins. 'b' appears in both — d2's value (3) overrides d1's value (2).*

---


---

## Post-class Quiz: Removing Dictionary Entries

---

### 
What is the output?

```python
d = {'a': 1, 'b': 2, 'c': 3}
val = d.pop('b')
print(val, d)
```

A) `2 {'a': 1, 'c': 3}`
B) `'b' {'a': 1, 'c': 3}`
C) `('b', 2) {'a': 1, 'c': 3}`
D) `None {'a': 1, 'c': 3}`

**Correct Answer: A**

*Explanation: `pop('b')` removes key 'b' and returns its value (2). The dictionary now has only 'a' and 'c'. `pop()` returns the value, not the key or a tuple.*

---

### 
What happens with `d.pop('x')` when 'x' is not in `d`?

A) Returns None
B) Returns False
C) Raises KeyError
D) Does nothing

**Correct Answer: C**

*Explanation: Without a default argument, `pop()` raises KeyError when the key is missing. To avoid this, use `d.pop('x', None)` or `d.pop('x', default_value)`.*

---

### 
What does `popitem()` remove?

A) The first item added
B) A random item
C) The last item added
D) The item with the smallest key

**Correct Answer: C**

*Explanation: In Python 3.7+, dictionaries maintain insertion order. `popitem()` removes and returns the last inserted key-value pair (LIFO — Last In, First Out).*

---

### 
What is the difference between `del d` and `d.clear()`?

A) No difference
B) `del d` deletes the variable; `clear()` empties the dict
C) `del d` empties the dict; `clear()` deletes the variable
D) Both delete the variable

**Correct Answer: B**

*Explanation: `del d` removes the variable completely — accessing `d` afterward causes NameError. `d.clear()` empties the dictionary but the variable `d` still exists as an empty dict `{}`.*

---

### 
What is the safe way to remove multiple keys?

A) `for k in keys: del d[k]`
B) `for k in keys: d.pop(k, None)`
C) `for k in d: d.pop(k)`
D) `d.remove(keys)`

**Correct Answer: B**

*Explanation: `pop(k, None)` skips missing keys safely. Option A crashes if any key is missing. Option C modifies the dict while iterating (RuntimeError). Option D doesn't exist — dicts have no `remove()` method.*

---


---

## Post-class Quiz: Checking for Key Existence

---

### 
What does `'hello' in {'hello': 'world'}` return?

A) True — 'hello' is a key
B) False — 'hello' is a value
C) True — 'hello' is a value
D) Error

**Correct Answer: A**

*Explanation: The `in` operator checks dictionary **keys**. 'hello' is a key in this dictionary, so it returns True.*

---

### 
How do you check if a VALUE exists in a dictionary?

A) `value in dict`
B) `value in dict.keys()`
C) `value in dict.values()`
D) `dict.has_value(value)`

**Correct Answer: C**

*Explanation: `dict.values()` returns a view of all values. Using `in` on this view checks if the value exists. `in dict` only checks keys. `has_value()` doesn't exist.*

---

### 
What is the output?

```python
d = {1: 'one', 2: 'two', 3: 'three'}
print(1 in d, 'one' in d)
```

A) `True True`
B) `True False`
C) `False True`
D) `False False`

**Correct Answer: B**

*Explanation: `1 in d` checks if 1 is a key — True. `'one' in d` checks if 'one' is a key — False (it's a value, not a key).*

---

### 
Which is the correct way to safely access a possibly missing key?

A) `if key in d: val = d[key]`
B) `val = d.get(key, default)`
C) Both A and B are correct
D) Neither is correct

**Correct Answer: C**

*Explanation: Both approaches are valid. `in` is better when you need different logic paths. `.get()` is more concise when you just need a fallback value.*

---

### 
What is the time complexity of `key in dict`?

A) O(n)
B) O(log n)
C) O(1) average
D) O(n²)

**Correct Answer: C**

*Explanation: Dictionaries use hash tables, so key lookup is O(1) on average — constant time regardless of dictionary size.*

---


---

## Post-class Quiz: Iterating Through Dictionaries

---

### 
What does `for key in dict` iterate over?

A) Values
B) Keys
C) Key-value tuples
D) Indices

**Correct Answer: B**

*Explanation: By default, iterating over a dictionary yields its keys. Use `.values()` for values or `.items()` for key-value pairs.*

---

### 
How do you iterate over both keys and values?

A) `for k, v in dict`
B) `for k, v in dict.items()`
C) `for k, v in dict.values()`
D) `for k, v in dict.keys()`

**Correct Answer: B**

*Explanation: `.items()` returns key-value pairs as tuples, which can be unpacked into `k` and `v`. The other options either don't provide pairs or would cause errors.*

---

### 
What is the output?

```python
d = {'a': 1, 'b': 2, 'c': 3}
result = sum(d.values())
print(result)
```

A) `'abc'`
B) `6`
C) `3`
D) Error

**Correct Answer: B**

*Explanation: `d.values()` returns `[1, 2, 3]`. `sum()` adds them: 1 + 2 + 3 = 6.*

---

### 
What does `max(d, key=d.get)` return for `d = {'a': 3, 'b': 1, 'c': 5}`?

A) `5`
B) `'c'`
C) `('c', 5)`
D) `'a'`

**Correct Answer: B**

*Explanation: `max(d)` iterates over keys. `key=d.get` tells `max()` to compare keys by their values. Key 'c' has the highest value (5), so 'c' is returned — the key, not the value.*

---

### 
Why should you NOT modify a dict while iterating over it?

A) It's slower
B) It can cause RuntimeError
C) It's a syntax error
D) It deletes the dict

**Correct Answer: B**

*Explanation: Modifying a dictionary (adding/removing keys) during iteration causes `RuntimeError: dictionary changed size during iteration`. Instead, build a new dict or collect keys to modify, then apply changes after the loop.*

---


---

## Post-class Quiz: Merging Dictionaries

---

### 
What is the output?

```python
d1 = {'a': 1, 'b': 2}
d2 = {'b': 3, 'c': 4}
d1.update(d2)
print(d1)
```

A) `{'a': 1, 'b': 2}`
B) `{'a': 1, 'b': 3, 'c': 4}`
C) `{'a': 1, 'b': 2, 'c': 4}`
D) `{'b': 3, 'c': 4}`

**Correct Answer: B**

*Explanation: `update()` adds new keys ('c') and overwrites existing ones with the new values ('b' becomes 3). All original keys that aren't in d2 remain unchanged ('a' stays 1).*

---

### 
What is the difference between `d1.update(d2)` and `{**d1, **d2}`?

A) No difference
B) `update()` modifies d1 in place; `{**d1, **d2}` creates a new dict
C) `{**d1, **d2}` modifies d1 in place; `update()` creates a new dict
D) `update()` keeps d1 values; `{**d1, **d2}` keeps d2 values

**Correct Answer: B**

*Explanation: `update()` modifies the dictionary it's called on. Unpacking `{**d1, **d2}` creates a brand new dictionary, leaving both originals unchanged.*

---

### 
In `{**d1, **d2, **d3}`, which dict's values take priority for shared keys?

A) d1 (first one)
B) d2 (middle one)
C) d3 (last one)
D) It raises an error for duplicate keys

**Correct Answer: C**

*Explanation: The last dictionary in the unpacking wins for shared keys. It's evaluated left to right — each subsequent dict overwrites values from earlier ones.*

---

### 
What is the output?

```python
d1 = {'x': {'a': 1, 'b': 2}}
d2 = {'x': {'c': 3}}
result = {**d1, **d2}
print(result)
```

A) `{'x': {'a': 1, 'b': 2, 'c': 3}}`
B) `{'x': {'c': 3}}`
C) `{'x': {'a': 1, 'b': 2}, 'x': {'c': 3}}`
D) Error

**Correct Answer: B**

*Explanation: Dictionary merging is **shallow**. The entire value for key 'x' from d2 replaces d1's value. Nested dicts are not recursively merged — the inner dict `{'a': 1, 'b': 2}` is completely replaced by `{'c': 3}`.*

---

### 
Which creates a merged dict WITHOUT modifying the originals?

A) `d1.update(d2)`
B) `d1 |= d2`
C) `{**d1, **d2}`
D) Both A and B

**Correct Answer: C**

*Explanation: `{**d1, **d2}` creates a new dict. Both `update()` and `|=` modify d1 in place. If you need the originals unchanged, use unpacking or the `|` operator (without `=`): `d1 | d2`.*

---


---

