# Track 4: Systems Thinking

**Timeline:** Weeks 7-8 (self-paced)  
**Goal:** Understand how multi-component systems communicate, and build your first simple "agent"

## Why This Matters

Phases 2 and 3 of EECS 498 AASE are about building agent systems — programs that use LLMs as reasoning engines, call tools, manage state, and coordinate with other programs. This isn't a single-file homework problem. It's software architecture.

If you've never thought about how two programs talk to each other, or how to design a system where components communicate through structured data, Phases 2-3 will feel overwhelming. This track builds the mental models you need.

## Skills Checklist

By the end of this track, you should be comfortable with:

- [ ] **Agents vs tools vs workflows** — what makes something an "agent" and why it matters
- [ ] **Inter-process communication** — stdin/stdout, files, HTTP, message queues
- [ ] **JSON as a protocol** — structured data exchange, schemas, validation
- [ ] **HTTP/REST basics** — requests, responses, methods, status codes, headers
- [ ] **System architecture reading** — looking at a multi-component system and understanding data flow
- [ ] **Building a simple agent loop** — read task → call LLM → write result

## Core Concepts

### What Is an "Agent"?

The word "agent" gets thrown around loosely. Here's a working taxonomy:

- **Chatbot** — Takes user input, returns LLM output. No tools, no memory, no autonomy. (ChatGPT in basic mode.)
- **Tool** — A single-purpose program. Does one thing, does it well. `grep` is a tool. A function that calls an API is a tool.
- **Workflow** — A fixed sequence of steps. "Run linter → run tests → deploy." No decisions, no branching based on results.
- **Agent** — A program that *makes decisions* about what to do next. It has a goal, it can use tools, and it decides which tools to call (and when to stop) based on the results it gets back.

The key distinction: **agents have a decision loop.** They observe, think, act, and repeat until done. A workflow runs the same steps every time. An agent might take a completely different path depending on what it encounters.

In this course, you'll build agents that read code, decide what to change, make edits, run tests, interpret failures, and try again. That loop — observe, reason, act — is the core pattern.

### How Programs Communicate

In EECS 281, your programs were self-contained. In real systems (and in this course), programs talk to each other. Here are the main channels:

**stdin/stdout (pipes)** — The simplest form. One program's output feeds into another's input.
```bash
cat tasks.json | python process_task.py | python apply_changes.py
```
Fast, simple, but limited to linear pipelines. You've used this in shell scripting.

**Files** — Programs read and write shared files. Simple, durable, but you need to handle concurrency (what if two programs write at the same time?).
```
Agent writes plan.json → Executor reads plan.json → Executor writes result.json → Agent reads result.json
```
This is how many agent frameworks manage state. You'll use this pattern in Phase 2.

**HTTP/REST** — Programs communicate over the network using HTTP requests and responses. This is how web APIs work, how LLM APIs work, and how most modern agent systems are structured.
```
Agent → POST /api/chat/completions → LLM Service → Response with generated text
```

**Message queues** — Programs put messages on a queue; other programs pick them up asynchronously. More complex, but handles high volume and decoupling. (We'll touch this in Phase 3.)

### JSON as a Protocol

When programs exchange structured data, JSON is the lingua franca. Every LLM API speaks JSON. Every agent framework uses JSON for configuration, state, and communication.

You need to be fluent in reading and writing JSON, not just syntactically but *as a design tool*. When you design an agent, one of the first questions is: "What does the message format look like?"

```json
{
  "task": {
    "id": "task-001",
    "type": "code_review",
    "target": "src/auth.py",
    "instructions": "Check for SQL injection vulnerabilities"
  },
  "context": {
    "repo": "my-app",
    "branch": "feature/login",
    "related_files": ["src/db.py", "src/models.py"]
  }
}
```

This is a task description an agent might consume. It's structured, unambiguous, and machine-parseable. Designing these schemas is a real skill you'll practice in the course.

### HTTP/REST Basics

You've already made HTTP calls in Track 3 (the LLM API exercise). Here's the conceptual framework:

- **Request** = method + URL + headers + body. `POST https://api.openai.com/v1/chat/completions` with a JSON body.
- **Response** = status code + headers + body. `200 OK` with a JSON body containing the LLM's output.
- **Methods:** `GET` (read), `POST` (create), `PUT` (replace), `PATCH` (update), `DELETE` (remove)
- **Status codes:** `200` (OK), `201` (created), `400` (bad request), `401` (unauthorized), `404` (not found), `429` (rate limited), `500` (server error)
- **Headers:** Metadata about the request/response. `Authorization` for API keys, `Content-Type` for format.

When your agent calls an LLM, it's making an HTTP POST. When it calls a tool via an API, same thing. When another system triggers your agent, it might send an HTTP request to your agent's server. HTTP is the connective tissue.

## Resources

1. **[OpenClaw Architecture Overview](https://docs.openclaw.ai)** — Read the architecture section. OpenClaw is a real agent orchestration platform — understanding its design teaches you how production agent systems work. (You'll use it in Phase 3.)
2. **[The Missing Semester — Data Wrangling](https://missing.csail.mit.edu/2020/data-wrangling/)** — Piping, text processing, structured data from the command line
3. **[MDN — HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)** — Clear, concise explanation of HTTP fundamentals
4. **[JSON.org](https://www.json.org/)** — The spec is one page. Read it once.
5. **[Anthropic — What is an AI Agent?](https://www.anthropic.com/research/building-effective-agents)** — Excellent overview of agent patterns from the team behind Claude

## Practice Exercises

### Exercise 1: Build a Simple Agent

Build a program that follows the agent loop: read task → reason → act → write result.

```python
"""
Simple agent that:
1. Reads a task from tasks/pending/<id>.json
2. Sends the task to an LLM with appropriate context
3. Writes the result to tasks/completed/<id>.json
4. Handles errors gracefully
"""
import json
from pathlib import Path
import httpx

def load_next_task(pending_dir: Path) -> dict | None:
    """Find and load the next pending task."""
    ...

def build_prompt(task: dict) -> list[dict]:
    """Construct the messages array for the LLM API call."""
    ...

def call_llm(messages: list[dict]) -> str:
    """Make a raw API call and return the response text."""
    ...

def save_result(task: dict, result: str, completed_dir: Path) -> None:
    """Write the completed task with its result."""
    ...

def run_agent():
    """Main agent loop."""
    pending = Path("tasks/pending")
    completed = Path("tasks/completed")
    
    while True:
        task = load_next_task(pending)
        if task is None:
            print("No more tasks. Done.")
            break
        
        print(f"Processing task: {task['id']}")
        messages = build_prompt(task)
        result = call_llm(messages)
        save_result(task, result, completed)
        print(f"Completed task: {task['id']}")
```

Create a few sample task files:
```json
{
  "id": "task-001",
  "type": "summarize",
  "content": "Paste a paragraph of text here",
  "instructions": "Summarize this in 2-3 sentences."
}
```

This is the simplest possible agent — no tool use, no branching, no memory. But it's the skeleton that every more complex agent builds on.

### Exercise 2: Add Error Handling and Retries

Extend your agent from Exercise 1:
- What happens when the API returns a 429 (rate limit)? Implement exponential backoff.
- What happens when the API returns malformed JSON? Catch it, log it, move on.
- What happens when a task file is malformed? Skip it, write an error report.
- Add a `tasks/failed/` directory for tasks that couldn't be completed after 3 retries.

Real agents fail constantly. The difference between a toy and a production agent is error handling.

### Exercise 3: Read the OpenClaw Architecture

Read the [OpenClaw documentation](https://docs.openclaw.ai), focusing on the architecture overview. Then write a 1-page summary (300-500 words) answering:

1. What are the main components of OpenClaw and what does each one do?
2. How do the components communicate with each other?
3. Where do LLM calls happen in the architecture?
4. How does OpenClaw handle multiple agents or tools?
5. What design patterns do you recognize from this track?

Writing forces understanding. If you can explain the architecture clearly, you understand it. If you can't, reread.

## Self-Check

Try these without looking anything up:

1. Explain the difference between an agent and a workflow — give an example of each
2. Describe three ways programs can communicate with each other, with a tradeoff for each
3. Design a JSON schema for a "code review task" that an agent would consume — what fields do you need?
4. Given a `curl` command that makes an API call, translate it to Python using `httpx`
5. Explain what happens (step by step) when your agent from Exercise 1 processes a task

If you can do all five, you've built the mental models for Phases 2 and 3. If the architecture summary (Exercise 3) was hard to write, spend more time with the OpenClaw docs — understanding existing systems is how you learn to build new ones.
