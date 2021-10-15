# Git



## Git Configuation

`git config`

```bash
# Set config
git config --global user.name "<name>"
git config --global user.email "<email>"
git config --global user.username "<username>"

# View config
git config -l
```



## Git Initialisation

`git init`

```bash
# Initialise current working directory as git repository
git init .
```



## Local Commands

`git status`

```bash
# Show current state of git working directory and staging area
git status
```



`git add`

```bash
# Add individual file to staging area
git add <file>

# Add all files to staging area (from current directory downwards)
git add .

# Add all files to staging area (regardless of current directory)
git add -A

# Remove individual file from staging area
git rm --cached <file>

# Remove all files from staging area
git rm -r --cached .
```



`git commit`

```bash
# Commit all files in the staging area
git commit -m "<message>"

# Amend previous commit message
git commit --amend -m "<new message>"
```



`git log` and `git show`

```bash
# Show commit history
git log

# Show details of a specified commit
git show <commit hash> # Hash obtained from `git log`
```



`git diff`

```bash
# Show difference between current working directory and commits
git diff
```



`git restore`

```bash
# Discard changes to file in working directory
git restore <file>

# Discard changes to all files in working directory
git restore .
```



`git`

```bash

```



`git`

```bash

```



`git`

```bash

```







