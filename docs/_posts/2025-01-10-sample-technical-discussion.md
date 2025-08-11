---
layout: post
title: "Sample Technical Discussion: Building a REST API"
date: 2025-01-10
categories: technical
tags: [api, rest, programming, tutorial]
---

# Sample Technical Discussion: Building a REST API

*This is a sample transcript demonstrating the format for technical discussions.*

## Question

**User**: How do I build a simple REST API with Node.js and Express?

## Response

**AI Assistant**: I'll guide you through creating a simple REST API using Node.js and Express. Here's a step-by-step approach:

### 1. Project Setup

First, initialize your project and install dependencies:

```bash
mkdir my-api
cd my-api
npm init -y
npm install express
npm install -D nodemon
```

### 2. Basic Server Structure

Create an `index.js` file:

```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.json({ message: 'Welcome to the API' });
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### 3. Creating RESTful Routes

Add CRUD operations for a resource (e.g., "users"):

```javascript
// In-memory data store (replace with database in production)
let users = [
  { id: 1, name: 'John Doe', email: 'john@example.com' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
];

// GET all users
app.get('/api/users', (req, res) => {
  res.json(users);
});

// GET single user
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).json({ error: 'User not found' });
  res.json(user);
});

// POST new user
app.post('/api/users', (req, res) => {
  const newUser = {
    id: users.length + 1,
    name: req.body.name,
    email: req.body.email
  };
  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT update user
app.put('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).json({ error: 'User not found' });
  
  user.name = req.body.name || user.name;
  user.email = req.body.email || user.email;
  res.json(user);
});

// DELETE user
app.delete('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ error: 'User not found' });
  
  users.splice(index, 1);
  res.status(204).send();
});
```

### 4. Error Handling

Add error handling middleware:

```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});
```

### 5. Testing Your API

You can test your API using curl or a tool like Postman:

```bash
# GET all users
curl http://localhost:3000/api/users

# POST new user
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Bob Wilson","email":"bob@example.com"}'

# UPDATE user
curl -X PUT http://localhost:3000/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Updated"}'

# DELETE user
curl -X DELETE http://localhost:3000/api/users/1
```

## Key Takeaways

- REST APIs follow standard HTTP methods (GET, POST, PUT, DELETE)
- Express makes it easy to create routes and handle requests
- Always include error handling and validation
- Use proper HTTP status codes
- Consider adding authentication, rate limiting, and input validation for production

---

*This transcript demonstrates a typical technical discussion format with code examples and explanations.*