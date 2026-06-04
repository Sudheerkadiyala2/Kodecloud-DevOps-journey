# Day 33: Git Hooks - Auto Create Release Tags

## Challenge

The Nautilus application development team was working on a Git repository:

```bash
/opt/apps.git
```

The repository was cloned under:

```bash
/usr/src/kodekloudrepos/apps
```

The team wanted to automate release tagging whenever changes were pushed to the `master` branch.

The requirements were:

1. Merge the `feature` branch into `master`.
2. Create a `post-update` Git hook.
3. Whenever changes are pushed to `master`, automatically create a release tag.
4. Tag format:

```text
release-YYYY-MM-DD
```

Example:

```text
release-2026-06-04
```

5. Test the hook.
6. Push changes to remote repository.

---

## Why This Challenge Matters

In real-world projects, releases happen frequently.

Instead of manually creating tags:

```bash
git tag release-2026-06-04
```

Git Hooks allow automation.

Whenever code reaches the master branch:

```text
Push
 ↓
Hook Executes
 ↓
Release Tag Created
```

This reduces manual work and prevents mistakes.

---

## Repository Structure

### Remote Repository

```bash
/opt/apps.git
```

This is a **bare repository**.

---

### Local Clone

```bash
/usr/src/kodekloudrepos/apps
```

This is the working repository where development happens.

---

## What is a Git Hook?

A Git Hook is a script that Git executes automatically when a specific event occurs.

Examples:

| Hook | Trigger |
|--------|---------|
| pre-commit | Before commit |
| post-commit | After commit |
| pre-push | Before push |
| post-update | After repository update |

---

## Understanding post-update Hook

The task required:

```text
Whenever master branch is updated
↓
Create release tag
```

Git automatically executes:

```bash
hooks/post-update
```

after a successful update.

---

## Commands Used

### Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/apps
```

---

### Verify Branches

```bash
git branch
```

Output:

```text
master
feature
```

---

### Create post-update Hook

```bash
cat > /opt/apps.git/hooks/post-update <<'EOF'
#!/bin/bash

for ref in "$@"
do
  if [ "$ref" = "refs/heads/master" ]; then
    TAG="release-$(date +%F)"
    git tag "$TAG" "$ref"
  fi
done
EOF
```

---

### Make Hook Executable

```bash
chmod +x /opt/apps.git/hooks/post-update
```

---

## Merge Feature Branch

Switch to master:

```bash
git checkout master
```

Merge:

```bash
git merge feature
```

Output:

```text
Fast-forward
```

or

```text
Merge made by recursive strategy
```

depending on repository history.

---

## Push Changes

```bash
git push origin master
```

This push triggers:

```text
post-update hook
```

which automatically creates the release tag.

---

## Verify Tag Creation

Check tags on remote repository:

```bash
git --git-dir=/opt/apps.git tag
```

Output:

```text
release-2026-06-04
```

---

### Fetch Tags Locally

```bash
git fetch --tags
```

View tags:

```bash
git tag
```

Output:

```text
release-2026-06-04
```

---

## Verification

Check latest commits:

```bash
git log --oneline -5
```

Output:

```text
cee4019 (HEAD -> master, tag: release-2026-06-04, origin/master, origin/feature, origin/HEAD, feature) Add feature

41931f2 initial commit
```

This confirms:

- Feature branch merged
- Push successful
- Hook executed
- Release tag created

---

## What I Learned

Before this challenge, I thought Git Hooks were only for local validations.

This challenge showed how hooks can automate repository-level workflows.

Examples:

```text
Automatic release tagging
Automatic deployments
Code quality checks
Notifications
CI/CD integrations
```

---

## Key Concept: Bare Repository

The hook was created inside:

```bash
/opt/apps.git/hooks
```

because:

```text
/opt/apps.git
```

is the actual remote repository.

Hooks placed inside:

```bash
/usr/src/kodekloudrepos/apps/.git/hooks
```

would only affect the local clone.

---

## Visual Workflow

```text
Feature Branch
      |
      | git merge
      v

Master Branch
      |
      | git push origin master
      v

Remote Repository
/opt/apps.git
      |
      | post-update hook
      v

Release Tag Created
release-2026-06-04
```

---

## Useful Commands

View hooks:

```bash
ls -l /opt/apps.git/hooks
```

View tags:

```bash
git tag
```

Fetch tags:

```bash
git fetch --tags
```

View commit history:

```bash
git log --oneline
```

---

## Key Takeaways

- Git Hooks automate actions during Git events.
- `post-update` runs after repository updates.
- Bare repositories store server-side hooks.
- Hooks can automatically create release tags.
- Automation reduces manual release management.
- Git Hooks are widely used in CI/CD pipelines.

---

## Real-World Relevance

Many organizations automatically create release tags after:

```text
Push to Master
```

or

```text
Merge Pull Request
```

using hooks or CI/CD systems.

Examples:

```text
release-v1.0
release-v1.1
release-v2.0
```

This helps track deployments and rollback points.

---

## Final Thoughts

This challenge introduced Git Hooks and showed how Git can automate repetitive release-management tasks.

The most valuable lesson was understanding the difference between:

```text
Local Repository Hooks
```

and

```text
Remote Repository Hooks
```

and how server-side hooks can be used to automatically create release tags whenever production code is updated.

The final result was:

```text
release-2026-06-04
```

being automatically generated after pushing changes to the master branch.
