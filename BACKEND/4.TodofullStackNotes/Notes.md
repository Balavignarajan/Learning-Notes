# Todo App: React + Express + MongoDB — Beginner Friendly (for Bala)

**Coach:** Hello Bala 👋 — this is your step-by-step roadmap and starter project for building a full Todo website using **React (frontend)**, **Express (backend)**, and **MongoDB (database)**. I wrote this slowly and simply — as if I’m standing next to you. Treat this document like your study notebook. Work one small step at a time and check off exercises.

---

## What you will learn (big picture)

* How frontend (React) and backend (Express) talk using HTTP (GET, POST, PUT, DELETE).
* How to store data in MongoDB using Mongoose.
* How to build CRUD features: Create, Read, Update, Delete (the heart of many apps).
* How to run the app locally and test APIs with curl/Postman.

**Memory trick for CRUD:** **C**reate, **R**ead, **U**pdate, **D**elete — say aloud: *"Create, Read, Update, Delete — like putting things in, reading them, changing them, then throwing them away."*

---

## Project map (simple)

```
/todo-project
  /todo-backend    <-- Express + Mongoose
    .env
    package.json
    index.js
    /models
      Todo.js
    /routes
      todos.js
  /todo-frontend   <-- React (Vite)
    .env
    package.json
    src/
      main.jsx
      App.jsx
      /components
        TodoForm.jsx
        TodoList.jsx
        TodoItem.jsx
      api.js
```

**Feature list (start small):**

1. Show all todos (Read)
2. Add a todo (Create)
3. Mark todo done/undone (Update)
4. Edit todo text (Update)
5. Delete a todo (Delete)

We'll build the smallest working backend first, then the frontend to talk to it.

---

## Tools you need (and commands)

* Node.js & npm (install from nodejs.org). Check: `node -v` and `npm -v`.
* A code editor (VS Code recommended).
* Git (optional but recommended): `git --version`.
* MongoDB: use **MongoDB Atlas** (cloud) or install local MongoDB. Atlas is easier for beginners.
* Postman or curl for testing APIs.

---

## Tiny plan — one small step at a time (do 1 step, practice, then continue)

* **Step 0:** Install Node and create project folders.
* **Step 1:** Build backend server (Express) and make one working endpoint (GET /api/todos).
* **Step 2:** Add Mongoose model and connect to MongoDB.
* **Step 3:** Implement all CRUD backend routes and test with curl/Postman.
* **Step 4:** Bootstrap React frontend and show list of todos.
* **Step 5:** Add ability to create a todo from the frontend.
* **Step 6:** Add update (toggle complete + edit title) and delete from frontend.
* **Step 7+:** Polish UI, add styles, validations, and deploy if you want.

---

## STEP-BY-STEP: Backend (Express + Mongoose)

### 1) Create backend folder and init

Open terminal and run:

```bash
mkdir todo-project && cd todo-project
mkdir todo-backend && cd todo-backend
npm init -y
npm i express mongoose cors dotenv
npm i -D nodemon
```

Add script in `package.json` (inside `todo-backend`):

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
}
```

### 2) Create `index.js` (server entry)

Create file `index.js` and paste:

```js
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json()); // parse JSON body

// Simple health route
app.get('/', (req, res) => res.send('Todo API is running'));

// Mount routes (we'll create this file next)
const todosRouter = require('./routes/todos');
app.use('/api/todos', todosRouter);

const PORT = process.env.PORT || 5000;

mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    console.log('Connected to MongoDB');
    app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
  })
  .catch(err => {
    console.error('MongoDB connection error:', err.message);
  });
```

Create `.env` in `todo-backend` with:

```
MONGO_URI=your_mongo_connection_string_here
PORT=5000
```

(If using Atlas, paste the connection string and replace the password and DB name as needed.)

**Memory tip for .env:** Think of `.env` as a locked box with secret keys for your app.

### 3) Create Mongoose model `models/Todo.js`

```js
const mongoose = require('mongoose');

const TodoSchema = new mongoose.Schema({
  title: { type: String, required: true },
  completed: { type: Boolean, default: false },
}, { timestamps: true });

module.exports = mongoose.model('Todo', TodoSchema);
```

### 4) Create routes `routes/todos.js`

```js
const express = require('express');
const router = express.Router();
const Todo = require('../models/Todo');

// GET /api/todos - list all
router.get('/', async (req, res) => {
  try {
    const todos = await Todo.find().sort({ createdAt: -1 });
    res.json(todos);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// POST /api/todos - create
router.post('/', async (req, res) => {
  try {
    const { title } = req.body;
    if (!title) return res.status(400).json({ error: 'Title required' });
    const todo = new Todo({ title });
    await todo.save();
    res.status(201).json(todo);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// PUT /api/todos/:id - update
router.put('/:id', async (req, res) => {
  try {
    const { id } = req.params;
    const updates = req.body; // e.g. { completed: true } or { title: 'new' }
    const todo = await Todo.findByIdAndUpdate(id, updates, { new: true });
    res.json(todo);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// DELETE /api/todos/:id - delete
router.delete('/:id', async (req, res) => {
  try {
    const { id } = req.params;
    await Todo.findByIdAndDelete(id);
    res.json({ message: 'Deleted' });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

module.exports = router;
```

### 5) Test backend quickly

Start server:

```bash
npm run dev
```

In another terminal, test:

```bash
curl http://localhost:5000/
# test GET todos
curl http://localhost:5000/api/todos
# create
curl -X POST -H "Content-Type: application/json" -d '{"title":"Buy milk"}' http://localhost:5000/api/todos
```

If these return results, backend is good.

---

## STEP-BY-STEP: Frontend (React with Vite)

### 1) Create frontend project

(open a new terminal in `todo-project` root)

```bash
cd todo-project
npm create vite@latest todo-frontend -- --template react
cd todo-frontend
npm install
npm run dev
```

Vite dev server will start (default port 5173). Open the URL shown in terminal.

### 2) Add an env variable for API base URL

Create `.env` in `todo-frontend`:

```
VITE_API_URL=http://localhost:5000
```

(Vite requires `VITE_` prefix.)

### 3) Simple API helper `src/api.js`

Create `src/api.js`:

```js
const BASE = import.meta.env.VITE_API_URL || 'http://localhost:5000';

export async function fetchTodos() {
  const res = await fetch(`${BASE}/api/todos`);
  return await res.json();
}

export async function createTodo(title) {
  const res = await fetch(`${BASE}/api/todos`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ title }),
  });
  return await res.json();
}

export async function updateTodo(id, updates) {
  const res = await fetch(`${BASE}/api/todos/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(updates),
  });
  return await res.json();
}

export async function deleteTodo(id) {
  const res = await fetch(`${BASE}/api/todos/${id}`, { method: 'DELETE' });
  return await res.json();
}
```

### 4) Main App `src/App.jsx` (simple and clear)

Replace `App.jsx` with:

```jsx
import React, { useState, useEffect } from 'react';
import { fetchTodos, createTodo, updateTodo, deleteTodo } from './api';
import TodoForm from './components/TodoForm';
import TodoList from './components/TodoList';

export default function App() {
  const [todos, setTodos] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // runs once after component mounts
    (async () => {
      const data = await fetchTodos();
      setTodos(data);
      setLoading(false);
    })();
  }, []);

  async function handleAdd(title) {
    const newTodo = await createTodo(title);
    setTodos(prev => [newTodo, ...prev]);
  }

  async function handleToggle(todo) {
    const updated = await updateTodo(todo._id, { completed: !todo.completed });
    setTodos(prev => prev.map(t => (t._id === updated._id ? updated : t)));
  }

  async function handleDelete(id) {
    await deleteTodo(id);
    setTodos(prev => prev.filter(t => t._id !== id));
  }

  return (
    <div style={{ maxWidth: 600, margin: '30px auto', padding: 20 }}>
      <h1>Todo App (Bala's practice)</h1>
      <TodoForm onAdd={handleAdd} />
      {loading ? <p>Loading...</p> : <TodoList todos={todos} onToggle={handleToggle} onDelete={handleDelete} />}
    </div>
  );
}
```

### 5) `src/components/TodoForm.jsx` (add todos)

```jsx
import React, { useState } from 'react';

export default function TodoForm({ onAdd }) {
  const [title, setTitle] = useState('');

  function submit(e) {
    e.preventDefault();
    if (!title.trim()) return;
    onAdd(title.trim());
    setTitle('');
  }

  return (
    <form onSubmit={submit} style={{ display: 'flex', gap: 8 }}>
      <input value={title} onChange={e => setTitle(e.target.value)} placeholder="What to do?" />
      <button type="submit">Add</button>
    </form>
  );
}
```

### 6) `src/components/TodoList.jsx` and `TodoItem.jsx`

`TodoList.jsx`:

```jsx
import React from 'react';
import TodoItem from './TodoItem';

export default function TodoList({ todos, onToggle, onDelete }) {
  if (todos.length === 0) return <p>No todos yet — add one!</p>;
  return (
    <ul style={{ listStyle: 'none', padding: 0 }}>
      {todos.map(todo => (
        <TodoItem key={todo._id} todo={todo} onToggle={onToggle} onDelete={onDelete} />
      ))}
    </ul>
  );
}
```

`TodoItem.jsx`:

```jsx
import React from 'react';

export default function TodoItem({ todo, onToggle, onDelete }) {
  return (
    <li style={{ display: 'flex', justifyContent: 'space-between', padding: '8px 0' }}>
      <div>
        <input type="checkbox" checked={todo.completed} onChange={() => onToggle(todo)} />
        <span style={{ marginLeft: 8, textDecoration: todo.completed ? 'line-through' : 'none' }}>{todo.title}</span>
      </div>
      <button onClick={() => onDelete(todo._id)}>Delete</button>
    </li>
  );
}
```

### 7) Run both servers

* Start backend (from `/todo-backend`): `npm run dev` (runs on port 5000)
* Start frontend (from `/todo-frontend`): `npm run dev` (Vite runs on 5173)

Open the Vite URL and try adding, toggling, and deleting.

---

## Simple explanations for beginners (plain words)

* **React**: a tool for building the part of the app the user sees. Think of it as building blocks.
* **Express**: a simple HTTP server that answers browsers and apps with data.
* **MongoDB**: a place (database) to save your todos.
* **Mongoose**: a helper that lets Node talk to MongoDB easily.
* **HTTP verbs**: GET = read, POST = create, PUT = update, DELETE = delete.

**Hook memory tricks:**

* `useState` — *state remembers things*. Imagine a post-it note that reappears when React draws the screen.
* `useEffect` — *do this after render*. Like after you cook, you wash the dishes.

---

## Troubleshooting / Common errors

* **CORS errors**: The frontend tries to call backend but browser blocks it. Fix: make sure `app.use(cors())` is in backend `index.js`.
* **MongoDB connection error**: Check `.env` MONGO\_URI and allow your IP in Atlas network access (or set IP to 0.0.0.0/0 for testing).
* **Fetch 404 / wrong URL**: Confirm `VITE_API_URL` and that backend is running on that port.
* **`Cannot read property` or `undefined`**: Probably data shape different than expected — add console.log to see API response.

---

## Practice plan for a slow learner (Bala) — short sessions

**Day 1 (30–60m):** Set up environment, create folders, start backend and ensure `GET /` returns "Todo API is running".
**Day 2 (45–90m):** Add model and POST/GET routes; test using curl/Postman.
**Day 3 (45–90m):** Start frontend scaffold (Vite), show static todos in the UI.
**Day 4 (45–90m):** Hook frontend to backend to fetch real todos.
**Day 5 (45–90m):** Add create todo from UI.
**Day 6 (45–90m):** Add update (toggle complete) and delete.
**Day 7+:** Polish, add edit UI, localStorage caching, or deploy.

**Tip:** Keep a notebook — write the command you ran and one sentence about what it did.

---

## Small exercises (do one by one)

1. From backend: Create 3 todos using curl.
2. From frontend: show local static array of todos (hard-coded) — this makes you confident with React rendering.
3. Connect frontend to backend and show server todos.
4. Add a todo from UI and see it appear in DB (check Atlas or backend logs).
5. Toggle a todo and check in DB that `completed` changed.

At each step, stop and say: "I did it" or share error messages — and I will help.

---

## Next steps if you want more

* Add user authentication (optional later).
* Add filters (show only active or completed todos).
* Save UI theme or user's name to personalize.
* Deploy backend (Heroku/Render) and frontend (Vercel/Netlify) — I can guide you.

---

## Final motivational note (for Bala)

You're doing the right thing by learning step-by-step. Focus on tiny wins — finish one small step and celebrate it. When stuck, copy the exact error message and ask me — I will explain slowly and clearly. You're not alone.

---

If you want, I can now:

* Walk with you live through **Step 1** (I will tell you exact commands and what to type), OR
* Paste full ready-to-run code for all files so you can copy-paste and run locally (I can do that next).

Tell me which you prefer and I will act like your coach. (Say: **Start Step 1** or **Show all files**.)
