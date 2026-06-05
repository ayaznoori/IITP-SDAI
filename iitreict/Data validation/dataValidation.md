# Data Validation with Pydantic

## Why Does This Matter?

🎯 **Instructor Note**: Start with this hook - ask students to think about a time they received bad data that broke their code, then share a quick story about debugging mysterious errors caused by unexpected data types.

[Script:] Imagine you're building an API that accepts user registration data. A user submits their age as the string "twenty-five" instead of the number 25. Your code expects an integer, crashes, and now you're debugging at 2 AM wondering why your perfectly good logic failed. This is the reality of working with external data - whether from APIs, user forms, or file uploads.

Pydantic solves this exact problem by automatically validating and converting data types before they reach your business logic. Instead of writing dozens of if-statements to check data types and formats, Pydantic handles validation declaratively. When validation fails, you get clear error messages instead of cryptic crashes deep in your code.

The pain of not understanding data validation is real: production bugs, security vulnerabilities from malformed input, and hours spent debugging issues that proper validation would have caught immediately. Pydantic transforms data validation from a tedious chore into an elegant, automatic process.

## What Is the Concept?

**Pydantic** is a Python library that uses type hints to validate data automatically. Think of it as a bouncer for your data - it checks that incoming information meets your requirements before letting it into your application.

**Mental Model**: If regular Python classes are like unlocked houses where anyone can enter, Pydantic models are like houses with smart locks that only let in visitors who meet specific criteria.

**Key Components**:
- **BaseModel**: The foundation class you inherit from to create validated data structures
- **Type Hints**: Python annotations that tell Pydantic what data types to expect
- **Automatic Validation**: Pydantic checks and converts data types without explicit code
- **Field Constraints**: Additional rules like minimum values, string patterns, or custom validation logic

**Python vs JavaScript Comparison**: While JavaScript relies on runtime checks or external libraries like Joi for validation, Python with Pydantic leverages the type system built into the language. JavaScript might use `if (typeof age !== 'number')`, while Pydantic handles this automatically through type annotations.

**Common Mistakes**:
- Forgetting to inherit from `BaseModel`
- Using regular Python classes instead of Pydantic models for validation
- Not handling validation errors properly
- Overcomplicating validation with custom validators when built-in constraints would work

## How Do We Apply It?

### Learning Objective 1: Pydantic Models for Data Validation

**Problem**: We need to validate user data consistently without writing repetitive validation code.

**Translate Logic**: Create a class that inherits from Pydantic's BaseModel and use type hints to specify expected data types. Pydantic will automatically validate any data passed to create an instance.

**Write Code**:
```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    email: str

# Create a user with valid data
user_data = {"name": "Alice", "age": 25, "email": "alice@example.com"}
user = User(**user_data)
print(f"User: {user.name}, Age: {user.age}")
```

**Predict Before Running**: What will happen when we create a User with this data?

**Explain Result**: Pydantic validates that name and email are strings, age is an integer, then creates a User instance. The model automatically handles the validation without any explicit checking code.

### Learning Objective 2: Type Hints and Automatic Validation

**Problem**: We want automatic type conversion and validation without writing manual type checking.

**Translate Logic**: Pydantic uses Python's type hints to understand expected data types and automatically converts compatible types while rejecting incompatible ones.

**Write Code**:
```python
from pydantic import BaseModel

class Product(BaseModel):
    name: str
    price: float
    in_stock: bool

# Test automatic conversion
product = Product(name="Laptop", price="999.99", in_stock="true")
print(f"Price type: {type(product.price)}")
print(f"In stock type: {type(product.in_stock)}")
```

**Predict Before Running**: What types will price and in_stock have after Pydantic processes them?

**Explain Result**: Pydantic converts the string "999.99" to float 999.99 and "true" to boolean True. This automatic conversion saves us from writing manual parsing code while ensuring type safety.

🎯 **Instructor Note**: Pause here to emphasize that Pydantic is smart about conversions - it converts "999.99" to float but would reject "not_a_number".

### Learning Objective 3: Nested Models and Complex Schemas

**Problem**: Real-world data often has nested structures that need validation at multiple levels.

**Translate Logic**: Create separate Pydantic models for different data structures, then reference one model inside another using type hints.

**Write Code**:
```python
from pydantic import BaseModel
from typing import List

class Address(BaseModel):
    street: str
    city: str
    zip_code: str

class Customer(BaseModel):
    name: str
    address: Address
    phone_numbers: List[str]

# Create nested data
customer_data = {
    "name": "Bob",
    "address": {"street": "123 Main St", "city": "Boston", "zip_code": "02101"},
    "phone_numbers": ["555-1234", "555-5678"]
}
customer = Customer(**customer_data)
print(f"Customer lives in {customer.address.city}")
```

**Predict Before Running**: How will Pydantic handle the nested address dictionary?

**Explain Result**: Pydantic automatically creates an Address model instance from the nested dictionary and validates both the outer Customer and inner Address data structures.

### Learning Objective 4: Custom Validators and Field Constraints

**Problem**: We need validation rules beyond basic type checking, like ensuring emails are properly formatted or ages are within reasonable ranges.

**Translate Logic**: Use Pydantic's Field function to add constraints like minimum/maximum values, or create custom validator functions for complex rules.

**Write Code**:
```python
from pydantic import BaseModel, Field, validator

class Employee(BaseModel):
    name: str = Field(min_length=2, max_length=50)
    age: int = Field(ge=18, le=65)  # ge = greater or equal
    email: str
    
    @validator('email')
    def validate_email(cls, v):
        if '@' not in v:
            raise ValueError('Invalid email format')
        return v

# Test validation
employee = Employee(name="Carol", age=30, email="carol@company.com")
print(f"Valid employee: {employee.name}")
```

**Predict Before Running**: What happens if we try to create an employee with age 16 or email "invalid-email"?

**Explain Result**: Field constraints automatically check that age is between 18-65 and name length is 2-50 characters. The custom validator ensures email contains '@'. Invalid data raises clear validation errors.

### Learning Objective 5: Error Handling in Pydantic

**Problem**: When validation fails, we need to handle errors gracefully and provide useful feedback.

**Translate Logic**: Wrap Pydantic model creation in try-except blocks to catch ValidationError, then extract specific error information for user-friendly messages.

**Write Code**:
```python
from pydantic import BaseModel, ValidationError

class Account(BaseModel):
    username: str
    balance: float

try:
    # This will fail validation
    account = Account(username="", balance="not_a_number")
except ValidationError as e:
    print("Validation failed:")
    for error in e.errors():
        field = error['loc'][0]
        message = error['msg']
        print(f"  {field}: {message}")
```

**Predict Before Running**: What specific errors will Pydantic report for the invalid data?

**Explain Result**: ValidationError contains detailed information about each validation failure. We can iterate through errors to show user-friendly messages for each problematic field.

🎯 **Instructor Note**: Emphasize that good error handling makes the difference between a frustrating user experience and a helpful one.

## Lecture Summary

- **Pydantic models for data validation**: Create classes inheriting from BaseModel with type hints to automatically validate incoming data without manual checking code
- **Type hints and automatic validation**: Pydantic uses Python type annotations to validate and convert data types automatically, turning strings into numbers or booleans when appropriate
- **Nested models and complex schemas**: Build sophisticated data structures by referencing Pydantic models within other models, enabling validation of complex nested data
- **Custom validators and field constraints**: Add validation rules beyond basic types using Field constraints for ranges and lengths, plus custom validator functions for complex business logic
- **Error handling in Pydantic**: Catch ValidationError exceptions to handle validation failures gracefully and provide specific, actionable error messages to users
- **Practical value**: Pydantic eliminates the tedious and error-prone work of manual data validation, making your code more reliable and your debugging sessions shorter while providing clear feedback when data doesn't meet expectations
