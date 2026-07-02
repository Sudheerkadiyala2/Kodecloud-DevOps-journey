# Task 60 — Installing Jenkins Plugins for CI/CD 🔌
**100 Days of DevOps | Challenge #60**

## What's This About?

A Jenkins server is only as powerful as the plugins it runs. Today, I configured a Jenkins instance by installing essential plugins that enable seamless integration with Git repositories and GitLab.

This task focused on extending Jenkins' capabilities through the **Plugin Manager**, an important skill for anyone building CI/CD pipelines.

---

## The Problem

The **Nautilus DevOps Team** had recently deployed a Jenkins server and wanted to prepare it for upcoming CI/CD jobs.

The objectives were to:

- Log in to the Jenkins web interface.
- Install the **Git** plugin.
- Install the **GitLab** plugin.
- Restart Jenkins if required to complete the installation.
- Verify that Jenkins was ready with the new plugins.

---

## What I Did

- Logged into the Jenkins dashboard using the provided administrator credentials.
- Navigated to **Manage Jenkins → Plugins**.
- Searched for and selected:
  - **Git**
  - **GitLab**
- Installed both plugins.
- Restarted Jenkins after installation when prompted.
- Waited for Jenkins to come back online.
- Logged in again and verified the plugins were successfully installed.

---

## Breakdown

| Step | Purpose |
|------|---------|
| Login to Jenkins | Access the administration dashboard |
| Manage Jenkins → Plugins | Open the Plugin Manager |
| Install Git Plugin | Enable Git repository integration |
| Install GitLab Plugin | Enable GitLab integration for CI/CD |
| Restart Jenkins | Load newly installed plugins |
| Verify Installation | Confirm plugins are active |

---

## What I Learned

- How Jenkins plugins extend the functionality of the automation server.
- The difference between installing and activating plugins.
- Why some plugins require a Jenkins restart.
- How the Plugin Manager simplifies adding new capabilities to Jenkins.
- The importance of Git and GitLab plugins in modern CI/CD workflows.

---

## Real-World Use Case

The **Git** and **GitLab** plugins are commonly used to:

- Clone source code from Git repositories.
- Trigger builds automatically after code commits.
- Integrate merge requests with Jenkins pipelines.
- Enable Continuous Integration (CI).
- Automate deployments using GitLab repositories.

These plugins are among the most commonly installed extensions in enterprise Jenkins environments.

---

## Key Takeaway

Jenkins' flexibility comes from its plugin ecosystem. Installing the right plugins is a crucial step in preparing a Jenkins server for real-world CI/CD automation, allowing teams to integrate source control systems and automate software delivery efficiently.

---

## Environment

**Platform:** KodeKloud Engineer

**OS:** Ubuntu/Debian Linux

**Tools:** Jenkins Web UI, Git Plugin, GitLab Plugin

**Skill Area:** Jenkins Administration, CI/CD, DevOps

---


Part of my **#100DaysOfDevOps** journey—building practical DevOps skills through hands-on challenges every day.

**#100DaysOfDevOps #DevOps #Jenkins #Git #GitLab #CICD #Automation #KodeKloud #Linux #CloudComputing**
