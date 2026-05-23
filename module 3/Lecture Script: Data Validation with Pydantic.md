# Lecture Script: Data Validation with Pydantic
**Facilitator-Facing | 110 Minutes | Beginner Level**

---

## Running Example: Smart Cart Web App

Throughout this lecture, every concept, demo, and walkthrough is built around a single running example: a **Smart Cart Web App** — an e-commerce backend where customers can register, add items to a cart, and place orders. This context makes every validation rule feel real and consequential. Every model we define, every error we handle, every constraint we set — it all lives inside Smart Cart.

Refer back to it constantly. When learners see the same app grow and evolve, the concepts stick.

---

## Session Overview

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 15 min |
| 2 | What Is Pydantic? | 20 min |
| 3 | Defining Data Models | 25 min |
| 4 | Request and Response Validation | 20 min |
| 5 | Advanced Validation | 30 min |
| 6 | Lecture Summary | 10 min |

---

## Block 1: Why Does This Matter?

### 🎯 Instructor Note — Hook Activity

Before saying anything, write this on the board or display it as a prompt:

> "A customer submits an order. Their age is `"twenty-five"`. Their email is `"not-an-email"`. Their cart quantity is `-3`. What happens to your app?"

Give learners 60 seconds to think individually, then ask two or three people to share. You are not looking for correct answers — you are activating the problem space. Let answers be messy. Then say:

[Script:]
"Every single one of those values will break something in your app — silently or loudly, immediately or three database calls later. The question is not whether bad data causes problems. It does. The question is: when do you want to catch it? At the door, or after it has already caused damage?"

---

[Script:]
"In real production APIs, bad input data is one of the most common sources of bugs. Not logic errors. Not algorithm mistakes. Just: someone sent you a string where you needed a number, or left a required field blank, or submitted a price as a negative value. And if you do not have a system that catches this automatically, you are writing that check manually — over and over — in every single route handler."

"Pydantic is that system. It catches bad data at the boundary of your application, before it ever touches your business logic, your database, or your other services. It is the bouncer at the door of your API."

"In FastAPI specifically, Pydantic is not optional — it is the foundation. Every request body, every response shape, every query parameter can be validated automatically using Pydantic models. Once you understand Pydantic, you understand the core of how FastAPI handles data."

"If you get this wrong, or skip it, you end up with one of two bad outcomes: either your app crashes with confusing internal errors, or worse, it accepts garbage data silently and corrupts your system state. Neither of those is acceptable in production."

---

## Block 2: What Is Pydantic?

### Clear Definition

[Script:]
"Pydantic is a Python library for data validation and settings management using Python type hints. The core idea is simple: you describe the shape of your data using a Python class, and Pydantic enforces that shape automatically when data comes in."

"Let me give you a mental model. Think of a Pydantic model like a form with labeled fields. The form says: 'Name must be text. Age must be a number. Email must look like an email.' When someone fills out the form, Pydantic checks every field. If anything is wrong, it tells you exactly which field failed and why — before you ever process the form."

---

### Python vs. No Validation — The Comparison

[Script:]
"Without Pydantic, here is what manual validation looks like in plain Python:"

```python
def create_user(data: dict):
    if "name" not in data:
        raise ValueError("name is required")
    if not isinstance(data["name"], str):
        raise ValueError("name must be a string")
    if "age" not in data:
        raise ValueError("age is required")
    if not isinstance(data["age"], int):
        raise ValueError("age must be an integer")
    # ... and so on for every field
```

"Now here is the Pydantic version:"

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

"That is it. Two lines. Pydantic handles all the checking automatically. That is the core trade. You declare the shape. Pydantic enforces it."

---

### Mental Model: Pydantic as a Contract

[Script:]
"Think of a Pydantic model as a contract between the outside world and your application. The outside world — a browser, a mobile app, another service — sends data. Your Pydantic model is the contract that says what that data must look like. If the data matches the contract, it comes in. If it does not, Pydantic rejects it with a precise error message before your code even runs."

"This contract idea is important. It means your route handler code can assume the data is valid. You do not need defensive checks inside your logic. Pydantic has already done that work at the boundary."

---

### Common Mistakes — Surface These Early

[Script:]
"A few mistakes I see beginners make. First: confusing Pydantic models with regular Python dataclasses or plain classes. They look similar, but Pydantic models validate on instantiation. A plain class does not."

"Second: assuming Pydantic fixes bad data. It does not. Pydantic rejects bad data and tells you what is wrong. It will coerce some types — for example, the string `'42'` will become the integer `42` if your field is typed as `int` — but it will not silently drop fields or invent values."

"Third: forgetting that Pydantic is separate from FastAPI. FastAPI uses Pydantic, but Pydantic works fine on its own. Understanding them separately helps you understand what each one is doing."

---

## Block 3: Defining Data Models — Learning Objective 1

### Problem

[Script:]
"Back to Smart Cart. We need to accept customer registrations. A customer has a name, an email address, and an age. Without any validation, our API has no way to know if the data coming in is correct. Someone could register with an age of `'banana'` and our app would have no idea."

"So the problem is: how do we describe what valid customer data looks like, in a way that Python can enforce automatically?"

---

### Translate Logic to Pydantic

[Script:]
"We need three fields. Name is a string. Email is a string — we will add email-specific validation shortly. Age is an integer. In Pydantic, we express this by inheriting from `BaseModel` and declaring fields with type hints."

"Every field without a default value is required. If a field has a default value, it is optional. That is the rule."

---

### Write the Code

```python
from pydantic import BaseModel

class CustomerCreate(BaseModel):
    name: str
    email: str
    age: int
```

[Script:]
"We import `BaseModel` from Pydantic. We define `CustomerCreate` as a subclass of it. We declare three fields with type annotations. That is all that is needed for basic validation."

---

### 🎯 Instructor Note — Prediction Pause

Write on the board before running:

```python
c = CustomerCreate(name="Alice", email="alice@example.com", age=30)
print(c)
```

Ask: "Predict before running: what will this print? Will it work? What does the output look like?"

Take two or three answers. Then run it.

---

### Demo 1 — Model Instantiation

```python
from pydantic import BaseModel

class CustomerCreate(BaseModel):
    name: str
    email: str
    age: int

c = CustomerCreate(name="Alice", email="alice@example.com", age=30)
print(c)
print(c.name)
print(c.model_dump())
```

[Script:]
"Notice three things. First: `print(c)` gives a clean representation showing the model name and field values. Second: you access fields like normal Python attributes — `c.name` returns `'Alice'`. Third: `model_dump()` returns the data as a plain Python dictionary, which is useful when you need to pass the data somewhere else."

---

### 🎯 Instructor Note — Prediction Pause: Type Coercion

Now write:

```python
c2 = CustomerCreate(name="Bob", email="bob@example.com", age="28")
print(c2.age, type(c2.age))
```

Ask: "The age here is the string `'28'`, not the integer `28`. What will happen? Will Pydantic reject it or accept it?"

Let learners predict. Then run it.

[Script:]
"Pydantic coerced the string `'28'` into the integer `28`. This is intentional. Pydantic tries to be helpful when the conversion is unambiguous. But watch what happens when it cannot coerce:"

```python
c3 = CustomerCreate(name="Carol", email="carol@example.com", age="banana")
```

[Script:]
"Now we get a `ValidationError`. Pydantic cannot turn the string `'banana'` into an integer. It rejects the data and tells you exactly which field failed and why. That is the behavior we want."

---

### Explain the Result

[Script:]
"The key takeaway: Pydantic validates on instantiation. The moment you create a model instance with bad data, you get an error. In FastAPI, this validation happens automatically when a request comes in — you never even write the instantiation line yourself. FastAPI does it for you."

---

### Optional Fields and Defaults

[Script:]
"In Smart Cart, suppose a customer's phone number is optional — they do not have to provide it. We use Python's `Optional` type from the `typing` module, combined with a default value of `None`."

```python
from typing import Optional
from pydantic import BaseModel

class CustomerCreate(BaseModel):
    name: str
    email: str
    age: int
    phone: Optional[str] = None
```

[Script:]
"Now `phone` is not required. If it is not provided, it defaults to `None`. If it is provided, it must be a string. That is the pattern for optional fields: `Optional[type]` with a default."

---

## Block 4: Request and Response Validation in FastAPI — Learning Objective 2

### Problem

[Script:]
"We have a Pydantic model. Now how does it connect to FastAPI? The problem is: when a POST request arrives at our API with a JSON body, FastAPI needs to parse that JSON, validate it, and hand us clean data. Without something doing this automatically, we would be writing `request.json()`, manually checking every field, and returning error responses ourselves."

---

### Translate Logic

[Script:]
"In FastAPI, you connect a Pydantic model to a route by using it as the type annotation for a parameter. FastAPI sees the annotation, reads the model, and automatically validates the incoming request body against it. If validation fails, FastAPI returns a 422 error response automatically — you do not write that code."

---

### Request Validation Walkthrough

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class CustomerCreate(BaseModel):
    name: str
    email: str
    age: int

@app.post("/customers")
def create_customer(customer: CustomerCreate):
    return {"message": f"Customer {customer.name} created"}
```

[Script:]
"Look at the route function signature. The parameter `customer` is annotated with `CustomerCreate`. That single annotation tells FastAPI: parse the request body as JSON, validate it against `CustomerCreate`, and inject the result as a `CustomerCreate` instance. If validation fails, FastAPI handles the error response. If it passes, your function receives a clean, validated object."

"Inside the function, `customer.name`, `customer.email`, `customer.age` are all guaranteed to be the right types. You do not need to check them."

---

### Response Validation with `response_model`

[Script:]
"There is a second side to this: response validation. When your API returns data, you also want to control what shape it returns. You do not want to accidentally leak internal fields — like a hashed password — in your response."

"FastAPI lets you declare a `response_model` on a route. The response data is filtered and validated against that model before it is sent back."

```python
class CustomerResponse(BaseModel):
    name: str
    email: str

@app.post("/customers", response_model=CustomerResponse)
def create_customer(customer: CustomerCreate):
    # Even if we return extra fields, only name and email go out
    return {"name": customer.name, "email": customer.email, "age": customer.age}
```

[Script:]
"Notice we have two separate models: `CustomerCreate` is for incoming data, `CustomerResponse` is for outgoing data. This separation is intentional and important. Incoming data might have a password field. Outgoing data should never include it. Different models for different directions."

---

### 🎯 Instructor Note — Pause Point

This is a common confusion point. Learners often ask: why not use the same model for both? Address it directly.

[Script:]
"Why separate models? Because what you accept and what you return are different things. When creating a customer, you need a password. When returning a customer, you never want to return the password. Separate models make this explicit and safe. FastAPI enforces the response shape for you."

---

### Demo 2 — Request and Response in Action

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class ItemCreate(BaseModel):
    name: str
    price: float
    quantity: int

class ItemResponse(BaseModel):
    name: str
    price: float

@app.post("/items", response_model=ItemResponse)
def add_item(item: ItemCreate):
    return item
```

[Script:]
"We add an item to our Smart Cart. The request model has `name`, `price`, and `quantity`. The response model has only `name` and `price`. Even though we return `item` — which includes `quantity` — FastAPI strips `quantity` from the response because it is not in `ItemResponse`. That is response model filtering in action."

---

## Block 5: Advanced Validation — Learning Objectives 3 and 4

### 5A: Nested Models and Complex Schemas

#### Problem

[Script:]
"In Smart Cart, an order is not just a flat set of fields. An order contains customer information and a list of items. Real-world data is nested. How do we model that with Pydantic?"

---

#### Translate Logic

[Script:]
"Pydantic models can reference other Pydantic models as field types. You can also use Python's `List` type to model a list of items. Pydantic handles the validation of nested structures automatically — it validates the outer model and each nested model inside it."

---

#### Write the Code

```python
from typing import List
from pydantic import BaseModel

class CartItem(BaseModel):
    product_name: str
    quantity: int
    unit_price: float

class Order(BaseModel):
    customer_name: str
    items: List[CartItem]
    delivery_address: str
```

[Script:]
"Here `Order` has a field `items` that is a list of `CartItem` objects. When Pydantic validates an `Order`, it also validates every `CartItem` inside that list. If any item is malformed — wrong type, missing field — the whole validation fails and you get a precise error pointing to the exact nested field."

---

### 🎯 Instructor Note — Prediction Pause

Write on the board:

```python
order = Order(
    customer_name="Alice",
    items=[
        {"product_name": "Laptop", "quantity": 1, "unit_price": 999.99},
        {"product_name": "Mouse", "quantity": 2, "unit_price": "not-a-price"}
    ],
    delivery_address="123 Main St"
)
```

Ask: "Predict before running: which item causes a validation error? What will the error message tell us?"

Run it and read the error together.

[Script:]
"The error message tells you exactly where the problem is: `items -> 1 -> unit_price` — the second item, field `unit_price`, input was not a valid float. Pydantic traces the full path through the nested structure to tell you exactly what went wrong. That is enormously useful for debugging."

---

### 5B: Field Constraints and Validation Rules

#### Problem

[Script:]
"Saying a field is a `float` is not enough for a price field. A price cannot be negative. A quantity cannot be zero or negative. A product name cannot be an empty string. We need to express these business rules as constraints."

---

#### Translate Logic

[Script:]
"Pydantic provides a `Field` function that lets you attach constraints to fields directly in the model. `gt` means greater than. `ge` means greater than or equal. `lt` and `le` are less than and less than or equal. `min_length` and `max_length` constrain string length."

---

#### Write the Code

```python
from pydantic import BaseModel, Field

class CartItem(BaseModel):
    product_name: str = Field(min_length=1, max_length=100)
    quantity: int = Field(gt=0)
    unit_price: float = Field(gt=0.0)
```

[Script:]
"Now `quantity` must be greater than zero. `unit_price` must be greater than zero. `product_name` must be between one and one hundred characters. If any of these constraints are violated, Pydantic rejects the data immediately with a message that says exactly which constraint failed."

---

### 🎯 Instructor Note — Prediction Pause

Write:

```python
item = CartItem(product_name="Keyboard", quantity=0, unit_price=49.99)
```

Ask: "What happens? Which field fails? What does the error say?"

Run it.

[Script:]
"Pydantic tells us `quantity` failed: input should be greater than 0. The constraint name, the field name, and the actual input value are all in the error. This is the error message your API will return automatically — clean, precise, actionable."

---

### 5C: Custom Validators

#### Problem

[Script:]
"Field built-in constraints cover a lot of cases, but not everything. In Smart Cart, we want to validate that a customer's email address actually looks like an email. And we want to ensure that a customer's name does not contain digits — a name like `'Alice123'` is not a real name."

"For these cases, we write custom validators."

---

#### Translate Logic

[Script:]
"Pydantic provides a `field_validator` decorator that lets you attach a validation function to one or more fields. This function receives the field value and either returns it — possibly transformed — or raises a `ValueError` if it is invalid. The error message you put in the `ValueError` is what Pydantic includes in its error response."

---

#### Write the Code

```python
from pydantic import BaseModel, field_validator

class CustomerCreate(BaseModel):
    name: str
    email: str
    age: int

    @field_validator("email")
    @classmethod
    def email_must_contain_at(cls, value):
        if "@" not in value:
            raise ValueError("email must contain @")
        return value

    @field_validator("name")
    @classmethod
    def name_must_be_alphabetic(cls, value):
        if not value.replace(" ", "").isalpha():
            raise ValueError("name must contain only letters and spaces")
        return value
```

[Script:]
"Two things to notice. First: `@field_validator` takes the field name as a string argument. Second: the method must be a `classmethod`. Third: you either `return value` to accept it, or `raise ValueError` to reject it. The message in the `ValueError` is included in the validation error response."

---

### 🎯 Instructor Note — Prediction Pause

Write on the board:

```python
c = CustomerCreate(name="Alice3", email="not-an-email", age=25)
```

Ask: "How many validation errors will we get? Which fields fail? In what order does Pydantic report them?"

Run it.

[Script:]
"We get two errors — one for `name` and one for `email`. Pydantic runs all field validators and collects all errors together. It does not stop at the first failure. That is intentional: you get the full picture of what is wrong in a single response, which is much better for API consumers."

---

### 5D: Handling Validation Errors and Error Responses

#### Problem

[Script:]
"Pydantic raises a `ValidationError` when data is invalid. In FastAPI, this is handled automatically — FastAPI catches it and returns a 422 response. But what does that response look like? And what do you do when you need to handle validation errors yourself, outside of a FastAPI route?"

---

#### The ValidationError Object

[Script:]
"The `ValidationError` exception has a method called `errors()` that returns a list of error details. Each error is a dictionary with three key fields: `loc` — the location of the field that failed, as a tuple; `msg` — a human-readable message; and `type` — a machine-readable error code."

```python
from pydantic import BaseModel, ValidationError

class CartItem(BaseModel):
    product_name: str
    quantity: int
    unit_price: float

try:
    item = CartItem(product_name="", quantity=-1, unit_price=-5.0)
except ValidationError as e:
    print(e.errors())
```

[Script:]
"When we run this — remembering we have Field constraints on this model — we see a list of error dictionaries. Each one tells us the field, the problem, and a code. This is exactly the structure FastAPI returns to the client in a 422 response body."

---

### 🎯 Instructor Note — Run and Annotate

Run the above code. Point to each field in the `errors()` output and map it back to the model definition. Say:

[Script:]
"Look at the `loc` field — it is a tuple. For a top-level field, it is just `('quantity',)`. For a nested model, it would be `('items', 0, 'unit_price')` — path notation through the nesting. This is how you tell a client exactly where their submission went wrong, even in a deeply nested payload."

---

### Custom Error Responses in FastAPI

[Script:]
"By default, FastAPI handles `ValidationError` automatically with a 422 response. Sometimes you want to customize that — maybe you want a different HTTP status code, or a different response shape. FastAPI lets you add an exception handler using `@app.exception_handler`."

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse
from pydantic import ValidationError

app = FastAPI()

@app.exception_handler(ValidationError)
async def validation_error_handler(request: Request, exc: ValidationError):
    return JSONResponse(
        status_code=400,
        content={"errors": exc.errors()}
    )
```

[Script:]
"This is optional and not always necessary — FastAPI's default 422 behavior is correct in most cases. But knowing you can override it gives you full control. The key is that `exc.errors()` is always your source of truth for what went wrong."

---

### 🎯 Instructor Note — Confusion Point: 422 vs 400

Learners often ask: why 422 and not 400? Address it:

[Script:]
"HTTP 422 means 'Unprocessable Entity' — the server understood the request format, but the content was semantically invalid. HTTP 400 means 'Bad Request' — often used for malformed syntax. FastAPI uses 422 by default for validation failures because the data was syntactically valid JSON, but did not pass semantic validation. Both are used in practice. The important thing is consistency — pick one and use it everywhere in your API."

---

## Block 6: Lecture Summary

- **Pydantic provides automatic data validation** using Python type hints. You declare the shape of your data in a class that inherits from `BaseModel`, and Pydantic enforces it on instantiation.

- **Defining models with type hints** gives you required and optional fields, automatic type coercion for unambiguous conversions, and immediate rejection with precise error messages for invalid data.

- **In FastAPI, request validation is automatic.** Annotate a route parameter with a Pydantic model type and FastAPI validates the incoming request body against it, returning a 422 error response if validation fails — without any manual code.

- **Response models control what your API returns.** Use `response_model` on a route to filter and validate outgoing data, preventing accidental exposure of internal fields.

- **Nested models let you validate complex, real-world data structures.** A Pydantic model field can be another Pydantic model or a list of models. Validation errors include the full path through the nesting so you always know exactly where the problem is.

- **Field constraints using `Field(gt=..., min_length=..., ...)` enforce business rules** at the model level — no negative prices, no empty names, no zero quantities — without any manual `if` checks.

- **Custom validators using `@field_validator`** let you encode arbitrary business logic — email format checks, name character rules, cross-field comparisons — directly in the model. Return the value to accept it; raise `ValueError` to reject it.

- **Pydantic collects all validation errors at once**, not just the first one. The `ValidationError.errors()` method returns a structured list of every failure, with field location, human-readable message, and error type code.

- **The practical value of all of this:** your route handler code can be written with confidence that the data it receives is valid. You stop writing defensive checks inside business logic. You stop writing manual error responses. Pydantic handles the boundary. Your code handles the logic. That separation makes your application smaller, more readable, and dramatically easier to maintain.

---

### 🎯 Instructor Note — Closing Question

End by returning to the hook:

[Script:]
"Remember that question at the start — age as `'twenty-five'`, email as `'not-an-email'`, quantity as `-3`. Walk through each one now. What would happen with the models we built today?"

Let learners answer. Each one should now be able to say: Pydantic catches it at the boundary, returns a structured error with the field name and reason, and the route handler never runs. That is the full picture. That is what they learned today.