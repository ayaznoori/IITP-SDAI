# FastAPI Fundamentals - Lecture Script

## Why Does This Matter?

🎯 **Instructor Note**: Start with this hook question: "How many of you have ever tried to build a web API and spent more time on setup and documentation than actual business logic?"

[Script:] Today we're diving into FastAPI, a modern Python web framework that's revolutionizing how we build APIs. FastAPI has become the go-to choice for companies like Netflix, Uber, and Microsoft because it solves three critical problems that plague traditional API development.

First, it's incredibly fast - both in development speed and runtime performance. You can build a production-ready API in minutes, not hours. Second, it automatically generates interactive documentation that stays in sync with your code. No more outdated API docs that confuse your team. Third, it provides built-in data validation and type safety, catching errors before they reach production.

If you misunderstand these fundamentals, you'll find yourself writing verbose, error-prone code, manually maintaining documentation, and debugging issues that FastAPI would have caught automatically. Master these concepts, and you'll build robust APIs faster than ever before.

## What Is the Concept?

FastAPI is a modern, high-performance web framework for building APIs with Python. Think of it as Flask's more sophisticated cousin - it provides the simplicity you love but adds automatic documentation, data validation, and type hints.

**Key Mental Model**: FastAPI is like having a smart assistant that reads your Python type hints and automatically handles validation, serialization, and documentation. You write normal Python functions, add some decorators, and FastAPI transforms them into a full-featured web API.

**Comparison**: If you know Flask, FastAPI follows similar patterns but with superpowers. Where Flask requires manual validation and documentation, FastAPI does it automatically. If you're coming from Node.js/Express, FastAPI provides similar routing but with Python's clean syntax and automatic type checking.

**Common Mistakes to Avoid**:
- Forgetting to import FastAPI components
- Not using type hints (FastAPI relies on them heavily)
- Confusing path parameters with query parameters
- Trying to use complex objects without Pydantic models

## How Do We Apply It?

### Learning Objective 1: Introduction to FastAPI Framework & Creating Your First Application

**Problem**: You need to create a web API that can handle HTTP requests and return JSON responses.

**Translate Logic**: 
1. Import FastAPI
2. Create an application instance
3. Define route handlers using decorators
4. Return data from functions

**Write Code**:
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}

@app.get("/health")
def health_check():
    return {"status": "healthy"}
```

**Predict Before Running**: What will happen when we visit the root URL and the health endpoint?

🎯 **Instructor Note**: Pause here for predictions, then run the demo.

**Demo Result**: When you run this with `uvicorn main:app --reload`, you get a fully functional API server. Visit `http://localhost:8000/` and see the JSON response. The `/health` endpoint returns status information.

**Explain Result**: FastAPI automatically converts your Python dictionary returns into JSON responses, sets proper HTTP headers, and creates a web server. The `@app.get()` decorator tells FastAPI this function handles GET requests to that path.

### Learning Objective 2: Path Parameters and Query Parameters

**Problem**: You need to capture dynamic values from URLs and optional parameters from query strings.

**Translate Logic**:
1. Path parameters: capture parts of the URL path
2. Query parameters: capture optional key-value pairs after `?`
3. Use function parameters to receive these values

**Write Code**:
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int, include_posts: bool = False):
    return {
        "user_id": user_id, 
        "include_posts": include_posts,
        "type_of_user_id": type(user_id).__name__
    }
```

**Predict Before Running**: What happens if we visit `/users/123?include_posts=true`? What about `/users/abc`?

🎯 **Instructor Note**: Let students predict, then demonstrate both valid and invalid inputs.

**Demo Result**: 
- `/users/123?include_posts=true` returns `{"user_id": 123, "include_posts": true, "type_of_user_id": "int"}`
- `/users/abc` returns a validation error because `abc` cannot convert to `int`

**Explain Result**: FastAPI automatically converts and validates path parameters based on type hints. Query parameters become function parameters with default values. The framework handles all parsing and validation.

### Learning Objective 3: Request Bodies and Response Models

**Problem**: You need to accept structured data in requests and ensure consistent response formats.

**Translate Logic**:
1. Use Pydantic models to define data structures
2. Accept models as function parameters for request bodies
3. Return models or dictionaries that match expected schemas

**Write Code**:
```python
from fastapi import FastAPI
from pydantic import BaseModel

class User(BaseModel):
    name: str
    email: str
    age: int

app = FastAPI()

@app.post("/users")
def create_user(user: User):
    return {"message": f"Created user {user.name}", "user_data": user}
```

**Predict Before Running**: What happens when we POST valid JSON? What about invalid data?

🎯 **Instructor Note**: Use a tool like curl or show the automatic docs to test this.

**Demo Result**: Valid JSON creates the user and returns structured data. Invalid JSON (missing fields, wrong types) returns detailed validation errors.

**Explain Result**: Pydantic models automatically validate incoming JSON against your schema. FastAPI handles serialization/deserialization and provides detailed error messages for invalid data.

### Learning Objective 4: OpenAPI and Automatic API Documentation

**Problem**: You need comprehensive, up-to-date API documentation without manual maintenance.

**Translate Logic**:
1. FastAPI automatically generates OpenAPI specifications
2. Access interactive docs at `/docs` (Swagger UI)
3. Access alternative docs at `/redoc`
4. Documentation updates automatically with code changes

**Write Code**:
```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    description: str = None

app = FastAPI(title="My API", version="1.0.0")

@app.post("/items", summary="Create an item")
def create_item(item: Item):
    """Create a new item with name, price, and optional description."""
    return {"item_id": 1, "item": item}
```

**Predict Before Running**: What will we see when we visit `/docs`?

🎯 **Instructor Note**: Navigate to the docs URL and explore the interactive interface together.

**Demo Result**: A complete interactive API documentation interface appears, showing all endpoints, request/response schemas, and allowing live testing.

**Explain Result**: FastAPI reads your type hints, function signatures, and docstrings to automatically generate OpenAPI-compliant documentation. The docs stay synchronized with your code without any extra work.

## Lecture Summary

- **FastAPI Framework Introduction**: FastAPI is a modern Python web framework that combines simplicity with powerful features like automatic validation and documentation
- **First Application Creation**: Create APIs by importing FastAPI, instantiating an app, and using decorators like `@app.get()` to define route handlers
- **Path Parameters**: Capture dynamic URL segments using curly braces `{param}` and type hints for automatic conversion and validation
- **Query Parameters**: Accept optional URL parameters by defining function parameters with default values
- **Request Bodies**: Use Pydantic models to define and validate structured input data from POST/PUT requests
- **Response Models**: Structure API responses using Pydantic models or dictionaries for consistent data formats
- **OpenAPI Integration**: FastAPI automatically generates OpenAPI specifications from your code structure and type hints
- **Automatic Documentation**: Access interactive API docs at `/docs` and `/redoc` that update automatically as your code evolves
- **Practical Value**: These fundamentals enable you to build production-ready APIs rapidly while maintaining high code quality, automatic validation, and self-documenting interfaces that reduce development time and debugging effort
