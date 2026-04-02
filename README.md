# EECS 498-016: Summer Preparation Plan

> Get ready for Applied Agentic Software Engineering — Fall 2026

## Overview

The course moves fast — Phase 1 starts with AI-assisted coding on Day 1. These materials help you arrive prepared so you can focus on building agent systems instead of fighting setup issues.

**Time commitment:** ~2-4 hours/week over 8 weeks, self-paced  
**Not graded. Not required. Strongly recommended.**

## Timeline

| Weeks | Track | Focus |
|-------|-------|-------|
| 1-2 | [Python & TypeScript Fluency](track-1-python-fluency.md) | Modern Python 3.11+ and TypeScript for everything we build |
| 3-4 | [CLI & Git Mastery](track-2-cli-git-mastery.md) | Terminal productivity and professional Git workflows |
| 5-6 | [AI Tools Orientation](track-3-ai-tools-orientation.md) | Aider, LLM APIs, and your first AI-assisted project |
| 7-8 | [Systems Thinking](track-4-systems-thinking.md) | Multi-component architectures and building your first agent |

## The Four Tracks

### Track 1: Python & TypeScript Fluency (Weeks 1-2)
Projects use both Python and TypeScript. We assume you can write clean, well-structured code in both on Day 1. This track covers Python 3.11+ (type hints, dataclasses, async/await, pathlib, pytest) and TypeScript (types, interfaces, async/await, Node.js basics). You'll build the same exercises in both languages — patterns that show up directly in the course projects.

### Track 2: CLI & Git Mastery (Weeks 3-4)
You'll live in the terminal all semester. This track takes you beyond EECS 201 basics into real productivity: piping and text processing, environment variables, process management, SSH key setup, and `jq` for JSON. On the Git side, you'll practice branching strategies, interactive rebase, merge conflict resolution, `git log/diff/blame`, the GitHub CLI (`gh`), and fork/upstream workflows. The goal is muscle memory — you should be able to branch, commit, rebase, and PR without thinking.

### Track 3: AI Tools Orientation (Weeks 5-6)
Phase 1 uses Aider (an open-source AI coding agent) from Day 1. This track gets you ahead: install Aider, understand what AI coding tools actually do under the hood (context gathering → prompt construction → API call → response parsing), and learn the Context-Model-Prompt framework that guides every AI interaction in the course. You'll also make raw LLM API calls with Python so you understand what's happening beneath the abstractions — tokens, context windows, model selection, cost, and the request/response format.

### Track 4: Systems Thinking (Weeks 7-8)
Phases 2 and 3 are about building agent systems — programs that reason, use tools, and coordinate with other programs. This track builds the mental models: what makes something an "agent" vs a tool or workflow, how programs communicate (stdin/stdout, files, HTTP, message queues), JSON as a protocol for structured data exchange, and HTTP/REST fundamentals. You'll build a simple agent that reads tasks from JSON files, calls an LLM API, and writes results back. You'll also read the OpenClaw architecture docs and write a summary — understanding real agent platforms is how you learn to build your own.

## How to Use This

- **Already strong in Python and TypeScript?** Skip Track 1, start at Track 2.
- **Comfortable in the terminal?** Skip Track 2.
- **Already use AI coding tools?** Skim Track 3, focus on Track 4.
- **Coming from EECS 281 with solid C++?** Prioritize Tracks 1 (both languages) and 4.

Each track has:
- A skills checklist (self-assess before diving in)
- Curated resources (not a textbook — just the good stuff)
- Practice exercises (build real things, not toy examples)
- A self-check (can you do these without looking anything up?)

## Prerequisites Refresher

This plan assumes you've completed **EECS 281** and **EECS 201** (or equivalent). If you're rusty on:
- **Data structures & algorithms** → Review EECS 281 materials, especially graphs and trees
- **Linux/CLI basics** → Review EECS 201 materials or [The Missing Semester](https://missing.csail.mit.edu/)

## What NOT to Do

- ❌ Don't try to learn everything before class starts
- ❌ Don't spend money on API keys beyond $5-10 for exploration
- ❌ Don't develop AI coding habits you'll have to unlearn — Phase 1 teaches the principled approach
- ❌ Don't stress — this is preparation, not prerequisite

## Questions?

- 🌐 [Course Website](https://eecs498-aase.github.io/applied-agentic-software-engineering/)
- 📧 Contact: mmdarden@umich.edu or rsymonds@umich.edu
