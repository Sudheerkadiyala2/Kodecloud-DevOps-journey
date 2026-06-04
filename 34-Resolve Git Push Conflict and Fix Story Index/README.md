# Day 30: Resolve Git Push Conflict and Fix Story Index

## Challenge

Sarah and Max were collaborating on a Git repository named:

```text
story-blog
```

Max had recently added some changes and was trying to push them to the remote repository, but the push was failing.

Additionally:

- `story-index.txt` needed to contain all 4 story titles.
- There was a typo:

```text
The Lion and the Mooose
```

which needed to be corrected to:

```text
The Lion and the Mouse
```

The objective was to resolve the push issue, correct the index file, and successfully push the changes.

---

## Problem Encountered

While pushing changes:

```bash
git push origin master
```

Git returned:

```text
! [rejected] master -> master (non-fast-forward)

Updates were rejected because a pushed branch tip is behind its remote counterpart.
```

This indicated that the remote repository contained commits that were not present in Max's local repository.

---

## Investigation

Opening:

```text
story-index.txt
```

revealed:

```text
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
```

Only 3 stories were listed.

Further inspection revealed a merge conflict:

```text
<<<<<<< HEAD
3. The Fox and the Grapes
4. The Donkey and the Dog
=======
3. The Fox and the Grapes
>>>>>>> d772f5fd03792f347a8219fe2dd17cbe9ef61d46
```

---

## Root Cause

Two developers had modified the same file.

Git could not automatically merge the changes and inserted conflict markers.

The file required manual conflict resolution.

---

## Final Correct Content

```text
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

All conflict markers were removed.

---

## Solution

### Login as Max

```bash
su - max
```

Password:

```text
Max_pass123
```

---

### Navigate to Repository

```bash
cd /home/max/story-blog
```

---

### Fetch Latest Changes

```bash
git fetch origin
```

---

### Rebase Local Changes

```bash
git rebase origin/master
```

---

### Resolve Conflict

Edit:

```bash
vi story-index.txt
```

Replace content with:

```text
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

---

### Stage Changes

```bash
git add story-index.txt
```

---

### Continue Rebase

```bash
git rebase --continue
```

---

### Push Changes

```bash
git push origin master
```

Credentials:

```text
Username: max
Password: Max_pass123
```

---

## What I Learned

This challenge demonstrated a common real-world Git scenario where:

- Multiple developers modify the same file.
- Git rejects a push because local history is behind remote history.
- Merge conflicts must be manually resolved.

---

## Key Concepts

### Non-Fast-Forward Push

Occurs when:

```text
Remote Repository
     |
     +--- New Commit

Local Repository
     |
     +--- Older Commit
```

Git prevents the push to avoid overwriting newer changes.

---

### Rebase

```bash
git rebase origin/master
```

Replays local commits on top of the latest remote commits.

---

### Merge Conflict

Occurs when Git cannot automatically decide which changes to keep.

Conflict markers:

```text
<<<<<<< HEAD
=======
>>>>>>>
```

must be manually resolved.

---

## Visual Workflow

```text
Local Changes
      |
      v

Push Rejected
      |
      v

Fetch Remote Changes
      |
      v

Resolve Conflict
      |
      v

Rebase Complete
      |
      v

Push Successful
```

---

## Verification

Check repository status:

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

View latest commits:

```bash
git log --oneline -5
```

Verify index:

```bash
cat story-index.txt
```

Expected:

```text
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

---

## Key Takeaways

- Git protects remote history through non-fast-forward checks.
- `git fetch` downloads remote changes.
- `git rebase` replays local work on updated history.
- Merge conflicts require manual resolution.
- Conflict markers must always be removed before committing.
- Team collaboration often requires synchronizing changes before pushing.

---

## Final Thoughts

This challenge simulated a realistic collaboration issue faced by development teams.

The biggest takeaway was understanding how Git handles conflicting changes and how rebasing helps integrate local work with remote updates without losing data.

By resolving the conflict, fixing the story index, and successfully pushing the changes, I gained hands-on experience with one of the most common Git workflows used in professional software development.
