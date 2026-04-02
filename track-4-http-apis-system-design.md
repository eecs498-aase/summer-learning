# Track 4: HTTP, APIs & System Design

**Timeline:** Weeks 7-8 (self-paced)
**Goal:** Understand how programs communicate over HTTP, build and consume REST APIs, and think in terms of multi-component systems

## Why This Matters

Modern software isn't one monolithic program — it's a collection of services that talk to each other. A web app calls a backend API. That backend calls a database and maybe a third-party service. Understanding how these pieces communicate is fundamental to building anything real.

In the course, you'll work with multi-component systems constantly. If you can't reason about HTTP requests, design an API endpoint, or debug why two services aren't connecting, you'll spend your time fighting plumbing instead of solving problems.

## Skills Checklist

- [ ] **HTTP fundamentals** — methods (GET, POST, PUT, DELETE), status codes, headers, request/response cycle
- [ ] **REST API design** — resources, endpoints, CRUD operations, JSON request/response bodies
- [ ] **Building an API** — simple server in Python (FastAPI) and TypeScript (Express)
- [ ] **Calling APIs** — `httpx` (Python), `fetch` (TypeScript), handling responses and errors
- [ ] **Authentication** — API keys, Bearer tokens, where they go in requests
- [ ] **Inter-process communication** — stdin/stdout, files, HTTP, sockets — how programs talk to each other
- [ ] **Debugging network issues** — `curl`, reading error messages, checking ports

## Core Concepts

### HTTP: The Language of the Web

Every time a program talks to another program over a network, it's (almost always) using HTTP. Here's what an HTTP exchange looks like:

**Request** = method + URL + headers + body
```
POST /api/tasks HTTP/1.1
Host: localhost:8000
Content-Type: application/json
Authorization: Bearer my-api-key-123

{"title": "Review PR", "priority": "high"}
```

**Response** = status code + headers + body
```
HTTP/1.1 201 Created
Content-Type: application/json

{"id": 42, "title": "Review PR", "priority": "high", "status": "pending"}
```

**Methods — what you want to do:**
| Method | Purpose | Example |
|--------|---------|---------|
| `GET` | Read data | `GET /api/tasks` — list all tasks |
| `POST` | Create data | `POST /api/tasks` — create a new task |
| `PUT` | Replace data | `PUT /api/tasks/42` — replace task 42 entirely |
| `PATCH` | Update data | `PATCH /api/tasks/42` — update specific fields |
| `DELETE` | Remove data | `DELETE /api/tasks/42` — delete task 42 |

**Status codes — what happened:**
- `200 OK` — Request succeeded
- `201 Created` — Resource was created
- `400 Bad Request` — Your request was malformed
- `401 Unauthorized` — Missing or bad authentication
- `403 Forbidden` — Authenticated but not allowed
- `404 Not Found` — Resource doesn't exist
- `429 Too Many Requests` — Rate limited, slow down
- `500 Internal Server Error` — Server broke

**Headers — metadata about the request/response:**
- `Content-Type: application/json` — The body is JSON
- `Authorization: Bearer <token>` — Authentication credentials
- `Accept: application/json` — I want JSON back

You don't need to memorize every status code. You *do* need the intuition to read an error response and know what went wrong.

### REST API Design

REST is a convention for organizing APIs around *resources* (things) and *actions* (HTTP methods). A well-designed REST API is predictable — if you know the pattern, you can guess the endpoints:

```
GET    /api/tasks          → List all tasks
POST   /api/tasks          → Create a new task
GET    /api/tasks/42       → Get task 42
PUT    /api/tasks/42       → Replace task 42
DELETE /api/tasks/42       → Delete task 42

GET    /api/tasks?status=pending    → Filter tasks
GET    /api/tasks?sort=priority     → Sort tasks
```

The resource is `tasks`. The methods define the action. The URL path identifies which resource. Query parameters filter or modify the request.

### Building an API

**Python (FastAPI):**
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

class Task(BaseModel):
    title: str
    priority: str = "medium"

class TaskResponse(BaseModel):
    id: int
    title: str
    priority: str
    status: str = "pending"

tasks: dict[int, TaskResponse] = {}
next_id = 1

@app.get("/api/tasks")
def list_tasks() -> list[TaskResponse]:
    return list(tasks.values())

@app.post("/api/tasks", status_code=201)
def create_task(task: Task) -> TaskResponse:
    global next_id
    response = TaskResponse(id=next_id, title=task.title, priority=task.priority)
    tasks[next_id] = response
    next_id += 1
    return response

@app.get("/api/tasks/{task_id}")
def get_task(task_id: int) -> TaskResponse:
    if task_id not in tasks:
        raise HTTPException(status_code=404, detail="Task not found")
    return tasks[task_id]
```

**TypeScript (Express):**
```typescript
import express from 'express';

const app = express();
app.use(express.json());

interface Task {
  id: number;
  title: string;
  priority: string;
  status: string;
}

const tasks: Map<number, Task> = new Map();
let nextId = 1;

app.get('/api/tasks', (req, res) => {
  res.json([...tasks.values()]);
});

app.post('/api/tasks', (req, res) => {
  const task: Task = {
    id: nextId++,
    title: req.body.title,
    priority: req.body.priority || 'medium',
    status: 'pending',
  };
  tasks.set(task.id, task);
  res.status(201).json(task);
});

app.get('/api/tasks/:id', (req, res) => {
  const task = tasks.get(parseInt(req.params.id));
  if (!task) return res.status(404).json({ detail: 'Task not found' });
  res.json(task);
});

app.listen(8000, () => console.log('Running on http://localhost:8000'));
```

### Calling APIs

**Python (httpx):**
```python
import httpx

async def create_task(title: str, priority: str = "medium") -> dict:
    async with httpx.AsyncClient() as client:
        response = await client.post(
            "http://localhost:8000/api/tasks",
            json={"title": title, "priority": priority},
            headers={"Authorization": "Bearer my-key"},
        )
        response.raise_for_status()
        return response.json()
```

**TypeScript (fetch):**
```typescript
async function createTask(title: string, priority = 'medium'): Promise<Task> {
  const response = await fetch('http://localhost:8000/api/tasks', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer my-key',
    },
    body: JSON.stringify({ title, priority }),
  });

  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  return response.json();
}
```

### How Programs Communicate

Beyond HTTP, programs have several ways to exchange data:

**stdin/stdout (pipes)** — The simplest form. One program's output feeds another's input:
```bash
cat data.json | python transform.py | python analyze.py > results.json
```
Fast and simple, but limited to linear pipelines.

**Files** — Programs read and write shared files. Simple and durable, but you need to handle concurrency:
```
Program A writes config.json → Program B reads config.json → Program B writes output.json
```

**HTTP** — Programs communicate over the network. This is how web APIs, microservices, and most modern systems work:
```
Client → POST /api/process → Server → Response with results
```

**Sockets** — Lower-level network communication. TCP sockets for reliable connections, WebSockets for persistent bidirectional channels. HTTP is built on TCP sockets — understanding this helps when debugging connection issues.

Each mechanism has tradeoffs:
| Mechanism | Speed | Complexity | Use Case |
|-----------|-------|------------|----------|
| stdin/stdout | Fast | Low | Chaining CLI tools |
| Files | Medium | Low | Shared state, config |
| HTTP | Medium | Medium | Service-to-service, APIs |
| Sockets | Fast | High | Real-time, persistent connections |

## Resources

1. **[MDN — HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)** — Clear, concise HTTP fundamentals
2. **[FastAPI Tutorial](https://fastapi.tiangolo.com/tutorial/)** — Build your first API in 30 minutes
3. **[Express Getting Started](https://expressjs.com/en/starter/hello-world.html)** — Same for TypeScript/Node
4. **[curl Manual](https://curl.se/docs/manual.html)** — The universal API testing tool
5. **[JSON.org](https://www.json.org/)** — The spec is one page. Read it once.
6. **[RESTful API Design Best Practices](https://restfulapi.net/)** — Conventions and patterns

## Practice Exercises

### Exercise 1: Build a REST API
Build a complete task management API in **both** Python (FastAPI) and TypeScript (Express):

Endpoints:
- `GET /api/tasks` — List all tasks (support `?status=` filter)
- `POST /api/tasks` — Create a task
- `GET /api/tasks/:id` — Get a specific task
- `PUT /api/tasks/:id` — Update a task
- `DELETE /api/tasks/:id` — Delete a task

Requirements:
- Proper status codes (201 for creation, 404 for not found, 400 for bad input)
- Input validation (title required, priority must be low/medium/high)
- JSON request and response bodies
- Test every endpoint with `curl`:
  ```bash
  # Create
  curl -X POST http://localhost:8000/api/tasks \
    -H "Content-Type: application/json" \
    -d '{"title": "Write tests", "priority": "high"}'

  # List
  curl http://localhost:8000/api/tasks

  # Get one
  curl http://localhost:8000/api/tasks/1

  # Delete
  curl -X DELETE http://localhost:8000/api/tasks/1
  ```

### Exercise 2: Build a Client
Write a client library (in both languages) that wraps your API from Exercise 1:

```python
class TaskClient:
    def __init__(self, base_url: str, api_key: str | None = None):
        ...

    async def list_tasks(self, status: str | None = None) -> list[Task]:
        ...

    async def create_task(self, title: str, priority: str = "medium") -> Task:
        ...

    async def get_task(self, task_id: int) -> Task | None:
        ...

    async def delete_task(self, task_id: int) -> bool:
        ...
```

Requirements:
- Typed responses (not raw dicts/objects)
- Error handling: 404 → return None, 400 → raise with message, 500 → raise
- Authentication header support
- Write tests using a mock HTTP server or by running the real server

### Exercise 3: Chain Two Services Together
Build two services that communicate with each other:

**Service A: Task API** (from Exercise 1)
**Service B: Notification Service** — receives notifications and logs them

The flow:
1. Client creates a task via Service A
2. Service A, after creating the task, sends a notification to Service B:
   ```json
   POST /api/notifications
   {"event": "task_created", "task_id": 42, "title": "Write tests"}
   ```
3. Service B logs the notification

Run both services simultaneously (different ports). This is the simplest possible multi-service architecture — two programs communicating over HTTP.

Bonus: Add error handling. What happens if Service B is down? Should Service A fail? Should it retry? Should it continue and log the failure? There's no single right answer, but you should have an opinion.

## Self-Check

Try these without looking anything up:

1. Explain what happens when you type a URL in your browser — the full HTTP request/response cycle
2. Design REST endpoints for a "bookmarks" resource — list, create, get, update, delete
3. Write a `curl` command that POSTs JSON to an API with an Authorization header
4. Build a simple API endpoint that accepts JSON input and returns JSON output — in either Python or TypeScript
5. Explain three ways programs communicate with each other, and when you'd choose each
6. Debug this: your client gets a 401 error calling your API. List 3 things to check.

If these feel solid, you have the foundation for building real multi-component systems. If building the API (Exercise 1) was a struggle, spend more time there — everything in modern software is APIs talking to APIs.
