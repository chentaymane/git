# Git Learning Project - Documentation

## Introduction

This project is designed to introduce you to the world of version control and collaboration using Git. Git is a powerful and widely used tool for tracking changes in your projects, collaborating with others, and ensuring the integrity of your code.

Throughout this project, you will embark on a journey of progressively building your Git skills. Starting from the basics, you'll gradually explore more advanced topics, equipping yourself with the essential knowledge and practices for effective version control and collaboration.

**Let's Git ready for it!**

## Repository Information

- **Student**: achent
- **Platform**: Zone01 Oujda
- **Project**: git
- **Repository**: https://learn.zone01oujda.ma/git/achent/git

## Project Structure

```
work/
├── hello/
│   ├── .git/
│   ├── lib/
│   │   ├── hello.sh
│   │   └── greeter.sh
│   ├── Makefile
│   └── README.md
├── cloned_hello/
├── hello.git/ (bare repository)
└── README.md (this documentation)
```

## Instructions

⚠️ **Important**: Each exercise is documented below with the actual commands used. The completion of tasks is evaluated based on:
- Commit history reflecting the changes made throughout the exercises
- This documentation detailing the process, challenges, and solutions

---

## Exercise 1: Setting Up Git

### Objective
Install Git on your local machine and configure it with your username and email address.

### Commands Used

```bash
# Install Git (follow official Git website instructions for your OS)
# Download from: https://git-scm.com/

# Verify installation
git --version

# Configure Git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify configuration
git config --list
```

### Process
1. Downloaded and installed Git from official website
2. Configured user information globally for all repositories
3. Verified installation was successful

---

## Exercise 2: Git Commits to Commit

### Objective
Learn basic Git workflow: initialize repository, stage changes, and make commits.

### Task 1: Initialize Repository and First Commit

**File content**: `echo "Hello, World"`

```bash
cd hello/
nano hello.sh          # Created file with: echo "Hello, World"
git init               # Initialize git repository
git status             # Check repository status
git add hello.sh       # Stage the file
git commit -m "Create hello.sh"
git log --oneline      # View commit history
```

### Task 2: Modify Content and Commit

**File content**:
```bash
#!/bin/bash
echo "Hello, $1"
```

```bash
nano hello.sh          # Updated content to use parameter $1
git add hello.sh
git commit -m "Change the hello.sh content"
```

### Task 3: Add Comments with Separate Commits

**File content**:
```bash
#!/bin/bash
# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

**First commit** - for the comment in line 3:
```bash
nano hello.sh          # Added comment line
git add -e hello.sh    # Interactive staging (select only comment line)
git commit -m "add comment to hello.sh"
git show               # Verify changes in commit
```

**Second commit** - for lines 4 and 5:
```bash
git add hello.sh       # Stage remaining changes
git commit -m "Add some changes after the comment"
git log --oneline      # View commit history
```

### Key Learnings
- `git add -e` enables interactive staging for partial file commits
- Making separate commits for logical changes improves history readability
- `git show` displays the most recent commit with its changes

---

## Exercise 3: History

### Objective
Learn to view and customize Git history in various formats.

### Task 1: Show Full History

```bash
git log                # Full commit history with all details
```

### Task 2: One-Line History

```bash
git log --oneline      # Condensed view showing only hashes and messages
```

### Task 3: Controlled Entries

```bash
# Display last 2 entries
git log -2

# View commits made within last 5 minutes
git log --since="5 minutes ago"
```

### Task 4: Personalized Format

**Required format**: `* e4e3645 2023-06-10 | Added a comment (HEAD -> main) [John Doe]`

```bash
git log --pretty=format:"* %h %ad | %s%d [%an]" --date=short
```

**Format placeholders**:
- `%h` - abbreviated commit hash
- `%ad` - author date
- `%s` - commit subject/message
- `%d` - ref names (branches, tags)
- `%an` - author name
- `--date=short` - shows date in YYYY-MM-DD format

---

## Exercise 4: Check it Out

### Objective
Navigate through different snapshots in repository history.

### Task 1: Restore First Snapshot

```bash
git checkout <first-commit-hash>    # Navigate to first commit
cat hello.sh                         # Display file content
```

### Task 2: Restore Second Recent Snapshot

```bash
git checkout HEAD~1                  # Go to second most recent commit
cat hello.sh                         # Display content
```

### Task 3: Return to Latest Version

```bash
git checkout master                  # Return to latest version on master branch
cat hello.sh
```

### Important Concepts
- `HEAD` - pointer to current commit
- `HEAD~1` - one commit before HEAD
- `HEAD~2` - two commits before HEAD
- Checking out a specific commit creates "detached HEAD" state
- Return to branch name (e.g., `master`) to get back to normal state

---

## Exercise 5: TAG Me

### Objective
Learn to create, navigate, and manage tags in Git.

### Task 1: Tag Current Version as v1

```bash
git tag v1
```

### Task 2: Tag Previous Version as v1-beta

```bash
git tag v1-beta HEAD~1    # Tag the commit before current
git log --oneline          # Verify tags are created
```

### Task 3: Navigate Between Tagged Versions

```bash
git checkout v1-beta      # Switch to v1-beta
git checkout v1           # Switch to v1
```

### Task 4: List All Tags

```bash
git tag                    # Display all tags in repository
```

### Benefits of Tagging
- Provides memorable names for specific versions
- No need to remember long commit hashes
- Perfect for marking release versions
- Tags are permanent references (unlike branches that move)

---

## Exercise 6: Changed Your Mind?

### Objective
Learn different methods to undo changes in Git at various stages.

### Task 1: Reverting Changes (Before Staging)

**Unwanted content**:
```bash
#!/bin/bash
# This is a bad comment. We want to revert it.
name=${1:-"World"}
echo "Hello, $name"
```

```bash
nano hello.sh              # Made unwanted changes
git checkout -- hello.sh   # Revert to last committed version
```

### Task 2: Staging and Cleaning

**Unwanted content**:
```bash
#!/bin/bash
# This is an unwanted but staged comment
name=${1:-"World"}
echo "Hello, $name"
```

```bash
nano hello.sh              # Made unwanted changes
git add hello.sh           # Staged the changes
git reset HEAD hello.sh    # Unstage the changes
git checkout -- hello.sh   # Revert the file content
```

### Task 3: Committing and Reverting

**Unwanted content**:
```bash
#!/bin/bash
# This is an unwanted but committed change
name=${1:-"World"}
echo "Hello, $name"
```

```bash
nano hello.sh
git add hello.sh
git commit -m "Add unwanted comment by mistake"
git revert HEAD           # Create new commit that undoes the change
git show                  # View the revert commit details
cat hello.sh              # Verify file is back to original state
git log --oneline         # See both commits in history
```

### Task 4: Tagging and Removing Commits

```bash
git tag oops              # Tag the unwanted commit for reference
git reset --hard v1       # Reset HEAD to v1, removing all commits after it
git show                  # Verify we're at v1
```

### Task 5: Displaying Logs with Deleted Commits

```bash
git reflog                # Show reference log (includes "deleted" commits)
```

The reflog shows all changes to HEAD, including commits that are no longer in the main history.

### Task 6: Cleaning Unreferenced Commits

```bash
git gc --prune=now --aggressive         # Run garbage collection
git reflog expire --expire=now --all    # Expire all reflog entries immediately
```

After these commands, unreferenced commits are permanently deleted and won't appear in `git reflog`.

### Task 7: Author Information

**Content to add**:
```bash
#!/bin/bash
# Default is World
# Author: Jim Weirich
name=${1:-"World"}
echo "Hello, $name"
```

```bash
nano hello.sh              # Added author information
git add hello.sh
git commit -m "Add Author informations"
```

### Task 8: Amend Commit (Add Email)

**Updated content**:
```bash
#!/bin/bash
# Default is World
# Author: Jim Weirich (jim@email.com)
name=${1:-"World"}
echo "Hello, $name"
```

```bash
nano hello.sh              # Added email to author line
git add hello.sh
git commit --amend --no-edit    # Update last commit without changing message
```

### Summary of Undo Strategies

| Situation | Command | Effect |
|-----------|---------|--------|
| Before staging | `git checkout -- <file>` | Discard working directory changes |
| After staging | `git reset HEAD <file>` + `git checkout -- <file>` | Unstage and discard changes |
| After committing (safe) | `git revert <commit>` | Create new commit that undoes changes |
| After committing (destructive) | `git reset --hard <commit>` | Remove commits from history |
| Fix last commit | `git commit --amend` | Modify the most recent commit |

---

## Exercise 7: Move It

### Objective
Organize project structure by moving files and creating build configuration.

### Task 1: Move hello.sh to lib/ Directory

```bash
mkdir -p lib                        # Create lib directory
git mv hello.sh lib/hello.sh        # Move file (Git tracks the move)
git status                          # Shows: renamed: hello.sh -> lib/hello.sh
git commit -m "Move hello.sh into lib/ directory"
```

### Task 2: Create Makefile

**Makefile content**:
```makefile
TARGET="lib/hello.sh"

run:
	bash ${TARGET}
```

```bash
nano Makefile              # Create Makefile with above content
git add Makefile
git commit -m "Add Makefile to run lib/hello.sh"
```

### Key Points
- `git mv` is better than manual move because it preserves file history
- Git recognizes file moves and maintains the connection to previous versions
- Makefile provides a standard interface for building/running the project

---

## Exercise 8: Blobs, Trees and Commits

### Objective
Understand Git's internal object model and storage structure.

### Task 1: Exploring .git/ Directory

```bash
cd .git/
ls -la
```

### Detailed .git/ Directory Structure

#### 1. **objects/** Directory

**Purpose**: The heart of Git - stores all data as objects

**Structure**:
```
objects/
├── 0a/
│   └── 3f5b2c... (object file)
├── 1b/
│   └── 4e3d7a... (object file)
├── info/
└── pack/
```

- Objects are stored using SHA-1 hash (40 hex characters)
- First 2 characters = subdirectory name
- Remaining 38 characters = filename
- Example: hash `e4e3645abc...` → stored as `objects/e4/e3645abc...`

**Four types of objects**:

1. **Blob (Binary Large Object)**
   - Stores file content (data only, no filename)
   - Each version of each file is a separate blob
   - Compressed to save space
   
2. **Tree**
   - Represents a directory
   - Contains references to blobs (files) and other trees (subdirectories)
   - Stores filenames and permissions
   
3. **Commit**
   - Snapshot of the project at a point in time
   - Contains:
     - Reference to a tree (root directory)
     - Parent commit(s) reference
     - Author information (name, email, timestamp)
     - Committer information
     - Commit message
   
4. **Tag**
   - Named reference to a specific commit
   - Can include a message and tagger information

**How to inspect objects**:
```bash
# Find object type
git cat-file -t <hash>

# View object content
git cat-file -p <hash>
```

#### 2. **config** File

**Purpose**: Repository-specific configuration settings

**Contains**:
- Core repository settings
- Remote repository URLs
- Branch tracking information
- User-specific settings for this repo
- Merge and rebase preferences

**Example content**:
```ini
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
[remote "origin"]
    url = ../hello
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
```

**Configuration hierarchy**:
1. System: `/etc/gitconfig` (all users)
2. Global: `~/.gitconfig` (current user)
3. Local: `.git/config` (this repository) - **highest priority**

#### 3. **refs/** Directory

**Purpose**: Stores pointers (references) to commit objects

**Structure**:
```
refs/
├── heads/          # Local branches
│   ├── master
│   └── greet
├── remotes/        # Remote-tracking branches
│   └── origin/
│       ├── master
│       └── greet
└── tags/           # Tags
    ├── v1
    └── v1-beta
```

**Details**:
- **refs/heads/**: Local branches
  - Each file contains a single commit SHA-1 hash
  - Example: `refs/heads/master` contains the hash of the tip of master branch
  
- **refs/remotes/**: Remote-tracking branches
  - Tracks the state of branches in remote repositories
  - Updated by `git fetch`
  - Example: `refs/remotes/origin/master`
  
- **refs/tags/**: Tags
  - Permanent markers for specific commits
  - Lightweight tags: just contain commit hash
  - Annotated tags: contain hash of tag object (which points to commit)

**Viewing references**:
```bash
cat .git/refs/heads/master       # Shows commit hash
git show-ref                      # Lists all references
```

#### 4. **HEAD** File

**Purpose**: Points to the current branch or commit (where you are now)

**Content examples**:

1. **Normal state** (on a branch):
```
ref: refs/heads/master
```
This means HEAD points to the master branch.

2. **Detached HEAD** (on a specific commit):
```
e4e3645abc123def456...
```
This means HEAD points directly to a commit, not a branch.

**Importance**:
- Determines what `git commit` will use as the parent commit
- Determines what branch gets updated when you commit
- `git log` shows history starting from HEAD
- Most Git commands operate relative to HEAD

**HEAD shortcuts**:
- `HEAD` - current commit
- `HEAD~1` or `HEAD^` - parent of current commit
- `HEAD~2` - grandparent of current commit
- `HEAD~n` - nth ancestor

#### 5. **index** File

**Purpose**: The staging area (also called "cache")

**What it contains**:
- List of files that will be included in next commit
- Snapshot of how files looked when you staged them
- Metadata: file paths, permissions, SHA-1 hashes

**Workflow**:
```
Working Directory → (git add) → Index → (git commit) → Repository
```

**Viewing the index**:
```bash
git ls-files --stage    # Show contents of staging area
```

#### 6. **hooks/** Directory

**Purpose**: Contains scripts that run automatically at specific Git events

**Common hooks**:
- `pre-commit`: Runs before commit is created (can abort commit)
- `post-commit`: Runs after commit is created
- `pre-push`: Runs before push (can abort push)
- `post-merge`: Runs after successful merge

**Usage**: Rename `.sample` files to activate them

#### 7. **logs/** Directory

**Purpose**: Stores reflog - history of where HEAD and branch tips have been

**Structure**:
```
logs/
├── HEAD                    # History of HEAD movements
└── refs/
    └── heads/
        └── master          # History of master branch
```

**Reflog uses**:
- Recover "lost" commits
- See what you did recently
- Undo mistakes even after `git reset --hard`

```bash
git reflog                  # Show HEAD movements
git reflog show master      # Show master branch movements
```

#### 8. **info/** Directory

**Purpose**: Additional repository information

**Contains**:
- `exclude`: Like `.gitignore` but not shared (local ignore rules)
- `refs`: Cached references information

#### 9. **description** File

**Purpose**: Used by GitWeb to display repository description

Not commonly used in local repositories.

### Task 2: Find Latest Object Hash

```bash
git rev-parse HEAD              # Get hash of latest commit
```

### Task 3: Print Object Type and Content

```bash
# Get object type
git cat-file -t <hash>          # Returns: commit

# Get object content (pretty-print)
git cat-file -p <hash>
```

**Example commit object output**:
```
tree 5b8e4d2a...
parent a3d25f1c...
author John Doe <john@email.com> 1234567890 +0000
committer John Doe <john@email.com> 1234567890 +0000

Commit message here
```

### Task 4: Dump Directory Tree

```bash
# Show tree referenced by HEAD commit
git ls-tree HEAD

# Show contents of lib/ directory
git ls-tree HEAD lib/

# Show specific file content
git cat-file -p HEAD:lib/hello.sh
```

**Example tree output**:
```
100644 blob a3f5b2c...    README.md
040000 tree b4e2d1a...    lib
100644 blob c5d3e4f...    Makefile
```

**File mode meanings**:
- `100644`: Regular file
- `100755`: Executable file
- `040000`: Directory
- `120000`: Symbolic link

### How Git Stores Data - Visual Representation

```
Commit (e4e3645...)
├── tree → abc123... (root directory)
│   ├── blob → def456... (README.md)
│   ├── tree → ghi789... (lib/)
│   │   ├── blob → jkl012... (hello.sh)
│   │   └── blob → mno345... (greeter.sh)
│   └── blob → pqr678... (Makefile)
├── parent → a3d25f... (previous commit)
├── author: John Doe
└── message: "Add greeter functionality"
```

### Key Insights About Git Internals

1. **Content-Addressable Storage**: 
   - Everything is stored by its SHA-1 hash
   - Same content = same hash = stored once (deduplication)

2. **Immutability**:
   - Objects are never modified after creation
   - "Changing history" creates new objects, old ones remain (until garbage collected)

3. **Snapshots, Not Diffs**:
   - Git stores full snapshots of files at each commit
   - Not just differences from previous version
   - Compression makes this efficient

4. **Three States**:
   - **Working Directory**: Your actual files
   - **Index (Staging Area)**: Files prepared for next commit
   - **Repository (HEAD)**: Committed snapshots

5. **References Are Pointers**:
   - Branches, tags, HEAD are just pointers to commits
   - Moving a branch is just updating a single hash in a file
   - Very lightweight operations

---

## Exercise 9: Branching

### Objective
Learn to create branches, work on parallel development, and manage diverging changes.

### Task 1: Create and Switch to New Branch

```bash
git checkout -b greet         # Create and switch to 'greet' branch
```

Equivalent to:
```bash
git branch greet              # Create branch
git checkout greet            # Switch to branch
```

### Task 2: Create greeter.sh

**File content**:
```bash
#!/bin/bash

Greeter() {
    who="$1"
    echo "Hello, $who"
}
```

```bash
cd lib/
nano greeter.sh               # Create file with above content
git add greeter.sh
git commit -m "Create greeter.sh"
```

### Task 3: Update lib/hello.sh

**File content**:
```bash
#!/bin/bash

source lib/greeter.sh

name="$1"
if [ -z "$name" ]; then
    name="World"
fi

Greeter "$name"
```

```bash
nano hello.sh                 # Update to use greeter.sh
git add hello.sh
git commit -m "Update the lib/hello.sh file "
```

### Task 4: Update Makefile

**File content**:
```makefile
# Ensure it runs the updated lib/hello.sh file
TARGET="lib/hello.sh"

run:
	bash ${TARGET}
```

```bash
cd ..
nano Makefile                 # Add comment
git add Makefile
git commit -m "Update the Makefile"
```

### Task 5: Compare Branches

```bash
git checkout master           # Switch to master branch

# Compare files between branches
git diff master greet -- Makefile
git diff master greet -- lib/hello.sh
git diff master greet -- lib/greeter.sh
```

### Task 6: Create README.md on Master

**File content**:
```
This is the Hello World example from the git project.
```

```bash
nano README.md                # Create README
git add README.md
git commit -m "Create README.md file"
```

### Task 7: View Commit History

```bash
git log --oneline --graph                    # Simple graph
git log --oneline --graph --all              # All branches
git log --oneline --reverse                  # Chronological order
```

### Task 8: Clean Up Unreferenced Objects

```bash
git fsck --unreachable                       # Check for unreachable objects
git gc --prune=now                           # Garbage collection
git reflog expire --expire=now --all        # Expire reflog entries
```

### Commit Tree Diagram

```
master:  A---B---C---D (README.md added)
              \
greet:         E---F---G (greeter functionality)
```

Where:
- A, B, C: Initial commits
- D: README.md added on master
- E: greeter.sh created on greet
- F: hello.sh updated on greet
- G: Makefile updated on greet

### Branch Concepts

**What is a branch?**
- A lightweight movable pointer to a commit
- Default branch is usually called `master` or `main`
- Creating a branch is just creating a new pointer

**Why use branches?**
- Isolate new features or experiments
- Work on multiple things in parallel
- Keep master/main stable while developing
- Easy to switch between different work contexts

**Branch commands**:
```bash
git branch                    # List branches
git branch <name>            # Create branch
git checkout <name>          # Switch branch
git checkout -b <name>       # Create and switch
git branch -d <name>         # Delete branch (safe)
git branch -D <name>         # Force delete branch
```

---

## Exercise 10: Conflicts, Merging and Rebasing

### Objective
Learn to merge branches, resolve conflicts, and understand rebasing.

### Task 1: Merge Master into Greet (Initial)

```bash
git checkout greet            # Switch to greet branch
git merge master              # Merge master into greet
git log --oneline
```

This merge brings the README.md from master into greet branch.

### Task 2: Make Changes in Master

**File content for lib/hello.sh**:
```bash
#!/bin/bash

echo "What's your name"
read my_name

echo "Hello, $my_name"
```

```bash
git checkout master           # Switch to master
nano lib/hello.sh             # Make changes (interactive version)
git add lib/hello.sh
git commit -m " make the changes below to the hello.sh file"
cat lib/hello.sh              # Verify changes
```

### Task 3: Merge Master into Greet (Conflict!)

```bash
git checkout greet            # Switch to greet
git merge master              # This creates a CONFLICT!
```

**Conflict occurs because**:
- Both branches modified lib/hello.sh
- Git cannot automatically decide which changes to keep

**Conflict markers in file**:
```bash
<<<<<<< HEAD
# Content from greet branch (current branch)
=======
# Content from master branch (being merged)
>>>>>>> master
```

### Task 4: Resolve Conflict

```bash
nano lib/hello.sh             # Manually edit file to resolve conflict
                              # Accept changes from master branch
git status                    # Shows: both modified: lib/hello.sh
git add lib/hello.sh          # Mark conflict as resolved
git commit                    # Complete the merge (opens editor for message)
```

**After resolution**:
```bash
git log --oneline             # View merge commit
cat lib/hello.sh              # Verify final content
git log --oneline --graph     # See merge in graph
```

### Task 5: Reset and Rebase

**Go back to before the merge**:
```bash
git tag last HEAD~            # Tag current position for reference
git log --oneline
git reset --hard e7be5a3      # Reset to before merge (use actual hash)
git log --oneline --reverse
```

**Rebase greet onto master**:
```bash
git rebase master
```

**If conflict occurs during rebase**:
```bash
nano lib/hello.sh             # Resolve conflict
git add lib/hello.sh          # Stage resolved file
git rebase --continue         # Continue rebase
```

**View result**:
```bash
git log --oneline --graph
git log --oneline --graph --all
```

### Task 6: Merge Greet into Master (Fast-Forward)

```bash
git checkout master           # Switch to master
git merge greet               # Fast-forward merge!
```

**Result**:
```bash
cat Makefile                  # Check files
ls lib/
cat lib/hello.sh
```

### Understanding Merging vs Rebasing

#### Merging

**What it does**:
- Creates a new "merge commit" with two parents
- Preserves complete history
- Shows when branches were merged

**Visual**:
```
Before merge:
    A---B---C master
         \
          D---E greet

After merge:
    A---B---C---F master
         \     /
          D---E greet
```

**Command**: `git merge <branch>`

**Pros**:
- Preserves exact history
- Non-destructive
- Clear record of when features were integrated

**Cons**:
- History can become complex with many merge commits
- Graph can be hard to read

#### Rebasing

**What it does**:
- Moves your commits to start from a different point
- Rewrites commit history
- Creates linear history

**Visual**:
```
Before rebase:
    A---B---C master
         \
          D---E greet

After rebase:
    A---B---C master
             \
              D'---E' greet
```

**Command**: `git rebase <branch>`

**Pros**:
- Clean, linear history
- Easier to understand
- No merge commits

**Cons**:
- Rewrites history (changes commit SHAs)
- Can be dangerous if commits are already pushed
- More complex conflict resolution

#### Fast-Forward Merge

**What is it?**
- When target branch hasn't diverged from source
- Git simply moves the pointer forward
- No merge commit created

**When it happens**:
```
Before:
    A---B---C master
             \
              D---E greet

After fast-forward:
    A---B---C---D---E master, greet
```

**Visual indicator**:
```
Updating abc123..def456
Fast-forward
 file.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

### Best Practices

**Use merging when**:
- Working with public/shared branches
- You want to preserve complete history
- Collaborating with others

**Use rebasing when**:
- Cleaning up local commits before pushing
- Wanting linear history
- Working on feature branches privately

**Golden Rule of Rebasing**:
> Never rebase commits that have been pushed to a shared repository

### Conflict Resolution Tips

1. **Understand both changes**: Read both versions carefully
2. **Communicate**: If unsure, ask the other developer
3. **Test after resolving**: Make sure code still works
4. **Use tools**: `git mergetool` can help with visual merge tools

### Commands Summary

```bash
# Merging
git merge <branch>                    # Merge branch into current
git merge --abort                     # Cancel merge if conflicts

# Rebasing
git rebase <branch>                   # Rebase current onto branch
git rebase --continue                 # Continue after resolving
git rebase --abort                    # Cancel rebase

# Conflict resolution
git status                            # See conflicted files
git add <file>                        # Mark as resolved
git commit                            # Complete merge
git rebase --continue                 # Continue rebase
```

---

## Exercise 11: Local and Remote Repositories

### Objective
Learn to clone repositories, work with remotes, and synchronize changes.

### Task 1: Clone Repository

```bash
cd ..                         # Go to work directory
git clone ./hello cloned_hello    # Clone hello repository
```

**What happens**:
- Creates new directory `cloned_hello`
- Copies all commits, branches, and history
- Sets up `origin` remote pointing to `./hello`
- Checks out the default branch

### Task 2: Explore Cloned Repository

```bash
cd cloned_hello
git log --oneline --graph --all    # View all history
```

### Task 3: Show Remote Information

```bash
git remote                    # Shows: origin
git remote -v                 # Shows URLs
git remote show origin        # Detailed information
```

**Output example**:
```
* remote origin
  Fetch URL: ../hello
  Push  URL: ../hello
  HEAD branch: master
  Remote branches:
    master tracked
    greet  tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

### Task 4: List All Branches

```bash
git branch              # Local branches only
git branch -r           # Remote branches only
git branch -a           # All branches (local and remote)
```

### Task 5: Make Changes in Original Repository

**Updated README.md content**:
```
This is the Hello World example from the git project.
(changed in the original)
```

```bash
cd ../hello                   # Go to original repository
echo "This is the Hello World example from the git project. (changed in the original)" > README.md
git add README.md
git commit -m "Update README.md File again"
cat README.md                 # Verify content
```

**Amend if needed**:
```bash
nano README.md                # Make additional edits
git add README.md
git commit --amend --no-edit  # Update last commit
git log --oneline
```

### Task 6: Fetch and Merge in Cloned Repository

```bash
cd ../cloned_hello            # Go to cloned repository
git fetch origin              # Fetch changes from origin
git log --oneline --all       # View all commits (including fetched)
git merge origin/master       # Merge changes into local master
```

**What `git fetch` does**:
- Downloads commits, files, and refs from remote
- Updates remote-tracking branches (origin/master)
- Does NOT modify your working directory or current branch

**What `git merge origin/master` does**:
- Merges remote-tracking branch into current branch
- Updates working directory

### Task 7: Track Remote Branch

```bash
git checkout -b greet origin/greet    # Create local greet tracking origin/greet
```

This creates a local `greet` branch that tracks `origin/greet`.

### Task 8: Add Remote and Push

```bash
cd ../hello                   # Go to original repository
git remote add shared ../hello.git    # Add bare repo as remote
git push shared master                # Push master branch
git push shared greet                 # Push greet branch (if exists)
```

### Important Question for Audit

**Question**: "What is the single git command equivalent to what you did before to bring changes from remote to local master branch?"

**Answer**: 
```bash
git pull origin master
```

**Explanation**:
`git pull` = `git fetch` + `git merge`

```bash
# These two are equivalent:
git pull origin master

# Is the same as:
git fetch origin
git merge origin/master
```

### Remote Repository Concepts

**What is a remote?**
- A version of your repository hosted elsewhere
- Can be on another computer, server, or directory
- Named references (default: `origin`)

**Common remote operations**:
```bash
git remote                        # List remotes
git remote -v                     # List with URLs
git remote add <name> <url>      # Add remote
git remote remove <name>          # Remove remote
git remote rename <old> <new>    # Rename remote
```

**Fetch vs Pull**:
- `git fetch`: Downloads changes but doesn't merge
- `git pull`: Downloads AND merges changes (fetch + merge)

**Push**:
```bash
git push <remote> <branch>        # Push branch to remote
git push -u origin master         # Push and set upstream
git push --all                    # Push all branches
```

**Remote-tracking branches**:
- Format: `<remote>/<branch>` (e.g., `origin/master`)
- Read-only from your perspective
- Updated by `git fetch`
- Show state of remote repository

---

## Exercise 12: Bare Repositories

### Objective
Understand bare repositories and their role in collaboration.

### What is a Bare Repository?

A **bare repository** is a Git repository that:
- Contains ONLY the version control information (.git contents)
- Has NO working directory (no actual files to edit)
- Cannot have commits made directly to it
- Used as a centralized sharing point

**Normal repository structure**:
```
hello/
├── .git/           # Git internals
├── lib/            # Working directory
│   └── hello.sh
├── Makefile
└── README.md
```

**Bare repository structure**:
```
hello.git/
├── objects/
├── refs/
├── HEAD
├── config
└── ...
(No working directory - just .git contents)
```

### Why is it Needed?

1. **Central collaboration point**:
   - Acts as hub for team to share changes
   - Everyone pushes to and pulls from bare repo

2. **Prevents conflicts**:
   - No working directory = no one can accidentally commit to it
   - Reduces confusion about which version is "current"

3. **Convention**:
   - Standard for remote repositories (GitHub, GitLab, etc.)
   - Named with `.git` suffix by convention

4. **Safer for pushing**:
   - Can't push to a branch that's checked out in normal repo
   - Bare repos don't have branches checked out

### Task 1: Create Bare Repository

```bash
cd ..                         # Go to work directory
git clone --bare hello hello.git    # Create bare repository
```

**What happens**:
- Creates `hello.git` directory
- Copies only the repository data (no working files)
- Equivalent to copying the `.git` directory and nothing else

### Task 2: Add Bare Repository as Remote

```bash
cd hello                      # Go to original repository
git remote add shared ../hello.git    # Add bare repo as remote
git remote -v                 # Verify remote added
```

### Task 3: Make Changes and Push

**Updated README.md content**:
```
This is the Hello World example from the git project.
(Changed in the original and pushed to shared)
```

```bash
nano README.md                # Update README
git add README.md
git commit -m "Update README.md in original repository"
git push shared master        # Push to bare repository
```

**What happens**:
- Commits are uploaded to `hello.git`
- Bare repository now has the latest changes
- Other clones can pull these changes

### Task 4: Pull Changes in Cloned Repository

```bash
cd ../cloned_hello            # Go to cloned repository
git checkout master           # Ensure on master branch
git pull ../hello master      # Pull from hello repository
```

**Alternative** (if shared remote configured):
```bash
git remote add shared ../hello.git
git pull shared master
```

### Workflow with Bare Repository

```
Developer A                  Bare Repo                Developer B
(hello/)                    (hello.git)              (cloned_hello/)
    |                            |                          |
    |------ git push shared ---->|                          |
    |                            |<---- git pull shared ----|
    |                            |                          |
    |                            |------ git push shared ----|
    |<---- git pull shared ------|                          |
```

### Common Bare Repository Commands

```bash
# Create bare repository
git clone --bare <source> <destination>
git init --bare <directory>

# Convert to bare
cd .git
mv * ..
cd ..
rm -rf .git

# Clone from bare
git clone <bare-repo-path>

# Push to bare
git push <remote> <branch>
```

### Real-World Usage

**Bare repositories are used in**:
- GitHub, GitLab, Bitbucket (central repositories)
- Company internal Git servers
- Shared network drives for team collaboration
- Backup repositories

**Naming convention**:
- Bare repositories typically end with `.git`
- Examples: `project.git`, `myapp.git`, `hello.git`

### Important Notes

1. **Never work directly in bare repository**:
   - Can't checkout, edit, or commit there
   - Only push/pull from other repositories

2. **Bare vs Non-Bare pushing**:
   - Can't push to checked-out branch in non-bare repo
   - Bare repos solve this problem

3. **Best practice for sharing**:
   - Use bare repository as central hub
   - All developers clone from it
   - Everyone pushes/pulls to/from bare repo

---

## Summary and Evaluation

### Project Completion Checklist

✅ **Setting Up Git**: Installed and configured Git
✅ **Basic Commits**: Created repository, made multiple commits
✅ **History**: Viewed logs in various formats
✅ **Navigation**: Checked out different commits and snapshots
✅ **Tagging**: Created and managed tags
✅ **Undoing Changes**: Reverted changes at different stages
✅ **File Organization**: Moved files and created Makefile
✅ **Git Internals**: Explored .git directory structure
✅ **Branching**: Created and worked with branches
✅ **Merging**: Resolved conflicts and merged branches
✅ **Rebasing**: Rebased branches for linear history
✅ **Remotes**: Cloned and synchronized with remote repositories
✅ **Bare Repositories**: Created and used bare repository for sharing

### Key Concepts Mastered

1. **Git Workflow**: Working Directory → Staging Area → Repository
2. **Commit History**: Understanding and navigating project history
3. **Branching Model**: Creating parallel lines of development
4. **Conflict Resolution**: Handling and resolving merge conflicts
5. **Remote Collaboration**: Pushing, pulling, and fetching changes
6. **Git Internals**: Objects, refs, HEAD, and how Git stores data

### Evaluation Criteria

Your work will be evaluated on:

1. **Correctness of Git Commands**
   - Proper use of Git commands for each task
   - Understanding when to use each command
   - Appropriate flags and options

2. **Clear Understanding of Concepts**
   - Ability to explain what each command does
   - Understanding of Git's internal structure
   - Knowledge of merge vs rebase differences
   - Understanding of bare repositories

3. **Commit History**
   - Commits reflect progression through exercises
   - Clear, descriptive commit messages
   - Logical organization of changes

4. **Documentation**
   - This README documents process and learnings
   - Challenges and solutions are described
   - Clear explanations for auditor

### Commands Reference Card

```bash
# Basic Operations
git init                          # Initialize repository
git add <file>                    # Stage changes
git commit -m "message"          # Commit changes
git status                        # Check status

# History
git log                           # View history
git log --oneline                 # Condensed history
git log --graph --all            # Visual history

# Undoing
git checkout -- <file>           # Discard changes
git reset HEAD <file>            # Unstage
git revert <commit>              # Safe undo
git reset --hard <commit>        # Destructive undo

# Branching
git branch <name>                # Create branch
git checkout <name>              # Switch branch
git checkout -b <name>           # Create and switch
git merge <branch>               # Merge branch
git rebase <branch>              # Rebase branch

# Remote Operations
git clone <url>                  # Clone repository
git remote add <name> <url>     # Add remote
git fetch <remote>               # Fetch changes
git pull <remote> <branch>      # Fetch and merge
git push <remote> <branch>      # Push changes

# Tagging
git tag <name>                   # Create tag
git tag <name> <commit>         # Tag specific commit

# Internals
git cat-file -t <hash>          # Object type
git cat-file -p <hash>          # Object content
git ls-tree <commit>            # List tree
```

### Resources for Further Learning

- [Official Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [Git Branching Tutorial](https://learngitbranching.js.org/)
- [Git Gud Game](https://github.com/benthayer/git-gud)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

### Final Notes for Auditor

This documentation represents my journey through learning Git from basics to advanced concepts. Each exercise was completed following the instructions, with the actual commands documented for review. The commit history in the repository reflects the progression through these exercises.

Key achievements:
- Understood Git's three-tree architecture
- Mastered branching and merging workflows
- Learned conflict resolution strategies
- Explored Git's internal object model
- Successfully worked with remote repositories
- Created and used bare repositories

**Repository**: https://learn.zone01oujda.ma/git/achent/git

---

**Date Completed**: January 2026  
**Student**: achent  
**Platform**: Zone01 Oujda
