# Track 1: Python & TypeScript Fluency

**Timeline:** Weeks 1-2 (self-paced)
**Goal:** Write clean, modern code in both Python 3.11+ and TypeScript with confidence

## Why This Matters

Course projects use both Python and TypeScript. We assume fluency in both on Day 1. If either language is rusty or new to you, this track gets you there. If you're strong in both, skip ahead — but try the self-check first to be sure.

## Python Skills Checklist

- [ ] **Type hints** — function signatures, `Optional`, `Union`, generics, `TypedDict`
- [ ] **Data classes** — `@dataclass`, field defaults, `__post_init__`, frozen classes
- [ ] **Pathlib** — `Path` objects, reading/writing files, directory traversal
- [ ] **JSON handling** — `json.loads`, `json.dumps`, custom encoders/decoders
- [ ] **HTTP requests** — `httpx` or `requests`, async HTTP, handling responses
- [ ] **Error handling** — custom exceptions, context managers, `try/except/finally`
- [ ] **Async basics** — `async/await`, `asyncio.gather`, when to use async vs sync
- [ ] **Testing** — `pytest`, fixtures, parametrize, mocking
- [ ] **Virtual environments** — `uv`, `venv`, `pip`, dependency management
- [ ] **f-strings and string formatting** — template strings, multiline strings

## TypeScript Skills Checklist

- [ ] **Types and interfaces** — primitives, union types, generics, `Record`, `Partial`, `Pick`
- [ ] **Async/await** — Promises, `async` functions, `Promise.all`, error handling
- [ ] **Node.js basics** — `fs/promises`, `path`, `process.env`, `fetch`
- [ ] **Package management** — `npm`/`pnpm`, `package.json`, `tsconfig.json`
- [ ] **JSON handling** — parsing, serialization, typed JSON responses
- [ ] **Testing** — Vitest or Jest basics
- [ ] **ES modules** — `import`/`export`, module resolution
- [ ] **Error handling** — try/catch, custom error classes, `Error` subclassing
- [ ] **Generics** — writing generic functions and classes, constraints with `extends`

## Resources

### Python
**If you're new to Python (from C++/Java):**
1. **[Python for Programmers](https://docs.python.org/3/tutorial/)** — Official tutorial, skip the basics, focus on chapters 5-12
2. **Practice:** Rewrite a small EECS 281 project in Python (e.g., a graph algorithm, a text processor)

**If you know Python but need modern features:**
1. **Type hints deep dive:** [mypy documentation](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)
2. **Dataclasses:** [Real Python Guide](https://realpython.com/python-data-classes/)
3. **Async Python:** [asyncio docs](https://docs.python.org/3/library/asyncio.html)

### TypeScript
1. **[TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)** — Start here if new to TS
2. **[Node.js + TypeScript](https://nodejs.org/en/learn/getting-started/nodejs-with-typescript)** — Official guide
3. **[TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)** — Free book, good for deeper understanding
4. **Practice:** After each Python exercise below, redo it in TypeScript

## Practice Exercises

Each exercise should be implemented **twice** — once in Python, once in TypeScript. This is deliberate: you internalize the patterns in both languages and start to see the parallels.

### Exercise 1: File-Based Key-Value Store
Build a persistent key-value store backed by JSON files.

**Python version:**
```python
from dataclasses import dataclass
from pathlib import Path

@dataclass
class FileStore:
    base_dir: Path

    def get(self, key: str) -> dict | None:
        """Read a value by key. Returns None if not found."""
        ...

    def set(self, key: str, value: dict) -> None:
        """Write a value. Creates file if not exists."""
        ...

    def delete(self, key: str) -> bool:
        """Delete a key. Returns True if existed."""
        ...

    def list_keys(self) -> list[str]:
        """List all keys in the store."""
        ...
```

**TypeScript version:**
```typescript
import { readFile, writeFile, unlink, readdir } from 'fs/promises';
import { join } from 'path';

class FileStore {
  constructor(private baseDir: string) {}

  async get(key: string): Promise<Record<string, unknown> | null> { ... }
  async set(key: string, value: Record<string, unknown>): Promise<void> { ... }
  async delete(key: string): Promise<boolean> { ... }
  async listKeys(): Promise<string[]> { ... }
}
```

Requirements:
- Each key maps to a `.json` file in the base directory
- Handle missing files gracefully (return `null`, not crash)
- Write tests: at least 5 test cases covering set, get, delete, list, and missing keys

### Exercise 2: Typed HTTP Client
Build a typed client for the GitHub REST API (no auth required for public repos).

**Python version:**
```python
@dataclass
class Repo:
    name: str
    full_name: str
    description: str | None
    stars: int
    language: str | None

class GitHubClient:
    def __init__(self, base_url: str = "https://api.github.com"):
        ...

    async def get_repo(self, owner: str, name: str) -> Repo:
        ...

    async def list_repos(self, owner: str) -> list[Repo]:
        ...
```

**TypeScript version:**
```typescript
interface Repo {
  name: string;
  fullName: string;
  description: string | null;
  stars: number;
  language: string | null;
}

class GitHubClient {
  constructor(private baseUrl: string = "https://api.github.com") {}

  async getRepo(owner: string, name: string): Promise<Repo> { ... }
  async listRepos(owner: string): Promise<Repo[]> { ... }
}
```

Requirements:
- Use `httpx` (Python) and `fetch` (TypeScript)
- Map the API response to your typed data structure (don't just return raw JSON)
- Handle HTTP errors: 404 → return null or throw typed error, 429 → log a warning
- Write tests using a mock/stub for the HTTP calls

### Exercise 3: CLI File Processor
Build a command-line tool that processes files in a directory.

```bash
# Python
python process.py scan ./my-project --ext .py --output report.json
python process.py stats report.json

# TypeScript
npx ts-node process.ts scan ./my-project --ext .ts --output report.json
npx ts-node process.ts stats report.json
```

The `scan` command should:
- Walk the directory recursively
- Find all files matching the extension
- For each file: count lines, count blank lines, count comment lines
- Write results as JSON

The `stats` command should:
- Read the JSON report
- Print summary: total files, total lines, average lines per file, largest file

Requirements:
- Use `argparse` (Python) or `process.argv` parsing (TypeScript) — no external CLI libraries
- Use `pathlib` (Python) and `fs/path` (TypeScript) for file operations
- Handle errors: missing directory, no matching files, malformed JSON

## Self-Check

Try these without looking anything up:

1. Write a dataclass (Python) and an interface (TypeScript) with the same shape — include optional fields and a `fromJSON` factory
2. Read all `.json` files from a directory using `pathlib` (Python) and `fs/promises` (TypeScript)
3. Make an async HTTP GET request, handle errors, and parse the JSON response — in both languages
4. Write a `pytest` test with `@pytest.mark.parametrize` and a Vitest/Jest test with `test.each`
5. Explain when you'd use `asyncio.gather` (Python) vs `Promise.all` (TypeScript) — and when you wouldn't

If all five feel natural in **both** languages, you're ready. If one language is shaky, spend more time there — it pays off all semester.
