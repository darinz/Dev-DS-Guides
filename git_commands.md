# Git Command Line Reference

A quick and practical reference for common Git commands with examples and explanations.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Working with Branches](#working-with-branches)
3. [Committing Changes](#committing-changes)
4. [Syncing with Remotes](#syncing-with-remotes)
5. [Viewing History](#viewing-history)
6. [Stashing Changes](#stashing-changes)
7. [Resetting and Reverting](#resetting-and-reverting)
8. [Tagging](#tagging)

---

## Getting Started

```bash
git init              # Initialize a new Git repository
# Example: git init my-project

git clone <url>       # Clone a repository
# Example: git clone https://github.com/user/repo.git

git config --global user.name "Name"    # Set Git username
# Example: git config --global user.name "Jane Doe"

git config --global user.email "email@example.com"  # Set Git email
# Example: git config --global user.email "jane@example.com"
```

## Working with Branches

```bash
git branch              # List branches
# Example: git branch

git branch <name>       # Create new branch
# Example: git branch feature-1

git switch <branch>     # Switch to existing branch (Git >= 2.23)
# Example: git switch main

git switch -c <branch>  # Create and switch to new branch
# Example: git switch -c hotfix

git checkout <branch>   # Switch branches (older method)
# Example: git checkout develop
```

## Committing Changes

```bash
git status              # Show working directory status
# Example: git status

git add <file>          # Stage file for commit
# Example: git add index.html

git add .               # Stage all changes
# Example: git add .

git commit -m "message" # Commit staged changes
# Example: git commit -m "Add login feature"
```

## Syncing with Remotes

```bash
git fetch               # Download objects and refs from remote
# Example: git fetch origin

# Explanation: Safe way to check for updates without changing working directory.

git pull                # Fetch and merge from remote
# Example: git pull origin main

# Explanation: Equivalent to fetch + merge. Can cause merge conflicts if branches diverge.

git push                # Push commits to remote
# Example: git push origin main
```

## Viewing History

```bash
git log                 # View commit history
# Example: git log

git log --oneline       # Compact log view
# Example: git log --oneline

git diff                # Show changes not yet staged
# Example: git diff

git diff --staged       # Show changes staged for commit
# Example: git diff --staged
```

## Stashing Changes

```bash
git stash               # Save uncommitted changes
# Example: git stash

git stash pop           # Reapply stashed changes and remove stash
# Example: git stash pop

git stash list          # Show list of stashes
# Example: git stash list
```

## Resetting and Reverting

```bash
git reset <file>        # Unstage a file
# Example: git reset index.html

git reset --hard HEAD   # Discard all uncommitted changes
# Example: git reset --hard HEAD

git revert <commit>     # Revert a commit by creating a new commit
# Example: git revert abc1234
```

## Tagging

```bash
git tag                 # List tags
# Example: git tag

git tag <name>          # Create tag
# Example: git tag v1.0.0

git push origin <tag>   # Push tag to remote
# Example: git push origin v1.0.0
```

---

**Note**: Git is a powerful version control system with many features. This guide focuses on commonly used commands for everyday workflows.

