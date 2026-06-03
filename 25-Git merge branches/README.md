# Day 25: Git Merge Branches

## Objective

Merge changes from feature branch into master.

## Commands

```bash
git checkout master

git merge xfusion
```

## Verification

```bash
git log --oneline --graph
```

## Key Concepts

### Merge

Combines changes from one branch into another.

Before:

master

A

xfusion

A---B

After:

master

A---B

xfusion

A---B

## Workflow

Create Branch
↓
Make Changes
↓
Commit
↓
Merge into Master
