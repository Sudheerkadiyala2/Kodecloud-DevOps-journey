# Day 28: Git Pull Request (PR) Workflow with Review and Merge

## Challenge

The objective of this challenge was to follow a proper Git collaboration workflow instead of allowing direct pushes to the `master` branch.

A developer named **Max** had already written and pushed a story to a feature branch:

```text
story/fox-and-grapes
```

However, company policy requires all changes to be:

1. Reviewed
2. Approved
3. Merged through a Pull Request (PR)

before reaching the `master` branch.

---

## Scenario

Repository:

```text
story-blog
```

Feature Branch:

```text
story/fox-and-grapes
```

Target Branch:

```text
master
```

Pull Request Title:

```text
Added fox-and-grapes story
```

Reviewer:

```text
tom
```

---

## Objectives

- Verify Max's commits
- Create a Pull Request
- Assign Tom as reviewer
- Review and approve the Pull Request
- Merge the Pull Request into master
- Verify the merge

---

## Step 1: Login as Max

SSH into the Storage Server:

```bash
su - max
```

Verify current user:

```bash
whoami
```

Output:

```text
max
```

---

## Step 2: Navigate to Repository

```bash
cd ~/story-blog
```

Verify repository:

```bash
git branch -a
```

Output:

```text
master
story/fox-and-grapes
```

---

## Step 3: Verify Commit History

Check all commits:

```bash
git log --oneline --all --decorate
```

Output:

```text
32a7584 Added fox-and-grapes story
b37d4b4 Merge branch 'story/frogs-and-ox'
d62ed17 Fix typo in story title
```

This confirmed that Max had already pushed his work.

---

## Step 4: Create Pull Request

Open the Gitea portal.

Login:

```text
Username: max
Password: Max_pass123
```

Create Pull Request:

### Title

```text
Added fox-and-grapes story
```

### Source Branch

```text
story/fox-and-grapes
```

### Destination Branch

```text
master
```

Create PR.

---

## Step 5: Add Reviewer

Open the newly created Pull Request.

Click:

```text
Reviewers
```

Add:

```text
tom
```

This requests a formal review before merging.

---

## Step 6: Login as Tom

Logout from Max's account.

Login:

```text
Username: tom
Password: Tom_pass123
```

Open the Pull Request:

```text
Added fox-and-grapes story
```

Review changes.

Approve the Pull Request.

---

## Step 7: Merge Pull Request

Click:

```text
Merge Pull Request
```

Result:

```text
Pull request successfully merged and closed
```

---

## Verification

Update local repository:

```bash
git checkout master
git pull origin master
```

Check history:

```bash
git log --oneline -5
```

Output:

```text
489a4a7 Merge pull request 'Added fox-and-grapes story' (#1) from story/fox-and-grapes into master

32a7584 Added fox-and-grapes story

b37d4b4 Merge branch 'story/frogs-and-ox'
```

This confirms that the Pull Request was successfully merged.

---

## What I Learned

Before this challenge, I mostly worked with:

```bash
git push
```

and

```bash
git merge
```

This challenge introduced the complete Pull Request workflow used in real-world software teams.

---

## Key Concepts

### Feature Branch

A separate branch used for development work.

Example:

```text
story/fox-and-grapes
```

Developers work here without affecting production code.

---

### Pull Request (PR)

A request to merge code from one branch into another.

Example:

```text
story/fox-and-grapes
          ↓
       Pull Request
          ↓
        master
```

---

### Reviewer

A team member responsible for validating:

- Code quality
- Logic
- Standards
- Security

In this challenge:

```text
tom
```

acted as reviewer.

---

### Merge Commit

When the PR is merged, Git creates a merge commit:

```text
489a4a7
```

This records the integration of the feature branch into master.

---

## Why Companies Use Pull Requests

Without Pull Requests:

```text
Developer
    ↓
Push Directly
    ↓
Master
```

Risk:

- Bugs
- Broken builds
- Unreviewed code

With Pull Requests:

```text
Developer
    ↓
Feature Branch
    ↓
Pull Request
    ↓
Review
    ↓
Approval
    ↓
Merge
    ↓
Master
```

This provides a controlled and auditable workflow.

---

## Visual Workflow

```text
Max
 |
 | Push Story
 v

story/fox-and-grapes
 |
 | Create PR
 v

Added fox-and-grapes story
 |
 | Review Request
 v

Tom
 |
 | Approve
 v

Merge Pull Request
 |
 v

master
```

---

## Commands Used

```bash
su - max

cd ~/story-blog

git branch -a

git log --oneline --all --decorate

git checkout master

git pull origin master

git log --oneline -5
```

---

## Key Takeaways

- Developers should not push directly to master.
- Feature branches isolate work in progress.
- Pull Requests enable collaboration and review.
- Reviewers improve code quality.
- Merge commits provide a clear history of changes.
- PR workflows are standard practice in professional DevOps and software engineering teams.

---

## Final Thoughts

This challenge was my first complete experience with a real Pull Request workflow.

Instead of simply merging branches from the command line, I followed the same process used by development teams in production environments:

- Feature branch development
- Pull Request creation
- Code review
- Approval
- Merge into master

This reinforced an important lesson:

> Good software isn't just written—it is reviewed, discussed, and merged responsibly.
