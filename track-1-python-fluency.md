# Track 1: Python Fluency

**Timeline:** Weeks 1-2 (self-paced)  
**Goal:** Be comfortable writing Python 3.11+ code with modern features

## Why This Matters

Every project in EECS 498 AASE is Python. We assume you can write clean, well-structured Python code on Day 1. If your Python is rusty or you learned in another language, this track gets you up to speed.

## Skills Checklist

By the end of this track, you should be comfortable with:

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

## Resources

### If you're new to Python (from C++/Java)
1. **[Python for Programmers](https://docs.python.org/3/tutorial/)** — Official tutorial, skip the basics, focus on chapters 5-12
2. **Practice:** Rewrite a small EECS 281 project in Python (e.g., a graph algorithm, a text processor)

### If you know Python but need modern features
1. **Type hints deep dive:** [mypy documentation](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)
2. **Dataclasses:** [Real Python Guide](https://realpython.com/python-data-classes/)
3. **Async Python:** [asyncio docs](https://docs.python.org/3/library/asyncio.html)

### Practice Exercises

#### Exercise 1: File-Based State Manager
Build a simple key-value store backed by JSON files:
```python
class FileStore:
    """A persistent key-value store using JSON files."""
    
    def __init__(self, base_dir: Path):
        ...
    
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
This pattern shows up directly in Phase 2 (file-based agent state).

#### Exercise 2: HTTP API Client
Build a typed client for any public API (e.g., GitHub API, weather API):
```python
@dataclass
class Repo:
    name: str
    full_name: str
    description: str | None
    stars: int
    language: str | None

class GitHubClient:
    def __init__(self, token: str | None = None):
        ...
    
    async def get_repo(self, owner: str, name: str) -> Repo:
        ...
    
    async def list_repos(self, owner: str) -> list[Repo]:
        ...
```
This pattern shows up in Phase 2/3 (LLM API integration, tool execution).

#### Exercise 3: Simple CLI Tool
Build a command-line tool with `argparse` that processes files:
```bash
python process.py scan ./my-project --ext .py --output report.json
python process.py stats report.json
```
You'll build CLI tools throughout the course.

## Self-Check

Try this without looking anything up:
1. Write a dataclass with type hints and a `from_json` classmethod
2. Read all `.json` files from a directory using `pathlib`
3. Make an async HTTP request and parse the JSON response
4. Write a pytest test with parametrize for edge cases

If all four feel natural, you're ready. If not, spend more time here — it pays off all semester.
