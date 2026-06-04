# Day 27: Git Cherry-Pick a Specific Commit

## Challenge Statement

The Nautilus application development team has been working on a project repository:

```bash
/opt/games.git
```

This repository is cloned on the Storage Server at:

```bash
/usr/src/kodekloudrepos/games
```

The repository contains two branches:

```text
master
feature
```

A developer is currently working on the `feature` branch and their work is still in progress.

However, one specific commit from the feature branch needs to be merged into the master branch.

The commit message is:

```text
Update info.txt
```

The objective is to:

1. Identify the commit with the message `Update info.txt`
2. Apply only that commit to the `master` branch
3. Push the updated `master` branch to the remote repository

---
## Solution

### Step 1: Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/games
```

### Step 2: Mark Repository as Safe (if required)

```bash
sudo git config --global --add safe.directory /usr/src/kodekloudrepos/games
```

### Step 3: Find the Commit Hash

View commits from the feature branch:

```bash
sudo git log --oneline feature
```

Example:

```text
a1b2c3d Update info.txt
e4f5g6h Add welcome page
i7j8k9l Update styles
```

Identify the commit hash corresponding to:

```text
Update info.txt
```

In this example:

```text
a1b2c3d
```

### Step 4: Switch to Master Branch

```bash
sudo git checkout master
```

### Step 5: Cherry-Pick the Required Commit

```bash
sudo git cherry-pick a1b2c3d
```

This copies only the selected commit into the master branch.

### Step 6: Verify Changes

```bash
sudo git log --oneline
```

You should now see the cherry-picked commit on master.

### Step 7: Push Changes to Remote Repository

```bash
sudo git push origin master
```

---

## Commands Summary

```bash
cd /usr/src/kodekloudrepos/games

sudo git config --global --add safe.directory /usr/src/kodekloudrepos/games

sudo git log --oneline feature

sudo git checkout master

sudo git cherry-pick <commit-hash>

sudo git push origin master
```

---

## Key Concept

### Cherry-Pick

Cherry-pick allows a specific commit from one branch to be copied into another branch without merging the entire branch.

Before:

```text
master

A --- B

feature

A --- B --- C --- D --- E
            ^
      Update info.txt
```

After:

```text
master

A --- B --- C'

feature

A --- B --- C --- D --- E
```

Only the selected commit is copied to master.

---

## What I Learned

- How to inspect commits on another branch.
- How to identify a commit using its message.
- How `git cherry-pick` differs from `git merge`.
- How to safely move a single change from a feature branch into production.
- Why cherry-picking is useful when a feature branch contains unfinished work.

---

## Real-World Relevance

Cherry-picking is commonly used when:

- A bug fix is ready but the feature branch is still under development.
- A hotfix needs to be moved to production quickly.
- Specific commits need to be selectively promoted between environments.

This challenge demonstrated a practical Git workflow that is frequently used by DevOps and development teams.
