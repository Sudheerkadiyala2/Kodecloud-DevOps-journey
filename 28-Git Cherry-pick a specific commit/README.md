# Day 27: Git Cherry-Pick a Specific Commit

## Challenge

The Nautilus application development team was working on a repository:

```bash
/opt/games.git
```

which was already cloned on the Storage Server at:

```bash
/usr/src/kodekloudrepos/games
```

The repository contained two branches:

```text
master
feature
```

A developer was actively working on the `feature` branch, but their work was not yet complete.

However, one specific commit from the feature branch needed to be moved into the `master` branch.

The commit message was:

```text
Update info.txt
```

The task was to merge only this commit into `master` and push the changes to the remote repository.

---

## Why This Challenge Matters

In real-world development, feature branches often contain multiple commits.

Example:

```text
feature
├── Add new page
├── Update info.txt
├── Fix styling
└── Experimental code
```

Sometimes only one change is ready for production.

Instead of merging the entire branch, Git allows us to bring in a single commit using:

```bash
git cherry-pick
```

This challenge introduced that concept.

---

## Commands Used

### Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/games
```

### Mark Repository as Safe (if required)

```bash
sudo git config --global --add safe.directory /usr/src/kodekloudrepos/games
```

### View Commits on Feature Branch

```bash
sudo git log --oneline feature
```

Example:

```text
a1b2c3d Update info.txt
e4f5g6h Add welcome page
i7j8k9l Update styles
```

### Switch to Master

```bash
sudo git checkout master
```

### Cherry-Pick the Required Commit

```bash
sudo git cherry-pick a1b2c3d
```

### Push Changes

```bash
sudo git push origin master
```

---

## What I Learned

Before this challenge, I assumed there were only two ways to bring changes between branches:

```bash
git merge
```

or

```bash
git rebase
```

This challenge introduced a third option:

```bash
git cherry-pick
```

which allows selecting a specific commit and applying it to another branch.

---

## Concept: What is Cherry-Pick?

Cherry-pick copies a specific commit from one branch and applies it to another.

Example:

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

Only commit `C` is copied to master.

The rest of the feature branch remains untouched.

---

## Why Not Use Merge?

If we used:

```bash
git merge feature
```

Git would bring:

```text
C
D
E
```

into master.

But the task specifically required:

```text
Only "Update info.txt"
```

Therefore:

```bash
git cherry-pick
```

was the correct solution.

---

## Mistakes I Made

### Mistake #1: Running Git Commands Outside the Repository

I tried:

```bash
git log --oneline feature
```

and received:

```text
fatal: not a git repository
```

Reason:

I was inside:

```bash
/usr/src/kodekloudrepos
```

instead of:

```bash
/usr/src/kodekloudrepos/games
```

Lesson:

Always verify your location:

```bash
pwd
```

before running Git commands.

---

### Mistake #2: Thinking Cherry-Pick Moves a Commit

Initially I thought:

```text
Cherry-pick = Move Commit
```

Actually:

```text
Cherry-pick = Copy Commit
```

The original commit stays on the feature branch.

Git creates a new commit on master with the same changes.

---

## Key Takeaways

- A feature branch can contain many commits.
- Not every commit is ready for production.
- `git cherry-pick` allows selecting a specific commit.
- Cherry-pick copies a commit instead of merging an entire branch.
- Always inspect commit history before performing a cherry-pick.
- Cherry-picking is commonly used for bug fixes and hotfixes.

---

## Visual Workflow

```text
feature
├── Commit A
├── Update info.txt
├── Commit C
└── Commit D

          |
          |
     cherry-pick
          |
          v

master
└── Update info.txt
```

---

## Useful Commands

View commits:

```bash
git log --oneline
```

View commits on another branch:

```bash
git log --oneline feature
```

Switch branch:

```bash
git checkout master
```

Cherry-pick commit:

```bash
git cherry-pick <commit-id>
```

Push changes:

```bash
git push origin master
```

---

## Final Thoughts

This challenge taught me one of the most practical Git concepts I've learned so far.

In real projects, teams rarely want to merge unfinished work into production. Cherry-picking allows developers to selectively move completed fixes and features without bringing in everything else.

Understanding the difference between:

```bash
git merge
```

and

```bash
git cherry-pick
```

was the biggest takeaway from this exercise.

It's another reminder that Git isn't just about storing code—it's about controlling exactly how changes move through a project.
