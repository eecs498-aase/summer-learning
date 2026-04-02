# Track 2: Terminal & Git Mastery

**Timeline:** Weeks 3-4 (self-paced)
**Goal:** Be fast and confident in the terminal, and use Git like a professional developer

## Why This Matters

You'll live in the terminal all semester. You'll manage multi-file projects with Git, collaborate through pull requests, and debug by reading logs and diffs. Students who fumble with basic shell commands or panic at merge conflicts lose hours every week. This track eliminates that friction.

You probably learned some terminal and Git in EECS 201. This goes further — from "I can use it" to "I'm fast in it."

## Shell Skills Checklist

- [ ] **Navigation & files** — `cd`, `ls`, `mkdir -p`, `cp -r`, `mv`, `find`, `chmod`, `chown`
- [ ] **Piping and redirection** — `|`, `>`, `>>`, `<`, `2>&1`, `tee`, chaining commands
- [ ] **Text processing** — `grep`, `sed`, `awk`, `sort`, `uniq`, `wc`, `cut`, `tr`
- [ ] **JSON on the command line** — `jq` for parsing, filtering, transforming JSON
- [ ] **Environment variables** — `export`, `.bashrc`/`.zshrc`, `$PATH`, `.env` files, `source`
- [ ] **Process management** — `ps`, `kill`, `bg`, `fg`, `&`, `jobs`, `nohup`
- [ ] **Scripting** — writing bash/zsh scripts, `if`/`for`/`while`, exit codes, `set -e`
- [ ] **SSH** — generating keys, `~/.ssh/config`, agent forwarding, connecting to remote hosts

## Git Skills Checklist

- [ ] **Branching** — create, switch, delete branches; understand `HEAD`, detached HEAD
- [ ] **Rebase** — interactive rebase, squashing commits, rebase vs merge
- [ ] **Merge conflicts** — identify, resolve, verify conflict resolution
- [ ] **History** — `git log --oneline --graph`, `git diff`, `git blame`, `git bisect`
- [ ] **Stash** — `git stash`, `git stash pop`, `git stash list`, named stashes
- [ ] **GitHub CLI (`gh`)** — create PRs, check CI status, review PRs from the terminal
- [ ] **Multiple remotes** — `origin` vs `upstream`, forking workflow, keeping forks in sync

## Resources

### Terminal Fluency
1. **[The Missing Semester — Shell Tools](https://missing.csail.mit.edu/2020/shell-tools/)** — Best single resource for leveling up shell skills
2. **[The Missing Semester — Command-line Environment](https://missing.csail.mit.edu/2020/command-line/)** — Job control, aliases, dotfiles, SSH
3. **`jq` tutorial:** [stedolan.github.io/jq/tutorial](https://stedolan.github.io/jq/tutorial/) — You'll use `jq` constantly to inspect JSON data

### Git Beyond Basics
1. **[Pro Git Book — Chapter 3: Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)** — Free online, covers everything
2. **[Atlassian Git Rebase Tutorial](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)** — Clear explanation of rebase vs merge
3. **[Oh Shit, Git!?!](https://ohshitgit.com/)** — How to undo mistakes. Bookmark this.
4. **[GitHub CLI docs](https://cli.github.com/manual/)** — Install `gh` and learn the basics

## Practice Exercises

### Exercise 1: Shell Gauntlet
Do all of these without leaving the terminal or opening a GUI:

```bash
# 1. Find all Python files in a project that contain "import asyncio"
find . -name "*.py" -exec grep -l "import asyncio" {} \;

# 2. Count total lines of code across all .py files in a directory
find . -name "*.py" | xargs wc -l | tail -1

# 3. Extract all unique import statements from a Python project, sorted
grep -rh "^import\|^from" --include="*.py" . | sort -u

# 4. Watch a log file in real-time, filtering for ERROR lines
tail -f app.log | grep --line-buffered "ERROR"

# 5. Create a simple shell script that takes a directory as argument
#    and reports: number of files, total size, most recently modified file
```

Don't just run these once — understand *why* each command works. Read `man grep` and actually learn the flags you use.

### Exercise 2: Write a Real Shell Script
Create a script called `project-stats.sh` that:

```bash
#!/bin/bash
set -euo pipefail

# Usage: ./project-stats.sh <directory> [extension]
# Default extension: all files
#
# Output:
# - Total files
# - Total lines of code
# - Largest file (by lines)
# - Most recently modified file
# - Top 5 most common file extensions
```

Requirements:
- Handle missing arguments with a usage message
- Use `set -euo pipefail` for safety
- Use functions to organize the logic
- Handle edge cases: empty directory, no matching files, permission errors

### Exercise 3: Dev Environment Setup
Set up a proper development environment from scratch:

1. **SSH keys:** Generate an Ed25519 key pair, add to GitHub, configure `~/.ssh/config`
2. **GitHub CLI:** Install `gh`, authenticate, try `gh repo list`, `gh pr list`
3. **Shell config:** Set up useful aliases in your `.zshrc`/`.bashrc`:
   ```bash
   alias gs="git status"
   alias gl="git log --oneline --graph --all"
   alias gd="git diff"
   alias gco="git checkout"
   export EDITOR="code --wait"  # or vim
   ```
4. **Verify:** Clone a repo via SSH (not HTTPS), make a change, push it

### Exercise 4: Git Workflow — Branch, Rebase, PR
Practice the workflow you'll use daily. Create a test repo and do this:

1. **Create a feature branch** from `main`:
   ```bash
   git checkout -b feature/add-config-parser
   ```
2. **Make 4-5 commits** with intentionally messy messages (like real development)
3. **Interactive rebase** to clean up history:
   ```bash
   git rebase -i HEAD~5
   ```
   Squash related commits, rewrite messages to be clear and descriptive.
4. **Simulate a conflict:** Make overlapping changes on `main`, then rebase your branch:
   ```bash
   git checkout main
   # Make a conflicting edit to the same file
   git commit -am "Change on main"
   git checkout feature/add-config-parser
   git rebase main
   # Resolve the conflict, then: git rebase --continue
   ```
5. **Push and create a PR** using `gh`:
   ```bash
   git push origin feature/add-config-parser
   gh pr create --title "Add config parser" --body "Implements JSON config loading"
   ```
6. **Merge and clean up:**
   ```bash
   gh pr merge --squash
   git checkout main && git pull
   git branch -d feature/add-config-parser
   ```

Run through this loop 3-4 times until it's muscle memory.

### Exercise 5: Fork and Upstream Workflow
Practice working with forked repositories:

1. Fork a public repo on GitHub (pick any open-source project)
2. Clone your fork, add the original as `upstream`:
   ```bash
   git clone git@github.com:YOU/repo.git
   cd repo
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

### Exercise 6: Git Archaeology
Pick any large open-source repo and practice these investigative skills:

```bash
# Find who last modified a specific line
git blame src/main.py

# Find when a specific function was introduced
git log -S "def process_task" --oneline

# Find all commits that touched a specific file in the last month
git log --since="1 month ago" --oneline -- src/config.py

# Use bisect to find which commit introduced a bug
git bisect start
git bisect bad HEAD
git bisect good v1.0
# ... test each commit git bisect suggests
```

## Self-Check

Try these without looking anything up:

1. Find all files larger than 1MB in your home directory, sorted by size, using a single pipeline
2. Write a bash script that takes a filename and prints its line count, word count, and character count
3. Resolve a Git merge conflict — create one deliberately, then fix it cleanly
4. Use `git rebase -i` to squash 3 commits into 1 with a clean message
5. Create a PR from the terminal using `gh`, check its status, then merge it
6. Explain the difference between `git merge` and `git rebase` out loud (not by reading a definition)
7. Use `git stash` to save work, switch branches, do something, switch back, and restore your stash

If these feel routine, you're ready. If merge conflicts still make you nervous, practice Exercise 4 until they don't.
