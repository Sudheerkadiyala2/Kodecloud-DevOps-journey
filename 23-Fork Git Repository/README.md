# Day 23: Fork a Git Repository

## Objective

Create a copy of an existing repository.

## Commands

```bash
git clone /opt/official.git /usr/src/kodekloudrepos/official
```

Create new repository:

```bash
git init --bare /opt/forked.git
```

Push code:

```bash
git remote add fork /opt/forked.git

git push fork master
```

## Key Concepts

### Fork

Creates an independent copy of a repository.

### Difference

Clone:
Works on same repository.

Fork:
Creates a separate repository.

## Workflow

Original Repo
↓
Clone
↓
Create New Remote
↓
Push
↓
Fork Repository
