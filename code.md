<!-- Modules -->

// math.js (exports a function)
function add(a, b) {
if (typeof a !== 'number' || typeof b !== 'number') {
throw new Error('Inputs must be numbers!'); // Error handling addition
}
return a + b;
}

function subtract(a, b) {
return a - b;
}

module.exports = { add, subtract }; // Export object

// app.js (imports & uses)
const math = require("./math"); // Relative path
console.log(math.add(5, 7)); // 12
console.log(math.subtract(10, 3)); // 7

<!-- Raw-server -->

// raw-server.js (No NPM needed!)
const http = require('http'); // Built-in module

const server = http.createServer((req, res) => {
// req: Incoming request (method, url, headers)
// res: Outgoing response (write, end)

if (req.method === 'GET' && req.url === '/') {
// Manual route check
res.writeHead(200, { 'Content-Type': 'text/plain' }); // Manual headers
res.end('Hello from Raw Node.js!'); // Send body & close
} else {
res.writeHead(404, { 'Content-Type': 'text/plain' });
res.end('Not Found :('); // Basic error
}
});

const PORT = 3000;
server.listen(PORT, () => {
console.log(`Raw server running on http://localhost:${PORT}`);
});

<!-- Express -->

// server.js
const express = require("express"); // Import framework
const app = express(); // Create app instance
const PORT = 3000; // Config port (env var later)

app.get("/", (req, res) => { // Route: GET / (home)
res.send("<h1>Hello from Express!</h1>"); // Response: HTML or text
// res.json({ message: "JSON response" }); // Alternative: JSON
});

app.listen(PORT, () => { // Start server
console.log(`Server running on http://localhost:${PORT}`);
});

<!-- middlewares -->

app.use(express.json()); // Parses JSON bodies automatically

app.post("/echo", (req, res) => {
res.json({ echoed: req.body }); // req.body now available!
});

app.use((req, res, next) => { // Logs every request
console.log(`${req.method} ${req.url} - ${new Date()}`);
next(); // Pass to next handler (required!)
});

app.get("/protected", (req, res) => {
res.send("This was logged!");
});

<!-- Handling get Request -->

app.get("/user/:id", (req, res) => { // :id = placeholder
const userId = req.params.id; // Access: "123"
res.send(`User profile for ID: ${userId}`);
});

app.get("/search", (req, res) => {
const query = req.query.term; // ?term=nodejs → "nodejs"
res.send(`Searching for: ${query}`);
});

<!-- Handling post request -->

app.use(express.json()); // Must-have for req.body

app.post("/greet", (req, res) => {
const { name, age } = req.body; // Destructure
if (!name) return res.status(400).send("Name required!"); // Validation
res.json({ message: `Hello, ${name}! You are ${age || 'ageless'}.` });
});

curl -X POST -H "Content-Type: application/json" \
 -d '{"name":"Alex", "age":25}' \
 http://localhost:3000/greet

<!-- Serving static files -->

app.use(express.static("public")); // Folder: public/ with index.html, style.css

const express = require("express");
const path = require("path");

const app = express();
const PORT = 3000;

// Serve static files
app.use(express.static(path.join(\_\_dirname, "public")));

// Serve the main page
app.get("/", (req, res) => {
res.sendFile(path.join(\_\_dirname, "views", "index.html"));
});

app.listen(PORT, () => {
console.log(`Server running on http://localhost:${PORT}`);
});

<!-- html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>About Me</title>
  <link rel="stylesheet" href="/style.css" />
</head>
<body>
  <div class="container">
    <img src="/profile.jpg" alt="My Picture" class="profile-pic" />
    <h1>About Me</h1>
    <p>Hello! I'm learning Express.js and this page is served with static files 🎉</p>
    
    <input type="text" id="nameInput" placeholder="Enter your name" />
    <p id="greeting"></p>
  </div>

  <script src="/script.js"></script>
</body>
</html>

<!-- css -->

body {
margin: 0;
font-family: Arial, sans-serif;
background: #f7f7f7;
display: flex;
justify-content: center;
align-items: center;
height: 100vh;
}

.container {
text-align: center;
background: white;
padding: 30px;
border-radius: 12px;
box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.profile-pic {
width: 120px;
height: 120px;
border-radius: 50%;
object-fit: cover;
margin-bottom: 15px;
}

input {
padding: 10px;
font-size: 16px;
width: 80%;
max-width: 250px;
margin-top: 10px;
border-radius: 6px;
border: 1px solid #ccc;
}

<!-- js -->

const nameInput = document.getElementById("nameInput");
const greeting = document.getElementById("greeting");

nameInput.addEventListener("input", () => {
const name = nameInput.value.trim();
greeting.textContent = name ? `Nice to meet you, ${name}!` : "";
});

<!-- Error Handling -->

// At end of file (after routes)
app.use((err, req, res, next) => {
console.error(err.stack); // Log for debugging
res.status(500).json({ error: "Something went wrong!" });
});

// Example route that throws
app.get("/fail", (req, res) => {
throw new Error("Oops!"); // Caught by middleware
});

app.use((req, res) => {
res.status(404).send("Page not found :(");
});

<!-- Environment variables  -->

require("dotenv").config(); // Load .env
const PORT = process.env.PORT || 3000;

app.get("/secret", (req, res) => {
res.send(`DB connected to: ${process.env.DB_URL}`);
});

<!-- Example Real life -->

const express = require("express");
const app = express();
app.use(express.json());

app.get("/", (req, res) => res.send("Welcome to User API!"));

app.post("/register", (req, res) => {
const { name, email } = req.body;
if (!name || !email) return res.status(400).json({ error: "Missing fields" });
// Simulate DB save
res.status(201).json({ message: `Registered: ${name} (${email})` });
});

app.get("/user/:id", (req, res) => {
res.json({ id: req.params.id, name: "Sample User" });
});

app.listen(3000, () => console.log("API live on port 3000"));
