# Track 3: AI Tools Orientation

**Timeline:** Weeks 5-6 (self-paced)  
**Goal:** Get comfortable with AI coding tools before the course starts

## Why This Matters

Phase 1 starts with Aider on Day 1. If this is your first time using an AI coding tool beyond ChatGPT, you'll spend the first week fighting setup instead of learning principles. This track gets the fumbling out of the way early.

## Skills Checklist

- [ ] **LLM API basics** — Understand API keys, tokens, pricing, rate limits
- [ ] **Aider setup** — Install, configure, connect to an LLM provider
- [ ] **Basic Aider workflow** — `/add` files, make edits, review diffs, `/undo`
- [ ] **ChatGPT/Claude** — Effective prompting for code generation and explanation
- [ ] **Context management** — Why context matters and how to manage it
- [ ] **Cost awareness** — How to estimate and control API spending

## Important Note

⚠️ **Don't develop bad habits.** The point of Phase 1 is to teach you *principled* AI coding. It's okay to explore now, but be ready to unlearn sloppy prompting habits. In the course, you'll learn the Context-Model-Prompt framework that makes AI coding repeatable and reliable.

## Resources

### Understanding LLMs (Conceptual)
1. **[What Is ChatGPT Doing... and Why Does It Work?](https://writings.stephenwolfram.com/2023/02/what-is-chatgpt-doing-and-why-does-it-work/)** — Stephen Wolfram's accessible explanation
2. **[Anthropic's Claude documentation](https://docs.anthropic.com/)** — Read the "Prompt engineering" section
3. **[OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)** — Official best practices

### Aider
1. **[Aider documentation](https://aider.chat/docs/)** — Installation and getting started
2. **[Aider tutorial videos](https://aider.chat/docs/usage/tutorials.html)** — Watch the basics
3. **[IndyDevDan: Principled AI Coding](https://www.youtube.com/@indydevdan)** — YouTube series that inspired Phase 1

### API Key Setup
You'll need access to at least one LLM provider. Options:
- **Anthropic Claude** — Strong coding model, reasonable pricing
- **OpenAI GPT** — Widely used, extensive documentation  
- **Google Gemini** — Free tier available
- **Local models via Ollama** — Free, private, good for experimentation

**Budget tip:** For summer exploration, start with a free tier or set a $5-10 spending limit. The course will provide guidance on cost-effective model selection in Week 1.

## Practice Exercises

#### Exercise 1: Set Up Aider
```bash
# Install Aider
pip install aider-chat

# Set up your API key (choose one provider)
export ANTHROPIC_API_KEY=your-key-here
# or
export OPENAI_API_KEY=your-key-here

# Start Aider in a repo
cd your-project
aider
```

Try these basic commands:
- `/add filename.py` — add a file to context
- Ask Aider to make a change in natural language
- Review the diff it proposes
- `/undo` if you don't like it
- `/tokens` to see context usage

#### Exercise 2: Build Something Small with Aider
Pick a small project (~100-300 lines) and build it entirely with Aider:
- A Markdown-to-HTML converter
- A simple HTTP API with Flask/FastAPI
- A file organizer script
- A quiz game

**Document your process:** What prompts worked? What didn't? When did you need to be more specific? This self-awareness is exactly what Phase 1 teaches formally.

#### Exercise 3: Compare Models
Try the same coding task with different models (or different prompting styles):
- Ask a vague question vs. a specific one
- Provide no context vs. full file context
- Use a cheap model vs. an expensive one

Notice the differences. The Context-Model-Prompt framework in Phase 1 will give you vocabulary for what you're observing.

## Self-Check

1. You can install and run Aider connected to an LLM provider
2. You've built at least one small project with AI assistance
3. You have a rough sense of what makes a good vs. bad prompt
4. You know approximately how much API calls cost

Don't worry about mastery — Phase 1 teaches the principled approach. The goal here is familiarity so you're not fighting tools on Day 1.
