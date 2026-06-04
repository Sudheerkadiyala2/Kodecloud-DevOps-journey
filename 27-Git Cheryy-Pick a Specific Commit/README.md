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
