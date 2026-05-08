# Pre-Read: HTTP Protocol, Methods – Making Requests from Browser to Server using Fetch API

## What You'll Learn

In this pre-read, you'll discover:

- How browsers and servers talk to each other using HTTP
- The different types of requests you can make (GET, POST, PUT, PATCH, DELETE)
- How to use Fetch API to send requests from JavaScript
- How to handle the data that comes back from the server
- How to perform basic CRUD operations using Fetch API

---

## Detailed Explanation

### What is HTTP?

**HTTP** stands for HyperText Transfer Protocol. It's the set of rules that computers follow when they communicate over the web.

Think of HTTP like a postal service. When you send a letter, you write an address, put it in an envelope, and mail it. The post office delivers it. The recipient reads it and sends a reply back. HTTP works the same way—your browser sends a request, the server receives it, processes it, and sends back a response.

### Why It Matters

Every time you click a button, submit a form, or see new content load on a page, HTTP is working behind the scenes. Understanding HTTP helps you:

- Build interactive web applications
- Debug issues when things go wrong
- Work with any web API confidently
- Understand how the entire web operates

### HTTP Request Structure

An HTTP request has three main parts:

1. **Request line** – Contains the method, path, and HTTP version
2. **Headers** – Extra information about the request (like data type)
3. **Body** – The actual data being sent (optional)

A simple request looks like this:

```
GET /users HTTP/1.1
Host: example.com
```

### HTTP Methods

**HTTP methods** are verbs that tell the server what action to perform.

| Method | Action | Real-world example |
|--------|--------|-------------------|
| **GET** | Read/retrieve data | Loading your profile |
| **POST** | Create new data | Signing up for an account |
| **PUT** | Replace data entirely | Updating all your profile details |
| **PATCH** | Update part of the data | Changing just your email |
| **DELETE** | Remove data | Deleting a post |

### CRUD Operations

**CRUD** stands for Create, Read, Update, Delete. These are the four basic things you can do with data:

- **Create** → POST
- **Read** → GET
- **Update** → PUT or PATCH
- **Delete** → DELETE

### Fetch API

The **Fetch API** is a built-in JavaScript tool for making HTTP requests. It comes with JavaScript, so you don't need to install anything.

Here's a basic GET request:

```javascript
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(data => console.log(data));
```

This sends a GET request, converts the response to JSON, and logs the data.

### Handling Responses

The server sends back a **response** with a status code:

- **200** – Success
- **201** – Created successfully
- **400** – Bad request (something was wrong)
- **404** – Not found
- **500** – Server error

Always check if the response was successful:

```javascript
fetch('https://api.example.com/users')
  .then(response => {
    if (!response.ok) {
      throw new Error('Request failed');
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Sending Data with POST

To send data to the server, include the method and body:

```javascript
fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ name: 'Alice', email: 'alice@example.com' }
})
  .then(response => response.json())
  .then(data => console.log('Created:', data));
```

---

## Practice Exercises

1. **Match the method** – Which HTTP method would you use for each action?
   - Loading a list of products
   - Submitting a contact form
   - Deleting your account
   - Changing your password

2. **Read the code** – What does this fetch call do?
   ```javascript
   fetch('/api/posts')
   ```

3. **Write a GET request** – Create a fetch call that gets users from `/api/users`.

4. **Write a POST request** – Create a fetch call that sends `{ title: 'My Post' }` to `/api/posts`.

5. **Add headers** – Add the proper headers to send JSON data in a POST request.

6. **Handle errors** – Add a `.catch()` block to handle network failures in a fetch request.
