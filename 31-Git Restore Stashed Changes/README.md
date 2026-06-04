# Day 31: Git Restore Stashed Changes

## Challenge

The objective of this task was to restore previously stashed changes from a Git repository and push those changes to the remote repository.

Repository path:

```bash
/usr/src/kodekloudrepos/games
```

The required stash identifier was:

```text
stash@{1}
```

---

## What Was Asked?

The task required us to:

1. Go to the `games` Git repository
2. Check the available stashes
3. Restore the stash with identifier `stash@{1}`
4. Commit the restored changes
5. Push the commit to the remote repository

---

## Commands Used

### Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/games
```

### Mark Repository as Safe

```bash
sudo git config --global --add safe.directory /usr/src/kodekloudrepos/games
```

### Check Available Stashes

```bash
sudo git stash list
```

Example output:

```text
stash@{0}: WIP on master
stash@{1}: WIP on master
```

### Restore Required Stash

```bash
sudo git stash pop stash@{1}
```

This restored the changes from `stash@{1}`.

---

## Apply vs Pop

There are two common ways to restore a stash:

```bash
git stash apply stash@{1}
```

and

```bash
git stash pop stash@{1}
```

Difference:

```text
apply = restore stash but keep it in stash list
pop   = restore stash and remove it from stash list
```

For this task, `pop` was used because the stash changes were restored and no longer needed in the stash list.

---

## Check Restored Changes

```bash
sudo git status
```

This showed the files restored from the stash.

---

## Stage Changes

```bash
sudo git add .
```

---

## Commit Changes

```bash
sudo git commit -m "Restore stashed changes"
```

---

## Push Changes

```bash
sudo git push origin master
```

---

## Verification

Check repository status:

```bash
sudo git status
```

Expected:

```text
On branch master
nothing to commit, working tree clean
```

Check latest commits:

```bash
sudo git log --oneline -3
```

---

## What I Learned

Before this task, I knew `git stash` was used to temporarily save work, but this challenge helped me understand how to restore a specific stash using its identifier.

The key command was:

```bash
git stash pop stash@{1}
```

This means:

> Restore the changes stored in the second stash entry.

---

## Key Concept: Git Stash

Git stash temporarily saves uncommitted changes without creating a commit.

It is useful when:

- You are working on something unfinished
- You need to switch branches
- You need to pull latest changes
- You do not want to commit incomplete work

---

## Visual Workflow

```text
Working Directory
       |
       | git stash
       v
Stash Area
       |
       | git stash pop stash@{1}
       v
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Repository
       |
       | git push origin master
       v
Remote Repository
```

---

## Useful Commands

List stashes:

```bash
git stash list
```

Restore latest stash and keep it:

```bash
git stash apply
```

Restore latest stash and remove it:

```bash
git stash pop
```

Restore specific stash:

```bash
git stash pop stash@{1}
```

Delete specific stash:

```bash
git stash drop stash@{1}
```

Delete all stashes:

```bash
git stash clear
```

---

## Key Takeaways

- `git stash` stores uncommitted changes temporarily.
- `git stash list` shows all saved stashes.
- `stash@{0}` is the latest stash.
- `stash@{1}` is the second latest stash.
- `git stash pop stash@{1}` restores a specific stash and removes it from stash list.
- After restoring stash changes, they must be added, committed, and pushed like normal changes.

---

## Final Thoughts

This task helped me understand how Git handles unfinished work.

In real projects, stash is useful when developers need to quickly switch context without losing local changes.

The most important thing I learned is that stashes are stored with identifiers, and we can restore a specific stash instead of always restoring the latest one.
