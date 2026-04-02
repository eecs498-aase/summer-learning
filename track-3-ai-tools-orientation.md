# Track 3: AI Tools Orientation

**Timeline:** Weeks 5-6 (self-paced)  
**Goal:** Understand how AI coding tools work, get hands-on with Aider, and make your first raw LLM API call

## Why This Matters

Phase 1 of EECS 498 AASE drops you into AI-assisted development on Day 1. You'll use Aider — an open-source AI coding agent — to build real software. If you've never used an AI coding tool beyond ChatGPT, you'll spend your first week figuring out the tool instead of learning the concepts.

This track gives you a head start. You'll install Aider, understand the basics of how LLMs work (enough to use them well, not to train them), and make a raw API call so you understand what's happening under the hood.

**Budget note:** You'll need an API key for this track. $5-10 is plenty for all the exercises. Don't go wild — Phase 1 covers cost management in detail.

## Skills Checklist

By the end of this track, you should be comfortable with:

- [ ] **What AI coding tools do** — code generation, editing, review, and their limitations
- [ ] **Aider basics** — install, configure, add files, make edits, review changes
- [ ] **Tokens and context windows** — what they are, why they matter, how to estimate cost
- [ ] **Model selection** — differences between GPT-4o, Claude Sonnet, etc., and when to pick which
- [ ] **The Context-Model-Prompt framework** — how to think about every AI interaction
- [ ] **Raw API calls** — send a request to OpenAI or Anthropic, parse the response
- [ ] **Request/response format** — messages, roles, system prompts, temperature, max_tokens

## Core Concepts

### How AI Coding Tools Work (High Level)
AI coding tools are wrappers around Large Language Models (LLMs). Here's what actually happens when you ask Aider to "add error handling to this function":

1. **Context gathering** — The tool reads relevant files, git history, your prompt
2. **Prompt construction** — It builds a structured prompt with system instructions, code context, and your request
3. **API call** — It sends that prompt to an LLM (like GPT-4o or Claude Sonnet)
4. **Response parsing** — The LLM returns text (usually a diff or code block), and the tool applies it to your files
5. **Verification** — The tool may run linters, tests, or ask you to review

That's it. No magic. Every AI coding tool — Aider, Cursor, Copilot, Windsurf — follows this same loop. Understanding it is the difference between using these tools effectively and using them blindly.

### The Context-Model-Prompt Framework
This is the mental model we use throughout the course. Every time you interact with an LLM, three things determine the output quality:

- **Context** — What information does the model have? (files, docs, conversation history)
- **Model** — Which LLM are you using? (capability, cost, speed tradeoffs)
- **Prompt** — What exactly are you asking? (specificity, structure, constraints)

Bad results almost always trace back to one of these three being wrong. Phase 1 teaches this systematically — for now, just know the framework exists.

### Tokens, Context Windows, and Cost
- **Tokens** ≈ word fragments. "Hello world" is 2 tokens. A typical Python or TypeScript file is 200-500 tokens. A full project might be 50,000+.
- **Context window** = maximum tokens the model can process in one request (input + output). GPT-4o: 128K. Claude Sonnet: 200K. Bigger isn't always better — cost scales with usage.
- **Cost** = (input tokens × input price) + (output tokens × output price). A typical Aider edit costs $0.01-0.10. A complex multi-file change might cost $0.50-2.00.

You don't need to memorize pricing tables. You *do* need the intuition that sending your entire codebase as context for a one-line fix is wasteful.

## Resources

### Understanding LLMs (Just Enough)
1. **[Andrej Karpathy — Intro to LLMs (1hr video)](https://www.youtube.com/watch?v=zjkBMFhNj_g)** — Best single explanation for engineers. Watch this.
2. **[Anthropic — Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)** — How to write effective prompts (directly applicable to the course)

### Aider
1. **[Aider documentation](https://aider.chat/)** — Install guide, usage, configuration
2. **[Aider GitHub](https://github.com/Aider-AI/aider)** — Source code (you'll read agent source code in Phase 2, so get used to this)

### LLM APIs
1. **[OpenAI API Quickstart](https://platform.openai.com/docs/quickstart)** — Create account, get API key, make first call
2. **[Anthropic API Getting Started](https://docs.anthropic.com/en/api/getting-started)** — Same for Claude models

## Practice Exercises

### Exercise 1: Install and Configure Aider
Get Aider running with a real API key:

```bash
# Install Aider
pip install aider-chat

# Set your API key (get one from platform.openai.com or console.anthropic.com)
export OPENAI_API_KEY="sk-..."
# or
export ANTHROPIC_API_KEY="sk-ant-..."

# Navigate to any small Python or TypeScript project (or create one)
cd ~/my-test-project
aider
```

Try these interactions:
1. Ask Aider to create a new file (Python or TypeScript) with a specific function
2. Ask it to add type hints to existing code
3. Ask it to write tests for a function
4. Ask it to refactor something — rename a function, extract a class
5. Review each change with `git diff` before accepting

Pay attention to *what Aider sends to the model* (use `--verbose` flag) and *how it applies the response*. This demystifies the tool.

### Exercise 2: Build a Small Project with Aider
Use Aider to build something from scratch. Pick one:

- **A Markdown link checker** — reads a `.md` file, finds all URLs, checks if they return 200
- **A simple expense tracker CLI** — add expenses, list by category, show totals
- **A git commit message analyzer** — reads git log, reports stats on message length, frequency, etc.

Rules:
- Start from an empty directory
- Use Aider for *all* code writing — don't type code manually
- Practice giving clear, specific instructions
- Notice when Aider gets confused and think about *why* (usually a context or prompt issue)

### Exercise 3: Make a Raw LLM API Call
Understand what's happening beneath tools like Aider. Write a script (Python or TypeScript) that calls an LLM API directly:

```python
import httpx

# OpenAI example
response = httpx.post(
    "https://api.openai.com/v1/chat/completions",
    headers={
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json",
    },
    json={
        "model": "gpt-4o-mini",
        "messages": [
            {"role": "system", "content": "You are a helpful coding assistant."},
            {"role": "user", "content": "Write a Python function that checks if a string is a palindrome."},
        ],
        "temperature": 0.0,
        "max_tokens": 500,
    },
)

result = response.json()
print(result["choices"][0]["message"]["content"])
```

Then extend it:
1. Try the same prompt with `temperature=0.0` vs `temperature=1.0` — what changes?
2. Add a multi-turn conversation (include previous assistant responses in the messages list)
3. Try the Anthropic API — the format is different (`messages` API with `role: "user"/"assistant"`)
4. Print the token usage from the response — how many input/output tokens did it use?

This exercise removes the magic. When Aider sends a request, *this is what it's doing* — just with more context and better prompts.

## Self-Check

Try these without looking anything up:

1. Start Aider in a project, add specific files to context, and make a targeted edit
2. Explain the Context-Model-Prompt framework — what are the three levers, and what happens when one is wrong?
3. Estimate the token count for a 200-line source file (ballpark is fine)
4. Make a raw API call to OpenAI or Anthropic using `httpx` and parse the response
5. Explain why sending your entire codebase as context for every request is a bad idea (hint: it's not just cost)

If these feel solid, you're ahead of the curve for Phase 1. If the API call feels shaky, redo Exercise 3 — understanding the raw API is foundational to everything in this course.
