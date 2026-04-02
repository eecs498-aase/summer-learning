# Track 3: Building & Deploying Software

**Timeline:** Weeks 5-6 (self-paced)
**Goal:** Know how to structure, build, containerize, and ship real software projects

## Why This Matters

Writing code is only half the job. Professional software needs to be organized, built, tested automatically, and deployed somewhere people (or other programs) can use it. If you've only ever written single-file homework assignments or projects that run on your laptop, this track bridges the gap to real-world software engineering.

You'll use all of these skills in the course: structuring multi-component projects, writing Makefiles and build scripts, containerizing services, and setting up CI pipelines that catch bugs before they hit main.

## Skills Checklist

- [ ] **Project structure** — organizing code into packages, separating config from code, monorepo basics
- [ ] **Config files** — `pyproject.toml`, `package.json`, `tsconfig.json`, `.env` files, `.gitignore`
- [ ] **Make/Makefile** — targets, dependencies, variables, phony targets, common patterns
- [ ] **npm scripts** — `package.json` scripts, chaining commands, pre/post hooks
- [ ] **Python build tools** — `uv`, `pip`, `setuptools`, `pyproject.toml`, editable installs
- [ ] **Docker basics** — Dockerfile, images, containers, `docker build`, `docker run`
- [ ] **docker-compose** — multi-container apps, services, volumes, networking
- [ ] **CI/CD concepts** — what it is, why it matters, how GitHub Actions works
- [ ] **GitHub Actions** — workflow files, triggers, jobs, steps, using actions
- [ ] **Deployment concepts** — local vs cloud, environment variables, ports, reverse proxies

## Core Concepts

### Project Structure

Real projects aren't one file. Here's what a well-organized project looks like:

```
my-project/
├── src/                    # Source code
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   └── utils/
│       ├── __init__.py
│       └── helpers.py
├── tests/                  # Tests mirror src/ structure
│   ├── test_main.py
│   └── test_helpers.py
├── Dockerfile              # Container definition
├── docker-compose.yml      # Multi-container orchestration
├── Makefile                # Build automation
├── pyproject.toml          # Python project metadata + dependencies
├── .github/
│   └── workflows/
│       └── ci.yml          # CI pipeline
├── .env.example            # Environment variable template
├── .gitignore              # What NOT to commit
└── README.md               # How to run this thing
```

Key principles:
- **Separate code from config.** Never hardcode API URLs, ports, or secrets. Use environment variables.
- **Tests live next to code.** Mirror the source directory structure.
- **Document how to run it.** A README with setup instructions isn't optional.
- **`.env.example` not `.env`.** Commit the template, not your actual secrets.

### Build Tools

**Makefile** — The universal build tool. Every project benefits from one:
```makefile
.PHONY: install test lint run clean

install:
	uv pip install -e ".[dev]"

test:
	pytest tests/ -v

lint:
	ruff check src/ tests/
	mypy src/

run:
	python -m src.main

clean:
	find . -type d -name __pycache__ -exec rm -rf {} +
	rm -rf .pytest_cache .mypy_cache
```

Anyone can clone the repo, run `make install && make test`, and know it works. No guessing.

**npm scripts** — The JavaScript/TypeScript equivalent:
```json
{
  "scripts": {
    "build": "tsc",
    "test": "vitest run",
    "lint": "eslint src/",
    "start": "node dist/main.js",
    "dev": "tsx watch src/main.ts"
  }
}
```

### Docker

Docker packages your application with its dependencies into a container — a lightweight, isolated environment that runs the same way everywhere.

**Why it matters:** "Works on my machine" is not acceptable. Docker guarantees that if it runs in the container, it runs in CI, on your teammate's laptop, and in production.

**Key concepts:**
- **Image** — A snapshot of an environment (OS, dependencies, your code). Built from a Dockerfile.
- **Container** — A running instance of an image. Isolated from your host system.
- **Volume** — Persistent storage that survives container restarts.
- **docker-compose** — Define and run multi-container applications (e.g., your app + a database).

A basic Dockerfile:
```dockerfile
FROM python:3.12-slim

WORKDIR /app
COPY pyproject.toml .
RUN pip install .
COPY src/ src/

CMD ["python", "-m", "src.main"]
```

### CI/CD with GitHub Actions

**CI (Continuous Integration):** Every time you push code, automated checks run — tests, linting, type checking. If any fail, the PR can't merge. This catches bugs before humans review code.

**CD (Continuous Deployment):** After CI passes and the PR merges, the code automatically deploys. We focus on CI in this track.

A basic GitHub Actions workflow:
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -e ".[dev]"
      - run: pytest tests/ -v
      - run: ruff check src/
```

### Deployment Concepts

"Deploying" means making your software available for use — whether that's a web server accessible on the internet, a CLI tool installed via pip, or a service running on a cloud VM.

Key concepts:
- **Ports:** Your app listens on a port (e.g., 8000). The outside world connects to that port.
- **Environment variables:** Configuration that changes between environments (dev/staging/prod). Never hardcode.
- **Local vs cloud:** Running on your laptop vs running on AWS/GCP/a VPS. Docker makes this transition smooth.
- **Process management:** Something needs to keep your app running, restart it if it crashes. (systemd, Docker restart policies, etc.)

## Resources

1. **[Docker Getting Started](https://docs.docker.com/get-started/)** — Official tutorial, hands-on
2. **[GitHub Actions Quickstart](https://docs.github.com/en/actions/quickstart)** — Your first workflow in 10 minutes
3. **[Makefile Tutorial](https://makefiletutorial.com/)** — Practical, example-driven
4. **[The Twelve-Factor App](https://12factor.net/)** — Principles for building deployable software (read factors I, III, V, and X at minimum)
5. **[uv documentation](https://docs.astral.sh/uv/)** — Modern Python package management

## Practice Exercises

### Exercise 1: Set Up a Proper Project
Create a new project from scratch with the full structure shown above:

1. Initialize a Git repo with a good `.gitignore`
2. Set up `pyproject.toml` with dependencies and dev dependencies
3. Write a `Makefile` with targets: `install`, `test`, `lint`, `run`, `clean`
4. Create a small app (e.g., a CLI that fetches weather data or processes CSV files)
5. Write at least 3 tests
6. Verify: `make install && make test && make lint` should all pass from a clean clone

Do the same for a TypeScript project:
1. `npm init`, configure `tsconfig.json`
2. Add scripts to `package.json`: `build`, `test`, `lint`, `start`
3. Write equivalent app and tests
4. Verify: `npm install && npm test && npm run lint` works from a clean clone

### Exercise 2: Dockerize It
Take the project from Exercise 1 and containerize it:

1. Write a `Dockerfile` that builds and runs your app
2. Build the image: `docker build -t my-app .`
3. Run the container: `docker run my-app`
4. Add a `docker-compose.yml` that runs your app
5. If your app needs external services (like a database), add them to compose:
   ```yaml
   services:
     app:
       build: .
       ports:
         - "8000:8000"
       environment:
         - DATABASE_URL=postgres://db:5432/mydb
     db:
       image: postgres:16
       environment:
         - POSTGRES_DB=mydb
         - POSTGRES_PASSWORD=devpass
   ```
6. Verify: `docker compose up` starts everything, and your app works

Bonus: Write a multi-stage Dockerfile that separates the build step from the runtime image (smaller final image).

### Exercise 3: Write a CI Pipeline
Set up GitHub Actions for your project:

1. Create `.github/workflows/ci.yml`
2. On every push and PR: install dependencies, run tests, run linter
3. Push to GitHub and verify the workflow runs (green checkmark on your commit)
4. Deliberately break a test and push — verify CI catches it (red X)
5. Add a step that runs type checking (`mypy` for Python, `tsc --noEmit` for TypeScript)

Bonus: Add a matrix strategy to test on multiple Python/Node versions:
```yaml
strategy:
  matrix:
    python-version: ["3.11", "3.12", "3.13"]
```

### Exercise 4: End-to-End
Combine everything: create a small web API (using FastAPI or Express), Dockerize it, add CI, and document it in a README that includes:

- What the project does
- How to run locally (with and without Docker)
- How to run tests
- Environment variables needed
- API endpoints (if applicable)

A stranger should be able to clone your repo and have it running in under 5 minutes.

## Self-Check

Try these without looking anything up:

1. Write a Makefile with three targets that have dependencies between them
2. Write a Dockerfile for a Python app — FROM, WORKDIR, COPY, RUN, CMD
3. Explain the difference between a Docker image and a container
4. Write a GitHub Actions workflow that runs tests on every PR
5. Explain what environment variables are for and why you shouldn't hardcode config values
6. Given a `docker-compose.yml`, explain what each section does (services, ports, volumes, environment)

If these feel solid, you're ready. If Docker is fuzzy, spend extra time on Exercise 2 — containerization is a skill you'll use for the rest of your career.
