# 🚀 Git & GitHub — Complete Guide (Beginner to Advanced)

> A complete, easy-to-understand reference covering everything from "What is Git?" to advanced workflows used by professional developers.

---

## 📚 Table of Contents

1. [What is Git?](#1-what-is-git)
2. [Git vs GitHub](#2-git-vs-github)
3. [Installation & Setup](#3-installation--setup)
4. [The .git Folder](#4-the-git-folder)
5. [File States in Git](#5-file-states-in-git)
6. [Core Git Workflow](#6-core-git-workflow)
7. [Branching](#7-branching)
8. [Merging](#8-merging)
9. [Stashing](#9-stashing)
10. [Working with GitHub (Remote)](#10-working-with-github-remote)
11. [Undoing Mistakes](#11-undoing-mistakes)
12. [Advanced Topics](#12-advanced-topics)
    - [git rebase](#121-git-rebase)
    - [git cherry-pick](#122-git-cherry-pick)
    - [git reflog](#123-git-reflog)
    - [git bisect](#124-git-bisect)
    - [git blame](#125-git-blame)
    - [git tag](#126-git-tag)
    - [git submodules](#127-git-submodules)
    - [git hooks](#128-git-hooks)
    - [git worktree](#129-git-worktree)
13. [GitHub Features](#13-github-features)
    - [Pull Requests](#131-pull-requests-prs)
    - [Issues](#132-issues)
    - [GitHub Actions (CI/CD)](#133-github-actions-cicd)
    - [GitHub Pages](#134-github-pages)
    - [Forks](#135-forks)
    - [GitHub CLI](#136-github-cli)
14. [Branching Strategies](#14-branching-strategies)
15. [.gitignore](#15-gitignore)
16. [Complete Command Reference](#16-complete-command-reference)

---

## 1. What is Git?

Git is a **version control system** — a tool that tracks every change you make to your files over time.

Think of it like this:

> 🎮 **Git is like save points in a video game.** Every time you commit, you save your progress. If you mess something up later, you can go back to any earlier save point.

**Without Git:**
- You'd manually save files like `project_v1.zip`, `project_v2_final.zip`, `project_FINAL_REAL.zip`
- Collaborating with teammates means emailing files back and forth
- One mistake can destroy hours of work

**With Git:**
- Every change is tracked automatically
- You can undo any mistake
- Multiple people can work on the same project simultaneously
- You always know *what* changed, *when*, and *who* changed it

```bash
# The most basic Git workflow
git init                    # Start tracking this folder
git add .                   # Stage all changes
git commit -m "My first commit"  # Save a snapshot
```

---

## 2. Git vs GitHub

These two are often confused — but they are completely different things.

| Feature | Git | GitHub |
|---|---|---|
| **What it is** | A tool/software | A website/platform |
| **Purpose** | Track code changes locally | Store & share code online |
| **Where it runs** | Your computer | The internet (cloud) |
| **Created by** | Linus Torvalds (2005) | Founded 2008, owned by Microsoft |
| **Needs internet?** | No | Yes |
| **Interface** | Command line | Web UI + collaboration tools |

**Simple analogy:**
> 📹 **Git** is like your video camera recording footage.
> 📺 **GitHub** is like YouTube where you upload and share that footage.

You can use Git without GitHub entirely. But GitHub makes it easy to back up your code online and collaborate with others.

---

## 3. Installation & Setup

### Install Git

- **Windows:** Download from [git-scm.com](https://git-scm.com)
- **Mac:** Run `xcode-select --install` or install via Homebrew: `brew install git`
- **Linux (Ubuntu/Debian):** `sudo apt install git`

### Verify Installation

```bash
git --version
# Output: git version 2.x.x
```

### First-Time Configuration

Before using Git, tell it who you are. This info gets attached to every commit you make.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set your preferred code editor (optional)
git config --global core.editor "code --wait"   # VS Code
git config --global core.editor "nano"          # Nano

# Check all your settings
git config --list
```

The `--global` flag means these settings apply to all projects on your computer. You can also set project-specific settings by running the same commands without `--global` inside a project folder.

---

## 4. The .git Folder

When you run `git init`, Git creates a hidden folder called `.git` inside your project.

```bash
git init
# → Initialized empty Git repository in /your-project/.git/
```

This `.git` folder is the **brain of Git**. It stores everything Git knows about your project. If you delete it, Git loses all history — but your actual files remain safe.

```
.git/
├── HEAD          → Points to the current branch you're on
├── config        → Project-specific Git settings
├── index         → The staging area (what's ready to commit)
├── objects/      → All commits, file snapshots stored here (Git's database)
├── refs/         → Pointers to branches and tags
└── logs/         → History of all your Git actions
```

### What Each Part Does

**`HEAD`** — A pointer that always says "you are here." Like a bookmark.
```
ref: refs/heads/main
# This means: you're currently on the "main" branch
```

**`objects/`** — Every file version and commit is stored here as a compressed object. This is the real database.

**`index`** — The staging area. When you `git add` a file, it goes here before being committed.

**`refs/`** — Contains files named after your branches, each holding the latest commit ID for that branch.

> ⚠️ **Never manually edit files inside `.git/`** — you could corrupt your repository.

---

## 5. File States in Git

Every file in your project is always in one of these states:

```
Untracked → Staged → Committed → Modified → Staged → New Commit
```

| State | Meaning | How to get here |
|---|---|---|
| **Untracked** | Git doesn't know this file exists | Create a new file |
| **Staged** | File is queued up for the next commit | Run `git add filename` |
| **Committed** | File is saved permanently in Git history | Run `git commit` |
| **Modified** | File was committed before, but has new changes | Edit a committed file |

### The Three Areas

```
Working Directory  →  Staging Area  →  Repository (.git)
  (your files)         (git add)        (git commit)
```

- **Working Directory:** The files you actually see and edit
- **Staging Area:** A "draft" zone — you decide exactly which changes go into the next commit
- **Repository:** Permanent, saved history inside `.git/objects/`

### Checking File States

```bash
git status
# Shows which files are untracked, modified, or staged
```

Example output:
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   index.html     ← staged

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
        modified:   style.css      ← modified

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        script.js                  ← untracked
```

---

## 6. Core Git Workflow

### Starting a Project

```bash
# Option 1: Start fresh
mkdir my-project
cd my-project
git init

# Option 2: Clone an existing project from GitHub
git clone https://github.com/username/repo-name.git
cd repo-name
```

### The Daily Workflow

```bash
# 1. Check what changed
git status

# 2. Stage specific files
git add index.html
git add style.css

# Or stage everything at once
git add .

# 3. See exactly what you're about to commit
git diff --staged

# 4. Commit with a descriptive message
git commit -m "Add navigation bar to homepage"
```

### Writing Good Commit Messages

Bad: `git commit -m "fix"`
Bad: `git commit -m "stuff"`
Good: `git commit -m "Fix login button not responding on mobile"`
Good: `git commit -m "Add user authentication with JWT tokens"`

> 💡 A good commit message completes the sentence: *"If applied, this commit will..."*

### Viewing History

```bash
# Full history
git log

# Compact one-line view
git log --oneline

# Graphical branch view
git log --oneline --graph --all

# See what changed in each commit
git log -p

# Search commits by message
git log --grep="login"

# Show a specific commit's details
git show abc1234
```

### Comparing Changes

```bash
# See unstaged changes (working directory vs staging area)
git diff

# See staged changes (staging area vs last commit)
git diff --staged

# Compare two branches
git diff main feature-branch

# Compare two commits
git diff abc1234 def5678
```

---

## 7. Branching

Branches let you work on new features or fixes **without touching the main code**.

> 🌳 **Think of branches like parallel universes.** The `main` branch is the "real" timeline. When you create a new branch, you create a separate universe where you can experiment freely. If your experiment succeeds, you merge it back into reality.

```
main:     A → B → C
                    \
feature:             D → E → F
```

### Branch Commands

```bash
# See all branches (* marks the current one)
git branch

# Create a new branch
git branch feature-login

# Switch to a branch
git switch feature-login
# (older way: git checkout feature-login)

# Create AND switch in one step
git switch -c feature-login
# (older way: git checkout -b feature-login)

# Rename a branch
git branch -m old-name new-name

# Delete a branch (only if it's been merged)
git branch -d feature-login

# Force delete (even if not merged)
git branch -D feature-login

# See all branches including remote ones
git branch -a
```

### Best Practice for Feature Branches

```bash
# 1. Start from an up-to-date main
git switch main
git pull

# 2. Create your feature branch
git switch -c feature/user-profile

# 3. Do your work, commit regularly
git add .
git commit -m "Add profile picture upload"

# 4. Merge back when done
git switch main
git merge feature/user-profile
```

---

## 8. Merging

Merging combines changes from one branch into another.

### Fast-Forward Merge

When the target branch has no new commits since the branch was created, Git simply moves the pointer forward — no merge commit needed.

```
Before:
main:    A → B
                \
feature:         C → D

After merge:
main:    A → B → C → D   (pointer just moved forward)
```

```bash
git switch main
git merge feature-branch
# "Fast-forward" shown in output
```

### Three-Way Merge

When both branches have new commits, Git creates a special **merge commit** that combines both histories.

```
Before:
main:    A → B → C
                  \
feature:   D → E

After:
main:    A → B → C → M   (M is the merge commit)
                  \ /
feature:   D → E
```

```bash
git switch main
git merge feature-branch
# Git opens your editor to write a merge commit message
```

### Merge Conflicts

A conflict happens when Git can't automatically decide which version of a change to keep (both branches edited the same line).

```bash
git merge feature-branch
# CONFLICT (content): Merge conflict in index.html
# Automatic merge failed; fix conflicts and then commit the result.
```

Open the conflicted file and you'll see:

```
<<<<<<< HEAD
<h1>Welcome to My Site</h1>       ← your version (current branch)
=======
<h1>Hello World</h1>              ← incoming version (branch being merged)
>>>>>>> feature-branch
```

**To resolve:**
1. Delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
2. Keep the version you want (or combine both)
3. Save the file

```bash
git add index.html        # Mark conflict as resolved
git commit                # Complete the merge
```

> 💡 Tools like VS Code have a visual conflict resolver — click "Accept Current Change", "Accept Incoming Change", or "Accept Both Changes"

---

## 9. Stashing

Stashing temporarily saves your work-in-progress without committing it. Useful when you need to quickly switch branches but aren't ready to commit.

> 🧳 **Stash is like a clipboard** — you temporarily set aside what you're working on, do something else, then come back and pick it up again.

```bash
# Save current changes to stash
git stash

# Save with a description
git stash push -m "half-finished login form"

# See all stashed items
git stash list
# stash@{0}: On main: half-finished login form
# stash@{1}: WIP on main: 3a4b5c6 previous commit

# Restore the most recent stash (and keep it in stash list)
git stash apply

# Restore a specific stash
git stash apply stash@{1}

# Restore AND remove from stash list
git stash pop

# Delete a stash without applying
git stash drop stash@{0}

# Delete ALL stashes
git stash clear
```

**Real-world example:**
```bash
# You're working on a new feature when your boss says "fix that bug NOW!"
git stash                    # Save your unfinished work
git switch hotfix-branch     # Switch to fix the bug
# ... fix the bug, commit ...
git switch feature-branch    # Come back to your feature
git stash pop                # Restore your work exactly where you left off
```

---

## 10. Working with GitHub (Remote)

A **remote** is a version of your repository stored online (on GitHub, GitLab, etc.).

### Connecting to GitHub

```bash
# Add a remote called "origin" (the standard name for your main remote)
git remote add origin https://github.com/username/repo.git

# See all configured remotes
git remote -v

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git

# Remove a remote
git remote remove origin
```

### Pushing and Pulling

```bash
# Push your local commits to GitHub
git push origin main

# Push a new branch to GitHub for the first time
git push -u origin feature-login
# The -u flag sets "origin feature-login" as the default upstream
# After this, you can just run: git push

# Download latest changes WITHOUT merging
git fetch origin

# Download latest changes AND merge them
git pull origin main
# (git pull = git fetch + git merge)
```

### Cloning vs Forking

**Clone:** Makes a copy of a repo on your computer. You still need permission to push to the original.

```bash
git clone https://github.com/username/repo.git
```

**Fork (GitHub only):** Makes a copy of someone else's repo under YOUR GitHub account. You can push freely and then submit a Pull Request to the original.

> Use **clone** for repos you have access to.
> Use **fork** for open-source repos you want to contribute to.

### Keeping Your Fork Updated

```bash
# Add the original repo as a remote called "upstream"
git remote add upstream https://github.com/original-owner/repo.git

# Fetch changes from the original
git fetch upstream

# Merge them into your local main
git merge upstream/main

# Push the updated main to YOUR fork
git push origin main
```

---

## 11. Undoing Mistakes

This is one of the most important things to know in Git.

### Undo Unstaged Changes

```bash
# Discard changes in a specific file (revert to last committed version)
git restore index.html

# Discard ALL unstaged changes
git restore .
```

> ⚠️ This is permanent — these changes are gone forever!

### Unstage a File

```bash
# Remove file from staging area (keeps changes in working directory)
git restore --staged index.html
```

### Undo a Commit (Safely — keeps history)

```bash
# Create a new commit that reverses the changes from an old commit
git revert abc1234

# Revert the most recent commit
git revert HEAD
```

> ✅ `git revert` is safe to use on shared branches because it adds a new commit — it doesn't erase history.

### Reset — Rewind History

```bash
# Move back 1 commit, keep the changes staged
git reset --soft HEAD~1

# Move back 1 commit, keep changes in working directory (unstaged)
git reset --mixed HEAD~1   # (default behavior)

# Move back 1 commit, DISCARD all changes
git reset --hard HEAD~1
```

| Reset Type | Commit History | Staging Area | Working Directory |
|---|---|---|---|
| `--soft` | Moved back | Changes kept staged | Untouched |
| `--mixed` | Moved back | Cleared | Changes kept |
| `--hard` | Moved back | Cleared | Changes DELETED |

> ⚠️ Never use `git reset --hard` on commits that have been pushed to a shared branch — it rewrites history and causes problems for teammates.

### Delete Untracked Files

```bash
# Preview what would be deleted (dry run)
git clean -n

# Delete untracked files
git clean -f

# Delete untracked files AND directories
git clean -fd
```

---

## 12. Advanced Topics

### 12.1 git rebase

Rebase moves or replays your commits onto a different base commit. It's an alternative to merging that creates a cleaner, linear history.

**Merge creates this:**
```
main:    A → B → C → M (merge commit)
                  \ /
feature:   D → E
```

**Rebase creates this:**
```
main:    A → B → C
                  \
feature:           D' → E'  (commits replayed on top of C)
```

```bash
# Rebase feature branch onto main
git switch feature-branch
git rebase main

# If conflicts occur during rebase:
# 1. Fix the conflict in the file
git add conflicted-file.js
git rebase --continue

# If you want to cancel the rebase
git rebase --abort
```

**Interactive Rebase** — rewrite, squash, reorder, or delete commits:
```bash
# Open interactive editor for last 3 commits
git rebase -i HEAD~3
```

This opens an editor:
```
pick abc1234 Add login page
pick def5678 Fix typo in login
pick ghi9012 Add login validation

# Commands:
# pick   = use this commit as-is
# reword = change the commit message
# squash = merge into previous commit
# drop   = delete this commit entirely
```

**Squashing** example — combine "Fix typo" into the login commit:
```
pick abc1234 Add login page
squash def5678 Fix typo in login     ← change "pick" to "squash"
pick ghi9012 Add login validation
```
Result: `abc1234` and `def5678` become one single, clean commit.

> ⚠️ **Golden rule of rebase:** Never rebase commits that have been pushed to a shared branch. It rewrites history and breaks everyone else's copy.

---

### 12.2 git cherry-pick

Apply a specific commit from one branch to another without merging the entire branch.

> 🍒 **Cherry-pick = selectively grab one commit** and apply it elsewhere.

```
main:    A → B → C
feature: D → E → F

# You only want commit E on main:
main:    A → B → C → E'
```

```bash
# Apply one commit to the current branch
git cherry-pick abc1234

# Apply multiple commits
git cherry-pick abc1234 def5678

# Apply a range of commits
git cherry-pick abc1234..ghi9012

# Cherry-pick without automatically committing (stage only)
git cherry-pick --no-commit abc1234
```

**When to use it:**
- A bugfix was made on a feature branch and you need it on `main` immediately
- You want to backport a fix to an older release branch
- A teammate committed something useful on their branch that you need

---

### 12.3 git reflog

Reflog is your safety net. It records every time HEAD moves — including resets, rebases, and checkouts. It lets you recover "lost" commits even after `git reset --hard`.

```bash
# See all recent HEAD movements
git reflog

# Output looks like:
# abc1234 HEAD@{0}: reset: moving to HEAD~1
# def5678 HEAD@{1}: commit: Add login form
# ghi9012 HEAD@{2}: commit: Initial commit
```

**Recovery scenario:**
```bash
# You accidentally ran: git reset --hard HEAD~2
# Panic! 2 commits are gone!

git reflog
# def5678 HEAD@{1}: commit: Add login form   ← this is what you lost!

git reset --hard def5678   # Recovered!
```

> 💡 Reflog entries expire after 90 days by default, but that's usually plenty of time to recover mistakes.

---

### 12.4 git bisect

Binary search through your commit history to find exactly which commit introduced a bug.

> 🔍 **Bisect = "Detective mode"** — systematically narrows down which commit broke something.

```bash
# Start bisect
git bisect start

# Tell Git the current commit is broken
git bisect bad

# Tell Git a known-good commit (where things worked)
git bisect good v1.0.0

# Git will now checkout the commit halfway between good and bad
# Test if the bug exists, then tell Git:
git bisect good   # no bug here
git bisect bad    # bug is here

# Git narrows down further. Repeat until:
# → "abc1234 is the first bad commit"

# Exit bisect mode
git bisect reset
```

This can turn a 1000-commit search into just 10 steps (binary search — very efficient!).

---

### 12.5 git blame

See who wrote each line of a file and in which commit.

```bash
# Show author and commit for each line
git blame index.html

# Output:
# abc1234 (Harsh Vardhan 2024-01-15) <h1>Welcome</h1>
# def5678 (Alice Smith    2024-01-20) <p>Hello world</p>

# Blame a specific line range
git blame -L 10,25 index.html

# Ignore whitespace changes
git blame -w index.html
```

> 💡 VS Code has a built-in "Git Lens" extension that shows blame info inline as you type — much easier than the command line.

---

### 12.6 git tag

Tags mark specific commits as important milestones (usually version releases).

```bash
# Create a lightweight tag (just a name pointing to a commit)
git tag v1.0

# Create an annotated tag (recommended — includes message, date, author)
git tag -a v1.0 -m "Version 1.0 - first public release"

# Tag a specific old commit
git tag -a v0.9 abc1234 -m "Beta release"

# List all tags
git tag

# See tag details
git show v1.0

# Push tags to GitHub (tags aren't pushed automatically!)
git push origin v1.0
git push origin --tags    # Push ALL tags

# Delete a tag locally
git tag -d v1.0

# Delete a tag from GitHub
git push origin --delete v1.0
```

---

### 12.7 git submodules

Submodules let you include another Git repository inside your own — like a library or shared component.

```bash
# Add a submodule
git submodule add https://github.com/username/library.git libs/library

# Clone a project that has submodules
git clone --recurse-submodules https://github.com/username/project.git

# If you already cloned but forgot --recurse-submodules:
git submodule update --init --recursive

# Update all submodules to latest
git submodule update --remote
```

> ⚠️ Submodules add complexity. For most projects, it's simpler to use a package manager (npm, pip, etc.) instead.

---

### 12.8 git hooks

Hooks are scripts that run automatically at specific points in the Git workflow. They live in `.git/hooks/`.

```
.git/hooks/
├── pre-commit       ← runs before every commit
├── commit-msg       ← runs after you write a commit message
├── post-commit      ← runs after a commit completes
├── pre-push         ← runs before git push
└── post-merge       ← runs after a merge
```

**Example: Prevent committing if tests fail**

Create `.git/hooks/pre-commit`:
```bash
#!/bin/sh
npm test
if [ $? -ne 0 ]; then
  echo "Tests failed! Commit aborted."
  exit 1
fi
```

Make it executable:
```bash
chmod +x .git/hooks/pre-commit
```

> 💡 Since `.git/hooks/` isn't tracked by Git, use tools like **Husky** (for Node.js projects) to share hooks with your team through `package.json`.

---

### 12.9 git worktree

Worktree lets you check out multiple branches simultaneously in separate folders — no stashing or branch switching needed.

```bash
# Check out a branch in a new folder
git worktree add ../hotfix-work hotfix-branch

# Now you have:
# /my-project/          ← main branch
# /hotfix-work/         ← hotfix-branch (separate folder, same repo)

# List all worktrees
git worktree list

# Remove a worktree
git worktree remove ../hotfix-work
```

**When to use it:** You're deep in feature development and need to quickly fix a bug without disturbing your current work.

---

## 13. GitHub Features

### 13.1 Pull Requests (PRs)

A Pull Request is a formal way to propose that your branch be merged into another branch. It's the heart of team collaboration on GitHub.

**PR Workflow:**
```
1. Fork or clone the repo
2. Create a feature branch: git switch -c feature/new-thing
3. Make commits on your branch
4. Push to GitHub: git push origin feature/new-thing
5. Open a Pull Request on GitHub (New Pull Request button)
6. Team reviews your code, leaves comments
7. You make adjustments based on feedback
8. PR is approved and merged
```

**Good PR habits:**
- Keep PRs small and focused (one feature or bug per PR)
- Write a clear description explaining what and why
- Link to the related Issue
- Add screenshots for UI changes
- Request specific reviewers

---

### 13.2 Issues

Issues are GitHub's built-in task/bug tracker.

**Use issues to:**
- Report bugs: "Login button crashes on Safari"
- Request features: "Add dark mode"
- Track tasks: "Write documentation for API"
- Ask questions

**Close an issue automatically via a commit/PR message:**
```
git commit -m "Fix null pointer in login handler. Closes #42"
```

When this is merged to main, Issue #42 closes automatically.

**Issue Labels:** Organize with labels like `bug`, `enhancement`, `good first issue`, `help wanted`

---

### 13.3 GitHub Actions (CI/CD)

GitHub Actions lets you automate workflows — like running tests every time you push, or deploying your app when you merge to main.

Create a file at `.github/workflows/test.yml`:

```yaml
name: Run Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3        # Checkout your code

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

Now every push and PR automatically runs your tests. If tests fail, the PR shows a red ❌ warning.

**Common uses:**
- Run tests automatically (CI — Continuous Integration)
- Deploy to production on merge to main (CD — Continuous Deployment)
- Auto-format code
- Send Slack notifications

---

### 13.4 GitHub Pages

Host a static website directly from your GitHub repository — completely free.

```bash
# Option 1: Deploy from main branch /docs folder
# → Put your website files in a /docs folder

# Option 2: Deploy from gh-pages branch
git switch --orphan gh-pages
git rm -rf .
# Add your HTML/CSS/JS files
git add .
git commit -m "Deploy website"
git push origin gh-pages
```

Then go to: **Settings → Pages → Source → select branch**

Your site will be live at: `https://username.github.io/repo-name`

---

### 13.5 Forks

A fork is your personal copy of someone else's repository on GitHub.

**Fork workflow for open source contribution:**
```bash
# 1. Click "Fork" on GitHub — creates your own copy

# 2. Clone YOUR fork
git clone https://github.com/YOUR-USERNAME/project.git
cd project

# 3. Add original repo as "upstream"
git remote add upstream https://github.com/ORIGINAL-OWNER/project.git

# 4. Create a branch for your contribution
git switch -c fix/typo-in-readme

# 5. Make changes and commit
git commit -m "Fix typo in README"

# 6. Push to YOUR fork
git push origin fix/typo-in-readme

# 7. Open a Pull Request from your fork to the original repo on GitHub
```

---

### 13.6 GitHub CLI

The GitHub CLI (`gh`) lets you manage GitHub from your terminal.

```bash
# Install: https://cli.github.com

# Authenticate
gh auth login

# Create a new repo
gh repo create my-project --public

# Clone a repo
gh repo clone username/repo

# Create a PR
gh pr create --title "Add login feature" --body "This adds JWT authentication"

# List open PRs
gh pr list

# View and checkout a PR
gh pr checkout 42

# Create an issue
gh issue create --title "Bug: login crashes" --body "Steps to reproduce..."

# List issues
gh issue list
```

---

## 14. Branching Strategies

Teams use different strategies for organizing branches. Here are the most common:

### GitHub Flow (Simple — recommended for most teams)

```
main ← always deployable
  ↑
feature branches → PR → merge → deploy
```

Rules:
1. `main` is always production-ready
2. Create a branch for every new change
3. Open a PR to merge back to `main`
4. Deploy immediately after merging

Best for: Small teams, web apps, continuous deployment

### Git Flow (Complex — for versioned software)

```
main        ← production releases
develop     ← integration branch
feature/*   ← new features (branch from develop)
release/*   ← preparing a release (branch from develop)
hotfix/*    ← emergency fixes (branch from main)
```

Best for: Mobile apps, libraries, products with versioned releases

### Trunk-Based Development (Fast — for experienced teams)

Everyone commits to `main` (or `trunk`) directly, keeping branches very short-lived (hours, not days).

Best for: Large teams with excellent test coverage and feature flags

---

## 15. .gitignore

The `.gitignore` file tells Git which files and folders to never track.

Create a `.gitignore` file in your project root:

```gitignore
# Dependencies
node_modules/
vendor/

# Build output
dist/
build/
*.min.js

# Environment variables (NEVER commit these!)
.env
.env.local
.env.production

# OS files
.DS_Store          # Mac
Thumbs.db          # Windows

# Editor files
.vscode/
.idea/
*.swp              # Vim swap files

# Logs
*.log
logs/

# Compiled files
*.pyc
__pycache__/
*.class
```

### Ignoring Files That Are Already Tracked

If you accidentally committed a file that should be ignored:

```bash
# Remove from Git tracking (but keep the file on disk)
git rm --cached .env
git rm --cached -r node_modules/

# Now add it to .gitignore and commit
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Remove .env from tracking"
```

> 💡 Use [gitignore.io](https://gitignore.io) to generate `.gitignore` files for your tech stack automatically.

---

## 16. Complete Command Reference

| Command | Description |
|---|---|
| **Setup** | |
| `git init` | Initialize a new Git repository |
| `git clone URL` | Clone a remote repository |
| `git config --global user.name "Name"` | Set your name |
| `git config --global user.email "email"` | Set your email |
| `git config --list` | Show all configuration |
| **Status & History** | |
| `git status` | Show working directory status |
| `git log` | Show full commit history |
| `git log --oneline` | Show compact commit history |
| `git log --oneline --graph --all` | Show history with branch graph |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git show abc1234` | Show details of a commit |
| `git blame file.txt` | Show who changed each line |
| **Staging & Committing** | |
| `git add file.txt` | Stage a specific file |
| `git add .` | Stage all changes |
| `git commit -m "message"` | Commit with message |
| `git commit -am "message"` | Stage tracked files and commit |
| `git rm file.txt` | Delete and stage a file |
| `git mv old new` | Rename/move a file |
| **Undoing** | |
| `git restore file.txt` | Discard unstaged changes |
| `git restore --staged file.txt` | Unstage a file |
| `git revert abc1234` | Create undo-commit for a commit |
| `git reset --soft HEAD~1` | Undo commit, keep changes staged |
| `git reset --mixed HEAD~1` | Undo commit, keep changes unstaged |
| `git reset --hard HEAD~1` | Undo commit and DELETE changes |
| `git clean -fd` | Remove untracked files and folders |
| `git reflog` | Show all HEAD movements (for recovery) |
| **Branching** | |
| `git branch` | List local branches |
| `git branch -a` | List all branches including remote |
| `git branch name` | Create new branch |
| `git switch name` | Switch to branch |
| `git switch -c name` | Create and switch to branch |
| `git branch -d name` | Delete merged branch |
| `git branch -D name` | Force delete branch |
| **Merging & Rebasing** | |
| `git merge branch` | Merge branch into current branch |
| `git rebase branch` | Rebase onto another branch |
| `git rebase -i HEAD~3` | Interactive rebase (last 3 commits) |
| `git cherry-pick abc1234` | Apply a specific commit |
| **Stashing** | |
| `git stash` | Stash current changes |
| `git stash push -m "description"` | Stash with a description |
| `git stash list` | List all stashes |
| `git stash pop` | Apply and remove latest stash |
| `git stash apply stash@{1}` | Apply a specific stash |
| `git stash drop stash@{0}` | Delete a stash |
| `git stash clear` | Delete all stashes |
| **Remote** | |
| `git remote add origin URL` | Connect to a remote |
| `git remote -v` | List remotes |
| `git fetch origin` | Download changes (don't merge) |
| `git pull origin main` | Download and merge changes |
| `git push origin main` | Push commits to remote |
| `git push -u origin branch` | Push new branch and set upstream |
| **Tags** | |
| `git tag v1.0` | Create lightweight tag |
| `git tag -a v1.0 -m "msg"` | Create annotated tag |
| `git tag` | List all tags |
| `git push origin --tags` | Push all tags |
| **Advanced** | |
| `git bisect start` | Start binary search for a bug |
| `git ls-files` | List all tracked files |
| `git rev-parse HEAD` | Show current commit ID |
| `git worktree add ../dir branch` | Check out branch in new folder |
| `git submodule add URL path` | Add a submodule |

---

## 🎓 Quick Reference Cheat Sheet

```bash
# Start
git init / git clone URL

# Daily work
git status → git add . → git commit -m "msg" → git push

# Branches
git switch -c feature → work → git switch main → git merge feature

# Oops, undo!
git restore file          # discard file changes
git reset --soft HEAD~1   # undo last commit (keep changes)
git revert abc1234        # safe undo (adds new commit)
git reflog                # find anything you "lost"

# Collaborate
git pull → git push → open Pull Request on GitHub
```

---

*Happy coding! 🚀 Git takes some practice, but once it clicks, you'll wonder how you ever lived without it.*