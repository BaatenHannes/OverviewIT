# GIT

## Overview

Git is a version-control system for tracking changes in computer files and coordinating work on those files among multiple people.

## Initialize

`git init` --- create new local repository

`git clone` --- create copy of an existing repository

---

## Saving changes

The commands: `git add`, `git status`, and `git commit` are all used in combination to save a snapshot of a Git project's current state.

`git add` --- stages changes

`git status` --- checks which files are staged

`git reset` --- undo a git add

`git commit` --- commits the staged changes to the **local** repository

---

## Stashing changes

Stashing takes your uncommitted changes **(both staged and unstaged)**, saves them away for later use, and then reverts them from your working copy. 

`git stash` --- temporarily shelves changes

`git stash pop` --- reaplly stashed changes to local working copy

Git stash does not stash untracked files (new files). To stash new files you can add -u (--include-untracked)

`git stash -u` --- stashes changes with untracked files

---


