# Track 2: CLI & Git Mastery

**Timeline:** Weeks 3-4 (self-paced)  
**Goal:** Be fast and confident in the terminal, and use Git like a professional developer

## Why This Matters

In EECS 498 AASE, you'll live in the terminal. You'll run AI coding agents from the command line, manage multi-file projects with Git, collaborate through pull requests, and debug by reading logs and diffs. Students who fumble with basic shell commands or panic at merge conflicts lose hours every week. This track eliminates that friction.

You probably learned some terminal and Git in EECS 201. This goes further — from "I can use it" to "I'm fast in it."

## Skills Checklist

By the end of this track, you should be comfortable with:

- [ ] **Shell basics** — navigating directories, file manipulation, permissions, `chmod`, `chown`
- [ ] **Piping and redirection** — `|`, `>`, `>>`, `<`, `2>&1`, `tee`, chaining commands
- [ ] **Text processing** — `grep`, `sed`, `awk`, `sort`, `uniq`, `wc`, `cut`, `jq` for JSON
- [ ] **Environment variables** — `export`, `.bashrc`/`.zshrc`, `$PATH`, `.env` files
- [ ] **Process management** — `ps`, `kill`, `bg`, `fg`, `&`, `jobs`, `nohup`
- [ ] **SSH keys** — generating keys, `~/.ssh/config`, agent forwarding, connecting to remote hosts
- [ ] **Git branching** — create, switch, delete branches; understand `HEAD`, detached HEAD
- [ ] **Git rebase** — interactive rebase, squashing commits, rebase vs merge
- [ ] **Merge conflicts** — identify, resolve, and verify conflict resolution
- [ ] **Git history** — `git log --oneline --graph`, `git diff`, `git blame`, `git bisect`
- [ ] **GitHub CLI (`gh`)** — create PRs, check CI status, review PRs from the terminal
- [ ] **Multiple remotes** — `origin` vs `upstream`, forking workflow, keeping forks in sync

## Resources

### Terminal Fluency
1. **[The Missing Semester — Shell Tools](https://missing.csail.mit.edu/2020/shell-tools/)** — Best single resource for leveling up shell skills
2. **[The Missing Semester — Command-line Environment](https://missing.csail.mit.edu/2020/command-line/)** — Job control, aliases, dotfiles, SSH
3. **`jq` tutorial:** [stedolan.github.io/jq/tutorial](https://stedolan.github.io/jq/tutorial/) — You'll use `jq` constantly to inspect JSON (LLM responses, config files, agent state)

### Git Beyond Basics
1. **[Pro Git Book — Chapter 3: Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)** — Free online, covers everything
2. **[Atlassian Git Rebase Tutorial](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)** — Clear explanation of rebase vs merge
3. **[Oh Shit, Git!?!](https://ohshitgit.com/)** — How to undo mistakes. Bookmark this.
4. **[GitHub CLI docs](https://cli.github.com/manual/)** — Install `gh` and learn the basics

### Practice Exercises

#### Exercise 1: Terminal Gauntlet
Do all of these without leaving the terminal or opening a GUI:

```bash
# 1. Find all Python files in a project that contain "import asyncio"
# 2. Count total lines of code across all .py files in a directory
# 3. Extract all unique import statements from a Python project, sorted alphabetically
# 4. Watch a log file in real-time, filtering for lines containing "ERROR"
# 5. Set up an alias in your .zshrc/.bashrc that you'll actually use
# 6. Create a simple shell script that takes a directory as argument
#    and reports: number of files, total size, most recently modified file
```

Don't just run these once — understand *why* each command works. Pipe `man grep` and actually read the flags you use.

#### Exercise 2: Dev Environment Setup
Set up a proper development environment from scratch:

1. **SSH keys:** Generate an Ed25519 key pair, add to GitHub, configure `~/.ssh/config`
2. **GitHub CLI:** Install `gh`, authenticate, try `gh repo list`, `gh pr list`
3. **Shell config:** Set up useful aliases and environment variables in your `.zshrc`/`.bashrc`:
   ```bash
   alias gs="git status"
   alias gl="git log --oneline --graph --all"
   alias gd="git diff"
   export EDITOR="code --wait"  # or vim, whatever you prefer
   ```
4. **Verify it works:** Clone a repo via SSH (not HTTPS), make a change, push it

#### Exercise 3: Realistic Git Workflow
Practice the exact workflow you'll use in the course. Create a test repo and do this:

1. **Create a feature branch** from `main`
   ```bash
   git checkout -b feature/add-config-parser
   ```
2. **Make 4-5 commits** with intentionally messy messages (like you would in real development)
3. **Interactive rebase** to clean up history — squash related commits, rewrite messages
   ```bash
   git rebase -i HEAD~5
   ```
4. **Simulate a conflict:** Make changes on `main` that overlap with your branch, then rebase your branch onto `main` and resolve the conflicts
5. **Push and create a PR** using `gh`:
   ```bash
   git push origin feature/add-config-parser
   gh pr create --title "Add config parser" --body "Implements JSON config loading"
   ```
6. **Merge the PR** and clean up:
   ```bash
   gh pr merge --squash
   git checkout main && git pull
   git branch -d feature/add-config-parser
   ```

Run through this loop 3-4 times until it's muscle memory. In the course, you'll do this daily.

#### Exercise 4: Fork and Upstream Workflow
This is how you'll work on shared course repositories:

1. Fork a public repo on GitHub (pick any open-source project)
2. Clone your fork, add the original as `upstream`:
   ```bash
   git remote add upstream https://github.com/original-owner/repo.git
   git fetch upstream
   ```
3. Create a branch, make a change, push to your fork
4. Sync your fork with upstream:
   ```bash
   git fetch upstream
   git rebase upstream/main
   git push origin main
   ```

## Self-Check

Try these without looking anything up:

1. Find all files larger than 1MB in your home directory, sorted by size, using a single pipeline
2. Resolve a Git merge conflict — create one deliberately if you need to, then fix it
3. Use `git rebase -i` to squash 3 commits into 1 with a clean message
4. Create a PR from the terminal using `gh`, check its status, then merge it
5. Explain the difference between `git merge` and `git rebase` to a classmate (out loud, not by reading a definition)

If these feel routine, you're ready. If merge conflicts still make you nervous, practice Exercise 3 until they don't — you'll encounter them weekly in the course.
