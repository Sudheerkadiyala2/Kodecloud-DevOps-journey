# Day 29: Reset Git Commit History to Two Commits

## Challenge

The objective of this task was to rewrite the Git commit history so that the repository contained only **two commits**.

This is a common Git history-cleanup operation used before sharing code, creating releases, or maintaining a clean project history.

---

## Why This Challenge Matters

During development, developers often create many small commits:

```text
Fix typo
Update README
Add configuration
Fix configuration
Update documentation
```

While useful during development, these commits can clutter the project history.

Sometimes teams prefer a cleaner history:

```text
Initial Project Setup
Final Feature Implementation
```

This challenge demonstrated how to rewrite commit history using **Interactive Rebase**.

---

## Commands Used

### Check Existing Commit History

```bash
git log --oneline
```

Example:

```text
e5f6g7 Last update
d4e5f6 Update README
c3d4e5 Add feature
b2c3d4 Initial configuration
a1b2c3 Initial commit
```

---

### Count Existing Commits

```bash
git rev-list --count HEAD
```

Example:

```text
5
```

---

### Start Interactive Rebase

```bash
git rebase -i --root
```

This opens an editor showing all commits.

Example:

```text
pick a1b2c3 Initial commit
pick b2c3d4 Initial configuration
pick c3d4e5 Add feature
pick d4e5f6 Update README
pick e5f6g7 Last update
```

---

### Squash Commits

Modify the list:

```text
pick a1b2c3 Initial commit
squash b2c3d4 Initial configuration
squash c3d4e5 Add feature
pick d4e5f6 Update README
squash e5f6g7 Last update
```

Result:

```text
Commit 1
├── Initial commit
├── Initial configuration
└── Add feature

Commit 2
├── Update README
└── Last update
```

Save and exit.

---

### Verify Commit History

```bash
git log --oneline
```

Expected:

```text
abc123 Final Feature Commit
def456 Initial Project Commit
```

Only two commits should remain.

---

### Verify Commit Count

```bash
git rev-list --count HEAD
```

Expected:

```text
2
```

---

### Push Updated History

If commits were already pushed:

```bash
git push --force origin master
```

or safer:

```bash
git push --force-with-lease origin master
```

---

## What I Learned

Before this challenge, I thought Git history was permanent.

This challenge showed me that Git history can be rewritten using:

```bash
git rebase
```

This allows developers to:

- Clean commit history
- Combine related commits
- Improve readability
- Prepare code for release

---

## Key Concept: Interactive Rebase

Interactive Rebase allows modification of commit history.

Command:

```bash
git rebase -i --root
```

Git presents all commits and allows actions such as:

```text
pick
reword
edit
squash
fixup
drop
```

---

## Understanding Squash

### Before

```text
A
|
B
|
C
|
D
|
E
```

Five commits.

---

### After Squash

```text
ABCD
|
DE
```

Two commits.

The changes remain intact, but the history becomes cleaner.

---

## Common Rebase Actions

### Pick

Keep commit as-is.

```text
pick abc123 Add feature
```

### Squash

Combine commit with previous commit.

```text
squash def456 Fix typo
```

### Reword

Change commit message.

```text
reword ghi789 Update README
```

### Drop

Remove commit completely.

```text
drop xyz123 Experimental code
```

---

## Verification Commands

Check history:

```bash
git log --oneline
```

Count commits:

```bash
git rev-list --count HEAD
```

View graph:

```bash
git log --oneline --graph --decorate
```

Check status:

```bash
git status
```

---

## Visual Workflow

### Original History

```text
A
|
B
|
C
|
D
|
E
```

### Interactive Rebase

```text
git rebase -i --root
```

### Final History

```text
AB+C
|
D+E
```

Only two commits remain.

---

## Real-World Relevance

Interactive Rebase is commonly used before:

- Opening Pull Requests
- Creating Releases
- Sharing Code with Teams
- Maintaining Clean Git History

Many professional teams prefer:

```text
Feature Branch
├── 20 Development Commits
└── 1 Clean Squashed Commit
```

instead of exposing every temporary change.

---

## Key Takeaways

- Git history can be rewritten.
- Interactive Rebase is used for history cleanup.
- Squashing combines multiple commits into one.
- Always verify commit count after rebasing.
- Force push is required when rewriting already-pushed history.
- Clean history improves project maintainability.

---

## Final Thoughts

This challenge introduced one of Git's most powerful features: **Interactive Rebase**.

It demonstrated that Git is not only a version control system but also a tool for maintaining a clean and understandable project history.

Understanding how to safely squash commits and rewrite history is an essential skill for working in professional development and DevOps environments.
