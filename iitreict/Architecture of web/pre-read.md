# The Architecture of the Web

## In this pre-read, you'll discover:

- How your browser and servers talk to each other
- What happens when you visit a website
- Why apps use JSON to exchange data
- How cookies and sessions remember you
- What CORS does and why it blocks some requests

---

## How the Web Actually Works

When you type a website address into your browser, a lot happens behind the scenes. Let's break it down step by step.

### The Client-Server Architecture

**Client** is your browser (Chrome, Firefox, Safari). It asks for things and shows you the results.

**Server** is a powerful computer somewhere that stores files, runs code, and answers questions from clients.

Think of it like a restaurant. You (the client) sit at a table and place an order. The kitchen (the server) prepares your food and sends it back to you. You don't need to know how the kitchen works—you just want your food!

### The Request-Response Cycle

Every time you interact with a website, this happens:

1. **Your browser sends a request** — "Hey, give me this page!"
2. **The server receives the request** — It figures out what you asked for
3. **The server sends a response** — "Here's that page!" or "Here's that data!"
4. **Your browser receives the response** — It shows you the result

**Why it matters:** This cycle happens every single time you click a button, submit a form, or load a new page. Understanding this flow helps you debug issues and build better apps.

### JSON: The Data Format

**JSON** (JavaScript Object Notation) is a way to structure data so computers can read it easily. It's just text formatted in a specific way.

Here's what JSON looks like:

```json
{
  "name": "Alex",
  "age": 28,
  "hobbies": ["reading", "coding", "hiking"]
}
```

JSON has two main structures:
- **Objects** — wrapped in curly braces `{}`, contain key-value pairs
- **Arrays** — wrapped in square brackets `[]`, contain lists of items

**Why it matters:** Most web apps use JSON to send data back and forth. APIs return JSON, your frontend parses it, and your app uses it.

### Parsing JSON

Your browser can convert JSON text into a usable JavaScript object:

```javascript
const jsonString = '{"name": "Alex", "age": 28}';
const data = JSON.parse(jsonString);
console.log(data.name); // "Alex"
```

### Cookies: Tiny Data Files

**Cookies** are small pieces of data that websites store in your browser. They help remember you between visits.

When you log into a site, the server might send a cookie like this:

```
session_id = "abc123xyz"
```

Your browser saves this cookie. Next time you visit, your browser shows the server that cookie, and the server says "Oh, it's you!"

**Why it matters:** Cookies keep you logged in, remember your shopping cart, and personalize your experience.

### Sessions: Server-Side Memory

**Sessions** are like temporary files on the server that store information about you. The server links your session to your cookie.

Here's the flow:
1. You log in
2. Server creates a session with your user info
3. Server sends you a cookie with a session ID
4. Server looks up your session using that ID

**Why it matters:** Sessions keep sensitive data on the server (safer) while using a simple ID on the client side.

### CORS: Cross-Origin Resource Sharing

**CORS** is a security rule that controls which websites can access your API.

By default, browsers block one website from making requests to another website. This prevents malicious sites from stealing your data.

When you want to allow other sites to use your API, you add CORS headers:

```
Access-Control-Allow-Origin: https://example.com
```

**Why it matters:** If you're building an API, you need to understand CORS to let frontend apps access your data—or to know why your requests are being blocked.

---

## Practice Exercises

1. Draw a simple diagram showing a client, a server, and an arrow going each direction. Label the arrows "request" and "response."

2. Write a JSON object representing a book with a title, author, and number of pages.

3. If you have this JSON: `{"color": "blue", "size": "large"}`, what code would you use to get the value "blue"?

4. Explain in one sentence why cookies are useful for keeping users logged in.

5. What would happen if a website at `evil.com` tried to fetch data from `bank.com` without CORS rules?

6. True or false: The server always sends back a webpage. Explain your answer.
