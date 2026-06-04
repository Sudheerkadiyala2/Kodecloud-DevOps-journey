# Day 32: Git Rebase Feature Branch with Master

## Challenge

One of the developers was actively working on a feature branch.

Meanwhile, new changes had already been pushed to the `master` branch.

The requirement was:

- Update the feature branch with the latest changes from master.
- Preserve all work already done on the feature branch.
- Do not create a merge commit.
- Push the updated feature branch back to the remote repository.

This is a perfect use case for **Git Rebase**.

---

## Why This Challenge Matters

In collaborative environments, developers often work on feature branches while other team members continue making changes to the master branch.

Example:

```text
master

A --- B --- C

feature

A --- B --- D --- E
```

Now the feature branch is behind master.

We need the latest master changes without creating a merge commit.

---

## Commands Used

### Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/<repo-name>
```

### Mark Repository as Safe (if required)

```bash
sudo git config --global --add safe.directory /usr/src/kodekloudrepos/<repo-name>
```

### Check Available Branches

```bash
git branch
```

Example:

```text
master
feature
```

### Switch to Feature Branch

```bash
git checkout feature
```

### Rebase Feature Branch onto Master

```bash
git rebase master
```

Git takes all commits from feature branch and replays them on top of the latest master branch.

### Push Changes

Since rebase rewrites commit history:

```bash
git push origin feature --force
```

or safer:

```bash
git push origin feature --force-with-lease
```

---

## Verification

View commit graph:

```bash
git log --oneline --graph --decorate --all
```

---

## What I Learned

Before this challenge, I mostly used:

```bash
git merge
```

to bring changes between branches.

This challenge introduced a cleaner alternative:

```bash
git rebase
```

which updates a branch without creating an extra merge commit.

---

## Concept: What is Rebase?

Rebase means:

> Take commits from one branch and replay them on top of another branch.

---

## Before Rebase

```text
master

A --- B --- C

feature

A --- B --- D --- E
```

Feature branch is missing commit `C`.

---

## After Rebase

```text
master

A --- B --- C

feature

A --- B --- C --- D' --- E'
```

Git reapplies:

```text
D
E
```

on top of:

```text
C
```

creating:

```text
D'
E'
```

(new commit hashes).

---

## Why Not Merge?

Using merge:

```bash
git merge master
```

would create:

```text
A --- B --- C
      \      \
       D --- E --- M
```

where:

```text
M
```

is a merge commit.

The task explicitly required:

```text
No merge commit
```

Therefore:

```bash
git rebase master
```

was the correct solution.

---

## Key Difference: Merge vs Rebase

### Merge

```bash
git merge master
```

Result:

```text
A --- B --- C
      \      \
       D --- E --- M
```

Creates a merge commit.

---

### Rebase

```bash
git rebase master
```

Result:

```text
A --- B --- C --- D' --- E'
```

Creates a clean linear history.

---

## Important Note

Rebase changes commit hashes.

Before:

```text
D
E
```

After:

```text
D'
E'
```

Because history is rewritten.

This is why pushing requires:

```bash
git push --force
```

or

```bash
git push --force-with-lease
```

---

## Visual Workflow

```text
Feature Branch
       |
       | git rebase master
       v

Latest Master Changes
       |
       v

Replay Feature Commits
       |
       v

Updated Feature Branch
       |
       | git push --force-with-lease
       v

Remote Repository
```

---

## Useful Commands

View branches:

```bash
git branch
```

Switch branch:

```bash
git checkout feature
```

Rebase:

```bash
git rebase master
```

Push rebased branch:

```bash
git push origin feature --force-with-lease
```

View commit graph:

```bash
git log --oneline --graph --decorate --all
```

---

## Key Takeaways

- Rebase updates a branch without creating a merge commit.
- Feature branch work is preserved.
- Rebase creates a cleaner commit history.
- Commit hashes change after rebasing.
- Force push is required after rebasing a pushed branch.
- Rebase is commonly used before creating Pull Requests.

---

## Final Thoughts

This challenge helped me understand one of Git's most powerful concepts: **Rebase**.

Instead of combining histories with a merge commit, rebase rewrites history and places feature branch commits on top of the latest master branch.

This creates a cleaner and easier-to-read commit history while preserving all feature branch work.

Understanding when to use:

```bash
git merge
```

versus

```bash
git rebase
```

is an essential skill for working in professional development and DevOps teams.
