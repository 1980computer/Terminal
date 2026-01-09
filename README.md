# Cheat sheet
A cheatsheet of commands for my workflow.

## Quick Reference
Most frequently used commands for daily Git workflow.
- git status – check what's changed
- git add . – stage all changes
- git commit -m "msg" – commit changes
- git push – push to remote
- git pull – update from remote
- git log --oneline --graph --decorate --all – view history
- git branch -av – list branches
- git switch <branch> – switch branches

## Local Navigation (Shell)
Basic commands to move around directories and view files in your terminal.
- cd – go to home
- cd ~/Desktop – absolute path
- cd .. – up one directory
- pwd – print current path
- ls – names
- ls -la – incl. hidden + details

## Inspect Repo State
Check what files have changed, what's staged, and view differences before committing.
- git status – what's changed/staged?
- git status -sb – short, with branch
- git diff – unstaged changes
- git diff --staged – staged changes

## Working with an Existing Repo
Commands to clone a repository, view remotes, and sync with the remote repository.
- git clone <url> – HTTPS or SSH
- cd <folder>
- git remote -v – view remotes
- git fetch – update refs (no merge)
- git pull – fetch + merge (or rebase; see config below)

## Day-to-Day: Add, Commit, Push
The core workflow for saving your changes: stage files, commit them, and push to remote.
- git add . – stage all modified/new files
- git add -p – stage hunks interactively
- git commit -m "meaningful msg" – commit staged changes
- git commit --amend – fix the last commit message or add missed files
- git commit --amend --no-edit – add files to last commit without changing message
- git push -u origin <branch> – first push; sets upstream
- git push – subsequent pushes
- git pull --rebase – pull with rebase instead of merge

## Starting a Brand-New Repo
Initialize a new Git repository and connect it to a remote repository for the first time.
- git init
- git add --all
- git commit -m "initial commit"
- git branch -M main – (optional) rename to main
- git remote add origin <url>
- git push -u origin main

## Branching & Switching
Create and switch between branches to work on features or fixes in isolation.
- git branch -av – list local & remote branches
- git branch <new-branch> – create branch
- git checkout <branch> – switch (old syntax)
- git switch <branch> – switch (newer, nicer)
- git switch -c <new-branch> – create + switch
- git branch -d <branch> – delete merged branch (safe)
- git branch -D <branch> – force delete branch (unsafe, use with caution)

## Merging (Safe Workflow)
Safely integrate changes from one branch into another, keeping a clean history.
- git checkout main
- git pull --ff-only – update main safely
- git checkout <feature>
- git merge main – bring latest main into feature
- git checkout main
- git merge <feature> – merge feature into main
- git push

## Rebasing (Optional)
Reapply your commits on top of the latest main branch for a linear history, useful before merging.
- git checkout <feature>
- git fetch origin
- git rebase origin/main – rebase feature on latest main
- git add <fixed-files> – resolve conflicts if needed
- git rebase --continue – continue after fixing conflicts
- git rebase --abort – cancel rebase if needed
- git push --force-with-lease – ⚠️ Only force push if rebasing (never on shared branches)

## Stashing
Temporarily save uncommitted changes so you can switch branches or pull updates without committing.
- git stash – stash tracked changes
- git stash -u – include untracked
- git stash list – list all stashes
- git stash pop – restore & remove from stash
- git stash apply – restore but keep in stash
- git stash drop – delete a stash entry
- git stash clear – delete all stashes

## Undo & Recover
Revert changes, unstage files, undo commits, or recover lost work using reflog.
- git restore <file> – discard unstaged changes
- git checkout -- <file> – older syntax

- git reset HEAD <file> – unstage a staged file
- git reset --soft HEAD~1 – undo commit, keep staged
- git reset --mixed HEAD~1 – undo commit, keep changes unstaged
- git reset --hard HEAD~1 – ⚠️ DESTRUCTIVE: undo commit AND changes (use with caution)

- git revert <commit> – make a new commit that undoes <commit> (safe for shared branches)
- git reflog – find lost commits & HEAD moves
- git reflog show <branch> – show reflog for specific branch

## Logs & History
View commit history, see who changed what in a file, and inspect specific commits.
- git log --oneline --graph --decorate --all – compact visual history
- git log -p – show patches (full diffs)
- git log --stat – show file stats (additions/deletions)
- git log --follow <file> – track file through renames
- git log <branch> --oneline – commits on specific branch
- git blame <file> – see who changed each line
- git show <commit> – inspect specific commit

## Comparing Things
Compare differences between branches, commits, or files to see what changed.
- git diff <branchA>..<branchB> – compare two branches
- git diff HEAD – compare working directory to last commit
- git diff --name-only HEAD~1 – show only changed filenames
- git diff <commit1> <commit2> – compare two specific commits

## Tags (Releases)
Mark specific commits as release versions for easy reference and deployment.
- git tag v1.0.0
- git tag -a v1.0.0 -m "msg"
- git push origin v1.0.0
- git push --tags

## Remotes
Manage connections to remote repositories, including adding, updating, and syncing remote branches.
- git remote -v – view all remotes
- git remote add origin <url> – add remote
- git remote set-url origin <url> – update remote URL
- git remote remove <name> – remove a remote
- git remote show origin – detailed remote info
- git fetch origin --prune – fetch and remove deleted remote branches

## .gitignore Example
Example patterns to exclude files and directories from being tracked by Git.
node_modules/
*.log
.env
.DS_Store

## To apply .gitignore after adding files:
Remove already-tracked files from Git's index so .gitignore rules take effect.
- git rm -r --cached .
- git add .
- git commit -m "apply .gitignore"

## Useful Config
Set up your Git identity and configure default behaviors to streamline your workflow.
- git config --global user.name "Your Name"
- git config --global user.email "you@example.com"
- git config --global pull.rebase true – use rebase instead of merge for pulls
- git config --global init.defaultBranch main – default branch name
- git config --global alias.lg "log --oneline --graph --decorate --all" – custom log alias
- git config --global core.editor "code --wait" – set VS Code as editor (or vim, nano, etc.)

## Safety Notes
Important warnings and best practices to avoid losing work.

⚠️ **Destructive Commands (Use with Caution):**
- `git reset --hard` – permanently deletes uncommitted changes
- `git push --force` – overwrites remote history (use `--force-with-lease` instead)
- `git clean -fd` – permanently deletes untracked files
- `git branch -D` – force deletes branch even if not merged

✅ **Safe Practices:**
- Always use `--force-with-lease` instead of `--force` when pushing
- Use `git revert` on shared branches instead of `git reset`
- Check `git status` before destructive operations
- Use `git reflog` to recover lost commits
- Stash changes before switching branches if unsure

## Cherry-Picking
Apply a specific commit from another branch to your current branch.
- git cherry-pick <commit> – apply commit to current branch
- git cherry-pick <commit1> <commit2> – pick multiple commits
- git cherry-pick --abort – cancel cherry-pick if conflicts occur

## Cleaning Up
Remove untracked files and delete branches that are no longer needed.
- git clean -n – preview untracked files to be removed (dry run)
- git clean -fd – remove untracked files and directories
- git clean -fX – remove only files ignored by .gitignore

## Common Fixes for Push
Troubleshoot and fix common issues when pushing to remote, like branch name mismatches or conflicts.

**Upstream tracking issues:**
- git push -u origin <branch> – set upstream and push

**Branch name fixes:**
- git branch -m <old> <new> – rename branch locally
- git push origin :<old> <new> – delete old remote, push new
- git push -u origin <new> – set upstream for new branch

**Sync with remote:**
- git fetch origin – update remote refs
- git rebase origin/main – rebase on latest main
- git pull --rebase – pull with rebase

**⚠️ Force push safety:**
- git push --force-with-lease – safer than --force (checks remote first)
- git push --force – ⚠️ DANGEROUS: overwrites remote (avoid on shared branches)

## Troubleshooting
Common issues and their solutions when things go wrong.

**"Your branch is behind origin/main":**
- git fetch origin
- git rebase origin/main (or git merge origin/main)

**"Updates were rejected because the tip of your current branch is behind":**
- git pull --rebase (preferred)
- git pull (creates merge commit)

**Accidentally committed to wrong branch:**
- git reset --soft HEAD~1 (undo commit, keep changes)
- git stash (save changes)
- git switch <correct-branch>
- git stash pop (restore changes)

**Lost a commit:**
- git reflog – find the commit hash
- git cherry-pick <commit-hash> – restore it

**Merge conflicts:**
- git status – see conflicted files
- Edit files to resolve conflicts
- git add <resolved-files>
- git commit (or git rebase --continue if rebasing)
