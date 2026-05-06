# How the Web Works and Client-Server Communication

## What You'll Learn

In this pre-read, you'll discover:

- How computers talk to each other across the internet using a client-server model
- What happens when you type a website address and press Enter
- How data moves back and forth between your browser and a server
- The structure of JSON, a common format for sharing data online
- Three ways browsers can save data: localStorage, sessionStorage, and cookies

---

## Detailed Explanation

### How the Web Actually Works

Imagine you want to order food. You call a restaurant, place your order, and wait. The restaurant prepares your food and sends it back to you. This is exactly how the **web** works.

**Key terms:**

- **Client** — Your browser (like Chrome or Firefox). It's the one making requests.
- **Server** — A powerful computer that stores websites and data. It responds to requests.
- **Request** — A message from your browser asking for something.
- **Response** — The server's answer. It usually contains data or a webpage.

### The Request-Response Cycle

Every time you visit a website, this happens:

1. Your browser builds a **request** — a message saying "Hey, give me this page!"
2. The request travels across the internet to a server
3. The server processes the request
4. The server sends back a **response** — the data you asked for
5. Your browser displays the result

This whole process takes just milliseconds.

### What an HTTP Request Looks Like

HTTP stands for **HyperText Transfer Protocol** — a set of rules for sending messages. A simple request has three parts:

```
GET /home HTTP/1.1
Host: example.com
```

- **GET** — The method. It means "I want to retrieve something."
- **/home** — The path. The specific resource you want.
- **example.com** — The server address.

The response might look like:

```
HTTP/1.1 200 OK
Content-Type: application/json

{"message": "Welcome!", "status": "success"}
```

- **200** — The status code. 200 means "everything worked!"
- The body contains JSON data.

### Why This Matters

Understanding this cycle helps you debug issues. When something breaks, you'll know whether the problem is in sending the request, processing it, or receiving the response.

Benefits:

- You can build interactive websites that fetch real data
- You understand why some things load slowly
- You can fix errors by seeing exactly where they occur

### JSON — How Data Is Formatted

**JSON** stands for JavaScript Object Notation. It's a text format that computers use to exchange data. It looks almost like regular JavaScript objects.

Here's a JSON object:

```json
{
  "name": "Alex",
  "age": 25,
  "skills": ["HTML", "CSS", "JavaScript"]
}
```

**Why JSON?**

- It's easy for humans to read
- It's easy for computers to parse
- Almost every programming language can work with it

You can convert JSON strings into usable data in JavaScript:

```javascript
const jsonString = '{"name": "Alex", "age": 25}';
const user = JSON.parse(jsonString);
console.log(user.name); // "Alex"
```

### Browser Storage — Saving Data Locally

Websites can save data in your browser. This is useful for remembering preferences or keeping users logged in.

**localStorage** — Data stays until you manually delete it:

```javascript
localStorage.setItem("username", "Alex");
const name = localStorage.getItem("username");
console.log(name); // "Alex"
```

**sessionStorage** — Data disappears when you close the tab:

```javascript
sessionStorage.setItem("cart", "2 items");
```

**Cookies** — Small pieces of data sent between browser and server. They're often used for:

- Keeping you logged in
- Tracking shopping cart items
- Remembering site preferences

Cookies can expire automatically. They also get sent to the server with every request, which helps the server know who you are.

---

## Practice Exercises

1. Draw a simple diagram showing a client connected to a server with an arrow going each direction. Label the arrows "request" and "response."

2. Write a JSON object representing a book with three properties: title, author, and yearPublished.

3. Write two lines of code that save "Hello World" to localStorage under the key "greeting", then retrieve it.

4. Explain in one sentence the difference between localStorage and sessionStorage.

5. What does HTTP status code 200 mean? What about 404?

6. If you wanted to store a user's theme preference (light or dark mode) so it persists after they close the browser, which storage method would you use?