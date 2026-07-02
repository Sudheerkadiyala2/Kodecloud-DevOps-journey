# Task 61 — Configuring User Access & Project-Based Authorization in Jenkins 🔐
**100 Days of DevOps | Challenge #61**

## What's This About?

As Jenkins becomes the central automation server for CI/CD, controlling who can access and modify resources is critical. Today, I configured **Role-Based access** using the **Project-based Matrix Authorization Strategy**, created a new user, and assigned only the permissions required based on the principle of least privilege.

This challenge focused on securing Jenkins by implementing proper authorization instead of giving every user administrative access.

---

## The Problem

The **Nautilus DevOps Team** needed to onboard a new developer while ensuring the Jenkins server remained secure.

The objectives were to:

- Log in to the Jenkins administrator account.
- Create a new Jenkins user.
- Configure **Project-based Matrix Authorization Strategy**.
- Grant the new user only the required permissions.
- Remove all permissions from anonymous users.
- Ensure the administrator retained full control.

---

## What I Did

- Logged into the Jenkins dashboard as the administrator.
- Installed the **Matrix Authorization Strategy** plugin (if required).
- Restarted Jenkins after plugin installation.
- Created a new Jenkins user:
  - **Username:** `kirsty`
  - **Full Name:** `Kirsty`
- Enabled **Project-based Matrix Authorization Strategy** under **Manage Jenkins → Security**.
- Granted the **Overall → Read** permission to the `kirsty` user.
- Removed all permissions assigned to **Anonymous** users.
- Verified that the **admin** user retained **Overall → Administer** permission.
- Opened the existing Jenkins job and configured **Project-based Security**.
- Granted the `kirsty` user **Job → Read** permission only.
- Saved the configuration and verified the permission assignments.

---

## Breakdown

| Configuration | Purpose |
|--------------|---------|
| Create Jenkins User | Add a dedicated developer account |
| Matrix Authorization Strategy | Configure fine-grained permissions |
| Overall → Read | Allow basic Jenkins access |
| Job → Read | Permit viewing the existing job only |
| Remove Anonymous Permissions | Prevent unauthorized access |
| Admin → Overall Administer | Maintain complete administrative control |

---

## What I Learned

- How Jenkins manages authentication and authorization separately.
- How the **Project-based Matrix Authorization Strategy** enables granular permission control.
- Why the **Principle of Least Privilege (PoLP)** is essential in CI/CD environments.
- How project-level permissions differ from global permissions.
- Why anonymous access should be disabled in production Jenkins servers.

---

## Real-World Use Case

In enterprise environments, different team members require different levels of access.

For example:

- **Developers** can view and trigger builds.
- **QA Engineers** may access test jobs.
- **Release Managers** can approve deployments.
- **Administrators** manage plugins, security, credentials, and system configuration.

Using matrix-based authorization helps organizations maintain security while allowing teams to collaborate efficiently.

---

## Key Takeaway

Security is a fundamental part of DevOps. Implementing granular access controls in Jenkins ensures that users receive only the permissions necessary for their responsibilities, reducing risk while maintaining operational efficiency.

---

## Environment

**Platform:** KodeKloud Engineer

**OS:** Ubuntu/Debian Linux

**Tools:** Jenkins Web UI, Matrix Authorization Strategy Plugin

**Skill Area:** Jenkins Administration, CI/CD Security, Access Management

---

## Screenshots

📸 Captured screenshots of:

- Jenkins login page
- User creation (`kirsty`)
- Matrix Authorization Strategy configuration
- Overall permission assignments
- Job-level permission configuration
- Final successful configuration

---

Part of my **#100DaysOfDevOps** journey—building practical DevOps skills through hands-on labs and real-world scenarios.

**#100DaysOfDevOps #DevOps #Jenkins #JenkinsSecurity #CICD #AccessControl #Linux #KodeKloud #Automation #CloudComputing**
