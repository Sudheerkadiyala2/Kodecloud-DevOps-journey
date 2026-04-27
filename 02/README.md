# Day 2 – Create User with Expiry (Linux User Management)

## 📌 Task

As part of a temporary assignment, a user `rose` was created on App Server 3 with a defined expiry date to ensure limited-time access.

## ⚙️ Command Used

```bash
sudo useradd -m -e 2027-03-28 rose
```

## 🔍 Verification

```bash
sudo chage -l rose
```

## 📅 Output

* Account expires on: **March 28, 2027**

## 🧠 Concept

This task demonstrates Linux user management by creating a user with a home directory and setting an account expiry date.

## 🚀 DevOps Relevance

* Managing temporary access for developers, interns, or contractors
* Enforcing security using time-bound access
* Reducing manual effort by automating user deactivation
* Commonly used in server administration and cloud environments

## 📁 Environment

* Linux Server (App Server 3 – Stratos Datacenter)
* Command-line interface (CLI)

