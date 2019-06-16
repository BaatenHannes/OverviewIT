# GIT

## Overview

Git is a version-control system for tracking changes in computer files and coordinating work on those files among multiple people.

Example: stage, commit and push changes to remote repository called origin.

```
git add *
git commit -m "bugfix"
git fetch origin
git pull origin
git push origin --all 
```

Example: push exisiting project to new remote repository (without readme)
Step 1: Create local repository and commit changes
```
git init
git add .
git commit -m "Initial commit"
```
Step 2: add remote and push changes to remote
```
git remote add origin remote_origin_url
git remote -v (verifies then new remote URL)
git push origin master 
```

---

## Initialize

* `git init` --- create new local repository

* `git clone` --- create copy of an existing repository, automatically creates a remote connection called origin

---

## Saving changes

The commands: `git add`, `git status`, and `git commit` are all used in combination to save a snapshot of a Git project's current state.

* `git add` --- stages changes

* `git status` --- checks which files are staged

* `git reset` --- undo a git add

* `git commit` --- commits the staged changes to the **local** repository

---

## Stashing changes

Stashing takes your uncommitted changes **(both staged and unstaged)**, saves them away for later use, and then reverts them from your working copy. 

* `git stash` --- temporarily shelves changes

* `git stash pop` --- reaplly stashed changes to local working copy

Git stash does not stash untracked files (new files). To stash new files you can add -u (--include-untracked)

* `git stash -u` --- stashes changes with untracked files

---

## Syncing with remote repository

### Configuring remote

A remote repository can be reached through a URL and credentials. Git remote allows you to creates an alias for easy use of the URL of the remote repository.

* `git remote` --- get current remote configurations

* `git remote -v` --- same as the above but includes the URL of each remote

* `git remote add <name> <url>` --- adds a new remote

* `git remote rm <name>` --- remove remote

### Pushing

* `git push <remote> <branch>` 

### Merge vs Rebase

Both try to achieve the same result, but rebase rewrites the history by 'rebasing' one branch on top of the other, creating a lineair history. This prevents cluttering with merge commits, but can create confusing situations by creating a different history with other team members. [Link to explanation](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

![Merge](\img\merge.svg)

![Rebase](\img\rebase.svg)


### Git Reset

If you made a local commit, but did not push it to remote yet, you can use a git reset to 'remove' the commit and keep or throw away the changes.

-soft -- will keep the changes of the commit and stage them
-mixed -- will keep the changes of the commit, but will not stage them
-hard -- will throw away the changes of the commit