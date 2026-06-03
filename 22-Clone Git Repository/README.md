# Day 22: Clone Git Repository on Storage Server

## Objective

Clone an existing repository to another directory.

## Commands

```bash
git clone /opt/news.git /usr/src/kodekloudrepos/news
```

## Verification

```bash
git -C /usr/src/kodekloudrepos/news remote -v
```

Expected:

```text
origin  /opt/news.git (fetch)
origin  /opt/news.git (push)
```

## Key Concepts

### Clone

Creates a local working copy of a repository.

### Origin

Default remote created during cloning.

## Workflow

Remote Repo
↓
git clone
↓
Local Working Repository
