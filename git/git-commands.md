# Git Command Line Reference

A comprehensive reference for Git commands with practical examples, advanced workflows, and troubleshooting tips.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Working with Branches](#working-with-branches)
3. [Committing Changes](#committing-changes)
4. [Syncing with Remotes](#syncing-with-remotes)
5. [Viewing History](#viewing-history)
6. [Stashing Changes](#stashing-changes)
7. [Merging and Rebasing](#merging-and-rebasing)
8. [Resetting and Reverting](#resetting-and-reverting)
9. [Tagging](#tagging)
10. [Advanced Commands](#advanced-commands)
11. [Git Aliases](#git-aliases)
12. [Troubleshooting](#troubleshooting)
13. [Best Practices](#best-practices)

---

## Getting Started

### Basic Setup

```bash
# Initialize a new Git repository
git init
# Example: git init my-project

# Clone a repository
git clone <url>
# Example: git clone https://github.com/user/repo.git
# Example: git clone https://github.com/user/repo.git my-local-name

# Configure Git globally
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Configure Git for a specific repository
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Configure line ending behavior
git config --global core.autocrlf input  # For macOS/Linux
git config --global core.autocrlf true   # For Windows
```

### Configuration Examples

```bash
# Set up Git with your information
git config --global user.name "Jane Doe"
git config --global user.email "jane.doe@company.com"

# Configure your default editor
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"          # Vim
git config --global core.editor "nano"         # Nano

# Set up credential helper (caches your password)
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'  # Cache for 1 hour

# Configure merge tool
git config --global merge.tool vimdiff
```

---

## Working with Branches

### Basic Branch Operations

```bash
# List all branches (local)
git branch

# List all branches (local and remote)
git branch -a

# List remote branches only
git branch -r

# Create a new branch
git branch <branch-name>
# Example: git branch feature/user-authentication

# Create and switch to a new branch
git switch -c <branch-name>
# Example: git switch -c hotfix/critical-bug

# Switch to an existing branch
git switch <branch-name>
# Example: git switch main

# Delete a branch (after merging)
git branch -d <branch-name>
# Example: git branch -d feature/old-feature

# Force delete a branch (even if not merged)
git branch -D <branch-name>
# Example: git branch -D feature/abandoned-feature
```

### Advanced Branching

```bash
# Create a branch from a specific commit
git branch <branch-name> <commit-hash>
# Example: git branch hotfix abc1234

# Create a branch from a specific tag
git branch <branch-name> <tag-name>
# Example: git branch release-branch v1.0.0

# Rename the current branch
git branch -m <new-name>
# Example: git branch -m feature/new-name

# Set up tracking for a local branch
git branch --set-upstream-to=origin/<branch-name> <local-branch>
# Example: git branch --set-upstream-to=origin/main main
```

### Branch Workflow Examples

```bash
# Feature branch workflow
git switch main
git pull origin main
git switch -c feature/new-feature
# ... make changes ...
git add .
git commit -m "Add new feature"
git push origin feature/new-feature

# Hotfix workflow
git switch main
git switch -c hotfix/critical-fix
# ... fix the issue ...
git add .
git commit -m "Fix critical bug"
git switch main
git merge hotfix/critical-fix
git branch -d hotfix/critical-fix
```

---

## Committing Changes

### Basic Committing

```bash
# Check status of working directory
git status
git status -s  # Short format

# Stage specific files
git add <file-name>
# Example: git add src/components/Button.js

# Stage all files in current directory
git add .

# Stage all tracked files (including deletions)
git add -u

# Stage all files (new, modified, deleted)
git add -A

# Unstage a file
git reset <file-name>
# Example: git reset src/components/Button.js

# Commit staged changes
git commit -m "Your commit message"
# Example: git commit -m "Add user authentication feature"

# Commit with a longer message
git commit
# This opens your default editor for a detailed message
```

### Advanced Committing

```bash
# Amend the last commit
git commit --amend
# Example: git commit --amend -m "Updated commit message"

# Add changes to the last commit
git add <file>
git commit --amend --no-edit

# Commit with specific author
git commit -m "Message" --author="John Doe <john@example.com>"

# Sign your commit (if you have GPG set up)
git commit -S -m "Signed commit message"

# Commit with date
git commit -m "Message" --date="2023-12-25T10:30:00"
```

### Commit Message Best Practices

```bash
# Good commit message format:
git commit -m "feat: add user authentication system

- Implement JWT token authentication
- Add login/logout functionality
- Include password reset feature
- Add user profile management

Closes #123"

# Conventional Commits format:
git commit -m "feat(auth): add JWT authentication"
git commit -m "fix(api): resolve user data fetching issue"
git commit -m "docs(readme): update installation instructions"
git commit -m "style(ui): improve button styling"
git commit -m "refactor(utils): simplify date formatting function"
git commit -m "test(auth): add unit tests for login function"
```

---

## Syncing with Remotes

### Basic Remote Operations

```bash
# List remote repositories
git remote -v

# Add a remote repository
git remote add <name> <url>
# Example: git remote add origin https://github.com/user/repo.git

# Remove a remote
git remote remove <name>
# Example: git remote remove origin

# Rename a remote
git remote rename <old-name> <new-name>
# Example: git remote rename origin upstream

# Fetch updates from remote
git fetch <remote>
# Example: git fetch origin

# Fetch all remotes
git fetch --all

# Pull changes from remote
git pull <remote> <branch>
# Example: git pull origin main

# Push changes to remote
git push <remote> <branch>
# Example: git push origin main

# Push all branches
git push --all origin
```

### Advanced Remote Operations

```bash
# Push with upstream tracking
git push -u origin <branch-name>
# Example: git push -u origin feature/new-feature

# Force push (use with caution!)
git push --force origin <branch-name>
# Example: git push --force origin main

# Force push with lease (safer)
git push --force-with-lease origin <branch-name>

# Push tags
git push origin --tags

# Push specific tag
git push origin <tag-name>

# Pull with rebase
git pull --rebase origin main

# Fetch and merge specific branch
git fetch origin
git merge origin/feature-branch
```

### Remote Workflow Examples

```bash
# Setting up a new repository
git remote add origin https://github.com/user/repo.git
git branch -M main
git push -u origin main

# Working with forks
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main

# Syncing with upstream
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Viewing History

### Basic History Commands

```bash
# View commit history
git log

# Compact one-line format
git log --oneline

# Show last N commits
git log -n 5

# Show commits with stats
git log --stat

# Show commits with patches
git log -p

# Show commits in a graph
git log --graph --oneline --all

# Show commits by author
git log --author="John Doe"

# Show commits since a date
git log --since="2023-01-01"

# Show commits until a date
git log --until="2023-12-31"

# Show commits between dates
git log --since="2023-01-01" --until="2023-12-31"
```

### Advanced History Commands

```bash
# Show commits with file changes
git log --follow <file>
# Example: git log --follow src/components/Button.js

# Show commits that changed a specific line
git log -L <start>,<end>:<file>
# Example: git log -L 10,20:src/components/Button.js

# Show commits with custom format
git log --pretty=format:"%h - %an, %ar : %s"

# Show commits with relative dates
git log --relative-date

# Show commits in reverse order
git log --reverse

# Show commits with merge information
git log --merges

# Show commits without merges
git log --no-merges
```

### Diff Commands

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
git diff --cached  # Same as --staged

# Show changes between commits
git diff <commit1> <commit2>
# Example: git diff abc123 def456

# Show changes in a specific file
git diff <file>
# Example: git diff src/components/Button.js

# Show changes between branches
git diff <branch1>..<branch2>
# Example: git diff main..feature-branch

# Show changes with context
git diff -U5  # Show 5 lines of context

# Show changes without context
git diff -U0

# Show changes with word diff
git diff --word-diff
```

---

## Stashing Changes

### Basic Stashing

```bash
# Stash current changes
git stash

# Stash with a message
git stash push -m "Work in progress on feature"

# List all stashes
git stash list

# Show stash contents
git stash show
git stash show -p  # Show with patch

# Apply the most recent stash
git stash pop

# Apply a specific stash
git stash pop stash@{1}

# Apply stash without removing it
git stash apply
git stash apply stash@{1}

# Remove a stash
git stash drop stash@{1}

# Remove all stashes
git stash clear
```

### Advanced Stashing

```bash
# Stash specific files
git stash push <file1> <file2>
# Example: git stash push src/components/Button.js

# Stash untracked files
git stash -u

# Stash including ignored files
git stash -a

# Create a branch from a stash
git stash branch <branch-name> stash@{1}
# Example: git stash branch feature-from-stash stash@{0}

# Show stash in a specific format
git stash show -p stash@{1}
```

### Stash Workflow Examples

```bash
# Emergency stash workflow
git stash push -m "Emergency: working on feature"
git switch main
git pull origin main
# ... fix urgent issue ...
git commit -m "Fix urgent issue"
git push origin main
git switch feature-branch
git stash pop

# Selective stashing
git add src/components/Button.js
git stash push -m "Button component changes"
git add src/components/Modal.js
git commit -m "Add modal component"
git stash pop  # Restore button changes
```

---

## Merging and Rebasing

### Basic Merging

```bash
# Merge a branch into current branch
git merge <branch-name>
# Example: git merge feature-branch

# Merge with no-fast-forward (always create merge commit)
git merge --no-ff <branch-name>

# Merge with squash (combine all commits into one)
git merge --squash <branch-name>

# Abort a merge
git merge --abort

# Continue a merge after resolving conflicts
git merge --continue
```

### Rebasing

```bash
# Rebase current branch onto another branch
git rebase <base-branch>
# Example: git rebase main

# Interactive rebase
git rebase -i <commit-hash>
# Example: git rebase -i HEAD~3

# Abort a rebase
git rebase --abort

# Continue a rebase after resolving conflicts
git rebase --continue

# Skip a commit during rebase
git rebase --skip
```

### Merge Conflicts

```bash
# Check for conflicts
git status

# List conflicted files
git diff --name-only --diff-filter=U

# Resolve conflicts manually
# Edit the conflicted files, then:
git add <resolved-file>
git commit -m "Resolve merge conflicts"

# Use merge tool
git mergetool

# Abort merge and start over
git merge --abort
```

### Advanced Merge/Rebase Examples

```bash
# Merge workflow
git switch main
git pull origin main
git switch feature-branch
git merge main  # Merge main into feature
# ... resolve conflicts if any ...
git switch main
git merge feature-branch

# Rebase workflow
git switch feature-branch
git rebase main
# ... resolve conflicts if any ...
git switch main
git merge feature-branch

# Interactive rebase to clean up commits
git rebase -i HEAD~5
# This opens an editor where you can:
# - pick: keep commit as is
# - reword: change commit message
# - edit: modify commit
# - squash: combine with previous commit
# - fixup: combine with previous commit, discard message
# - drop: remove commit
```

---

## Resetting and Reverting

### Basic Resetting

```bash
# Soft reset (keep changes staged)
git reset --soft <commit>
# Example: git reset --soft HEAD~1

# Mixed reset (default, unstage changes)
git reset <commit>
git reset --mixed <commit>
# Example: git reset HEAD~1

# Hard reset (discard all changes)
git reset --hard <commit>
# Example: git reset --hard HEAD~1

# Reset to remote branch
git reset --hard origin/main
```

### Reverting

```bash
# Revert a commit (creates new commit)
git revert <commit>
# Example: git revert abc1234

# Revert without committing
git revert -n <commit>
# Example: git revert -n abc1234

# Revert multiple commits
git revert <commit1> <commit2>
# Example: git revert abc123 def456

# Revert a merge commit
git revert -m 1 <merge-commit>
# Example: git revert -m 1 abc1234
```

### Advanced Reset/Revert Examples

```bash
# Undo last commit but keep changes
git reset --soft HEAD~1

# Undo last commit and unstage changes
git reset HEAD~1

# Undo last commit and discard changes
git reset --hard HEAD~1

# Reset specific file to last commit
git reset HEAD <file>
# Example: git reset HEAD src/components/Button.js

# Reset file to specific commit
git reset <commit> <file>
# Example: git reset abc1234 src/components/Button.js

# Revert the last commit
git revert HEAD

# Revert a range of commits
git revert HEAD~3..HEAD
```

---

## Tagging

### Basic Tagging

```bash
# List all tags
git tag

# Create a lightweight tag
git tag <tag-name>
# Example: git tag v1.0.0

# Create an annotated tag
git tag -a <tag-name> -m "Tag message"
# Example: git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag a specific commit
git tag -a <tag-name> <commit> -m "Tag message"
# Example: git tag -a v1.0.0 abc1234 -m "Release version 1.0.0"

# Show tag information
git show <tag-name>
# Example: git show v1.0.0

# Delete a tag
git tag -d <tag-name>
# Example: git tag -d v1.0.0
```

### Advanced Tagging

```bash
# List tags with pattern
git tag -l "v1.*"

# Create a signed tag
git tag -s <tag-name> -m "Signed tag message"
# Example: git tag -s v1.0.0 -m "Signed release"

# Verify a signed tag
git tag -v <tag-name>
# Example: git tag -v v1.0.0

# Push a specific tag
git push origin <tag-name>
# Example: git push origin v1.0.0

# Push all tags
git push origin --tags

# Delete remote tag
git push origin --delete <tag-name>
# Example: git push origin --delete v1.0.0
```

### Tagging Workflow Examples

```bash
# Release tagging workflow
git switch main
git pull origin main
git tag -a v1.2.0 -m "Release version 1.2.0"
git push origin v1.2.0

# Hotfix tagging
git switch hotfix-branch
git tag -a v1.2.1 -m "Hotfix release 1.2.1"
git push origin v1.2.1
git switch main
git merge hotfix-branch
git push origin main
```

---

## Advanced Commands

### Cherry-picking

```bash
# Apply a specific commit to current branch
git cherry-pick <commit>
# Example: git cherry-pick abc1234

# Cherry-pick without committing
git cherry-pick -n <commit>

# Cherry-pick multiple commits
git cherry-pick <commit1> <commit2>
# Example: git cherry-pick abc1234 def5678

# Cherry-pick a range of commits
git cherry-pick <start-commit>..<end-commit>
# Example: git cherry-pick abc1234..def5678
```

### Bisecting

```bash
# Start bisecting to find a bad commit
git bisect start

# Mark current commit as bad
git bisect bad

# Mark a known good commit
git bisect good <commit>
# Example: git bisect good v1.0.0

# Mark current commit as good
git bisect good

# Reset bisect
git bisect reset
```

### Submodules

```bash
# Add a submodule
git submodule add <url> <path>
# Example: git submodule add https://github.com/user/lib.git libs

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone repository with submodules
git clone --recursive <url>
# Or after cloning:
git submodule update --init --recursive
```

### Worktree

```bash
# Create a new worktree
git worktree add <path> <branch>
# Example: git worktree add ../feature-branch feature-branch

# List worktrees
git worktree list

# Remove a worktree
git worktree remove <path>
# Example: git worktree remove ../feature-branch
```

---

## Git Aliases

### Useful Aliases

```bash
# Set up aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'

# Advanced aliases
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.lga "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"
git config --global alias.staged "diff --cached"
git config --global alias.unstaged "diff"
git config --global alias.branches "branch -a"
git config --global alias.remotes "remote -v"
```

### Using Aliases

```bash
# Instead of git checkout
git co main

# Instead of git status
git st

# Instead of git commit
git ci -m "Message"

# Instead of git branch
git br

# Show pretty log
git lg

# Show staged changes
git staged
```

---

## Troubleshooting

### Common Issues

```bash
# Fix "fatal: refusing to merge unrelated histories"
git pull origin main --allow-unrelated-histories

# Fix "fatal: The current branch has no upstream branch"
git push -u origin <branch-name>

# Fix "fatal: remote origin already exists"
git remote remove origin
git remote add origin <new-url>

# Fix "fatal: bad object reference"
git fsck --full

# Fix corrupted repository
git clone <url> <new-directory>
cp -r <old-directory>/.git <new-directory>/

# Fix line ending issues
git config --global core.autocrlf input  # For macOS/Linux
git config --global core.autocrlf true   # For Windows
```

### Recovery Commands

```bash
# Recover deleted branch
git reflog
git checkout -b <branch-name> <commit-hash>

# Recover deleted file
git checkout <commit> -- <file>
# Example: git checkout HEAD~1 -- src/components/Button.js

# Recover from hard reset
git reflog
git reset --hard <commit-hash>

# Clean untracked files
git clean -n  # Dry run
git clean -f  # Force clean
git clean -fd # Force clean including directories
```

### Performance Issues

```bash
# Optimize repository
git gc
git prune

# Check repository size
du -sh .git

# Clean up old branches
git remote prune origin

# Check for large files
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sed -n 's/^blob //p' | sort -nr -k 2 | head -10
```

---

## Best Practices

### Commit Messages

- Use conventional commits format
- Write clear, descriptive messages
- Keep first line under 50 characters
- Use imperative mood ("Add feature" not "Added feature")
- Reference issues when applicable

### Branching Strategy

- Use feature branches for new development
- Keep main branch stable
- Use descriptive branch names
- Delete merged branches
- Use pull requests for code review

### Workflow Tips

- Pull before pushing
- Use `--force-with-lease` instead of `--force`
- Review changes before committing
- Use meaningful commit messages
- Keep commits atomic and focused

### Security

- Never commit sensitive information
- Use `.gitignore` for configuration files
- Review changes before pushing
- Use signed commits when possible
- Regularly update Git

---

**Note**: Git is a powerful version control system with many features. This guide covers the most commonly used commands and workflows. For more advanced usage, refer to the [Git documentation](https://git-scm.com/doc).

