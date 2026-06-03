# Day 24: Git Create Branches

## Objective

Create a new branch from master.

## Commands

```bash
cd /usr/src/kodekloudrepos/beta

git checkout master

git checkout -b xfusioncorp_beta
```

## Verification

```bash
git branch
```

Expected:

```text
* xfusioncorp_beta
  master
```

## Key Concepts

### Branch

Independent line of development.

### Why Use Branches?

- Feature Development
- Bug Fixes
- Safe Testing

## Workflow

master
|
+--- xfusioncorp_beta
