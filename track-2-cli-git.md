# Track 2: CLI & Git Mastery

**Timeline:** Weeks 3-4 (self-paced)  
**Goal:** Be fast and comfortable in the terminal with Git workflows

## Why This Matters

You'll spend most of this course in a terminal. AI coding tools like Aider run in the CLI. Agent systems read and write files. Git is how you submit everything. If you're slow in the terminal, you'll be slow in the course.

## Skills Checklist

### Terminal Fundamentals
- [ ] **Navigation** — `cd`, `ls`, `pwd`, `find`, `tree`, tab completion
- [ ] **File operations** — `cat`, `head`, `tail`, `grep`, `wc`, `diff`
- [ ] **Pipes and redirection** — `|`, `>`, `>>`, `<`, `2>&1`
- [ ] **Process management** — `&`, `fg`, `bg`, `Ctrl+C`, `Ctrl+Z`, `jobs`
- [ ] **Environment variables** — `export`, `.env` files, `$PATH`
- [ ] **SSH** — key generation, `~/.ssh/config`, connecting to remote machines
- [ ] **Text editors** — comfortable in at least one of: vim, nano, or VS Code terminal

### Git Proficiency
- [ ] **Core workflow** — `add`, `commit`, `push`, `pull`, `status`, `log`, `diff`
- [ ] **Branching** — `branch`, `checkout`, `merge`, `rebase`
- [ ] **Collaboration** — pull requests, code review, merge conflicts
- [ ] **History** — `log --oneline --graph`, `blame`, `show`, `reflog`
- [ ] **Undoing things** — `reset`, `revert`, `stash`, `checkout -- file`
- [ ] **GitHub** — repos, issues, PRs, organizations, SSH keys

## Resources

### Terminal
1. **[The Missing Semester (MIT)](https://missing.csail.mit.edu/)** — Lectures 1-4 are excellent
2. **[explainshell.com](https://explainshell.com/)** — Paste any command to understand it
3. **Practice:** Do everything from the terminal for a week. No Finder/Explorer.

### Git
1. **[Learn Git Branching](https://learngitbranching.js.org/)** — Interactive visual tutorial (do all levels)
2. **[Oh Shit, Git!?!](https://ohshitgit.com/)** — How to fix common Git mistakes
3. **[GitHub Skills](https://skills.github.com/)** — Interactive GitHub courses

## Practice Exercises

#### Exercise 1: Git Workflow Simulation
Create a repo and practice this workflow 5 times until it's muscle memory:
```bash
# 1. Create a feature branch
git checkout -b feature/my-feature

# 2. Make changes, commit with good messages
git add -p  # stage selectively
git commit -m "feat: add X because Y"

# 3. Rebase on latest main before PR
git fetch origin
git rebase origin/main

# 4. Push and create PR
git push origin feature/my-feature
gh pr create --title "Add X" --body "Description"
```

#### Exercise 2: Command-Line File Processing
Without using Python, process a directory of log files:
```bash
# Count lines in all .log files
find . -name "*.log" | xargs wc -l

# Find all files containing "ERROR" 
grep -r "ERROR" --include="*.log" -l

# Extract timestamps from errors and sort uniquely
grep "ERROR" *.log | cut -d' ' -f1-2 | sort -u

# Watch a file for changes in real-time
tail -f app.log | grep --line-buffered "ERROR"
```

#### Exercise 3: Shell Scripting Basics
Write a bash script that:
1. Takes a directory as an argument
2. Finds all Python files
3. Counts total lines of code (excluding blank lines and comments)
4. Outputs a summary

```bash
#!/bin/bash
# Usage: ./count_python.sh ./my-project
DIR=${1:-.}
echo "Python LOC in $DIR:"
find "$DIR" -name "*.py" -exec grep -v '^\s*$\|^\s*#' {} + | wc -l
```

## Self-Check

Without looking anything up:
1. Create a repo, make 3 commits on a branch, rebase onto main, push
2. Resolve a merge conflict (create one intentionally)
3. Find all files larger than 1MB modified in the last 7 days
4. Write a one-liner that counts unique words in a file, sorted by frequency

If all four feel natural, you're good. Git especially — you'll use it every single day.
