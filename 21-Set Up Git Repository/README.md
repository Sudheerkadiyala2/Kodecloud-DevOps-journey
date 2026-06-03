# Day 21: Set Up Git Repository on Storage Server

## Objective
Install Git and create a bare repository on the Storage Server.

## Commands

```bash
yum install -y git

git init --bare /opt/beta.git
```

## Verification

```bash
ls -ld /opt/beta.git
```

## Key Concepts

### Git
Version Control System used to track changes.

### Bare Repository
Contains only Git metadata.

Example:

```text
/opt/beta.git
├── HEAD
├── objects
├── refs
└── config
```

### Why Bare Repository?

Acts as a central remote repository for developers.

## Workflow

Storage Server
↓
Create Bare Repository
↓
Developers Clone Repository
