# Git – Complete Notes (1-Hour Crash Course)

---

##  1. What is Git?

Git is a **distributed version control system (VCS)** that tracks changes in files over time.

- Created by **Linus Torvalds** in 2005
- Allows multiple developers to collaborate
- Every developer has a **full copy** of the repository (distributed)
- Works **offline** (unlike centralized VCS like SVN)

---

##  2. Key Concepts

| Term | Meaning |
|------|---------|
| **Repository (Repo)** | A folder tracked by Git |
| **Commit** | A snapshot of changes |
| **Branch** | An independent line of development |
| **Merge** | Combining branches |
| **Clone** | Copying a remote repo locally |
| **Push** | Uploading local commits to remote |
| **Pull** | Downloading & merging remote changes |
| **Staging Area** | Where changes wait before committing |
| **HEAD** | Pointer to the current commit/branch |
| **Origin** | Default name for the remote repo |

---

##  3. Git Architecture (The Three Trees)

```
Working Directory  →  Staging Area (Index)  →  Repository (.git)
     (edit)               (git add)              (git commit)
```

- **Working Directory** – your actual project files
- **Staging Area** – files queued up for the next commit
- **Repository** – history of all commits

---

##  4. Installation & Configuration

```bash
# Install (Ubuntu/Debian)
sudo apt install git

# Install (macOS)
brew install git

# Configure identity (required before first commit)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set default editor
git config --global core.editor "code --wait"   # VS Code
git config --global core.editor "vim"           # Vim

# View all config
git config --list
```

---

##  5. Starting a Repository

```bash
# Initialize a new repo
git init

# Clone an existing repo
git clone https://github.com/user/repo.git

# Clone into a specific folder
git clone https://github.com/user/repo.git my-folder
```

---

##  6. Basic Workflow

```bash
# Check status
git status

# Stage a specific file
git add filename.txt

# Stage all changes
git add .

# Commit staged changes
git commit -m "Your commit message"

# Stage + commit tracked files (skip staging)
git commit -am "Message"

# View commit history
git log
git log --oneline           # Compact view
git log --oneline --graph   # With branch graph
```

---

##  7. Undoing Changes

```bash
# Discard changes in working directory (unstaged)
git restore filename.txt        # (Git 2.23+)
git checkout -- filename.txt    # (Old syntax)

# Unstage a file (keep changes in working dir)
git restore --staged filename.txt
git reset HEAD filename.txt      # (Old syntax)

# Amend the last commit (before pushing)
git commit --amend -m "New message"

# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes unstaged
git reset --mixed HEAD~1    # (default)

# Undo last commit, DISCARD all changes 
git reset --hard HEAD~1

# Create a new commit that reverses a commit (safe for shared branches)
git revert <commit-hash>
```

---

##  8. Branching

```bash
# List branches
git branch          # local
git branch -r       # remote
git branch -a       # all

# Create a branch
git branch feature-login

# Switch to a branch
git switch feature-login        # (Git 2.23+)
git checkout feature-login      # (Old syntax)

# Create + switch in one step
git switch -c feature-login
git checkout -b feature-login   # (Old syntax)

# Rename current branch
git branch -m new-name

# Delete a branch (safe – won't delete unmerged)
git branch -d feature-login

# Force delete
git branch -D feature-login
```

---

##  9. Merging

```bash
# Merge a branch into current branch
git merge feature-login

# Merge with a commit message (no fast-forward)
git merge --no-ff feature-login

# Abort a merge in conflict
git merge --abort
```

### Types of Merge

| Type | Description |
|------|-------------|
| **Fast-forward** | Linear history, no merge commit created |
| **3-way merge** | Creates a merge commit when histories diverge |
| **Squash merge** | Combines all branch commits into one |

---

## 🔷 10. Resolving Merge Conflicts

When Git can't auto-merge, it marks conflict zones:

```
  Incoming change
```

**Steps to resolve:**
1. Open the conflicting file
2. Manually edit to keep the correct version
3. Remove the conflict markers
4. Stage the resolved file: `git add filename.txt`
5. Complete the merge: `git commit`

---

## 11. Rebasing

Rebase rewrites commit history by moving/replaying commits onto another base.

```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (edit, squash, reorder last 3 commits)
git rebase -i HEAD~3

# Abort rebase
git rebase --abort

# Continue after resolving conflict
git rebase --continue
```


---

##  12. Remote Repositories

```bash
# List remotes
git remote -v

# Add a remote
git remote add origin https://github.com/user/repo.git

# Remove a remote
git remote remove origin

# Rename a remote
git remote rename origin upstream

# Fetch changes (download, don't merge)
git fetch origin

# Pull changes (fetch + merge)
git pull origin main

# Pull with rebase instead of merge
git pull --rebase origin main

# Push to remote
git push origin main

# Push and set upstream tracking
git push -u origin main

# Delete a remote branch
git push origin --delete feature-login
```

---

##  13. Stashing

Stash temporarily shelves changes so you can switch context.

```bash
# Stash current changes
git stash

# Stash with a name
git stash push -m "WIP: login feature"

# List stashes
git stash list

# Apply most recent stash (keep in stash list)
git stash apply

# Apply and remove from stash
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Delete a stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

---

##  14. Tagging

Tags mark specific commits (typically releases).

```bash
# Create a lightweight tag
git tag v1.0

# Create an annotated tag (recommended)
git tag -a v1.0 -m "Version 1.0 release"

# List tags
git tag

# Tag a specific commit
git tag -a v0.9 <commit-hash>

# Push a tag to remote
git push origin v1.0

# Push all tags
git push origin --tags

# Delete a local tag
git tag -d v1.0

# Delete a remote tag
git push origin --delete v1.0
```

---

## 🔷 15. .gitignore

Tell Git which files/folders to ignore.

```gitignore
# Ignore a file
secret.env

# Ignore a folder
node_modules/
dist/

# Ignore all .log files
*.log

# Ignore all .txt except important.txt
*.txt
!important.txt

# Ignore files in any subdirectory
**/temp/
```

```bash
# If you already committed a file, remove it from tracking
git rm --cached filename.txt
git commit -m "Remove tracked file"
```

---

##  16. Viewing History & Diffs

```bash
# Full log
git log

# One-line log
git log --oneline

# Log with graph
git log --oneline --graph --all

# Show changes in a commit
git show <commit-hash>

# Diff unstaged changes
git diff

# Diff staged changes
git diff --staged

# Diff between two branches
git diff main..feature-login

# Diff between two commits
git diff <hash1> <hash2>

# Who changed each line (blame)
git blame filename.txt
```

---

##  17. Cherry-Pick

Apply a specific commit from another branch.

```bash
git cherry-pick <commit-hash>

# Cherry-pick without committing
git cherry-pick --no-commit <commit-hash>
```

---

##  18. Git Reset vs Revert vs Restore

| Command | Scope | Rewrites History? | Safe for Shared? |
|---------|-------|-------------------|-----------------|
| `git restore` | Working dir / index | No | Yes |
| `git reset` | Commits + index | Yes | ⚠️ No |
| `git revert` | Creates new undo commit | No | ✅ Yes |

---

##  19. Useful Shortcuts & Tips

```bash
# Show a short status
git status -s

# List all tracked files
git ls-files

# Find which commit introduced a bug (binary search)
git bisect start
git bisect bad            # current commit is bad
git bisect good v1.0      # last known good commit

# Clean untracked files (dry run first!)
git clean -n    # dry run
git clean -fd   # force delete files and directories

# Show the reflog (all HEAD movements)
git reflog

# Recover a deleted branch using reflog
git checkout -b recovered-branch <hash-from-reflog>
```

---

##  20. Common Git Workflows

### Feature Branch Workflow
```
main → create feature branch → develop → merge/PR → main
```

### Gitflow Workflow
```
main          (stable releases)
develop       (integration branch)
feature/*     (new features)
release/*     (release prep)
hotfix/*      (urgent fixes on main)
```

### Trunk-Based Development
```
Everyone commits to main frequently
Feature flags used for incomplete features
Short-lived branches only
```

---

##  21. SSH Setup for GitHub/GitLab

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "you@example.com"

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy public key (add to GitHub Settings > SSH Keys)
cat ~/.ssh/id_ed25519.pub

# Test connection
ssh -T git@github.com
```

---

## 22. Quick Reference Cheat Sheet

| Action | Command |
|--------|---------|
| Init repo | `git init` |
| Clone repo | `git clone <url>` |
| Stage all | `git add .` |
| Commit | `git commit -m "msg"` |
| Check status | `git status` |
| View log | `git log --oneline` |
| Create branch | `git switch -c branch` |
| Merge branch | `git merge branch` |
| Push | `git push origin main` |
| Pull | `git pull origin main` |
| Stash | `git stash` |
| Undo commit | `git reset --soft HEAD~1` |
| Revert commit | `git revert <hash>` |
| View diff | `git diff` |
| Delete branch | `git branch -d branch` |

---
