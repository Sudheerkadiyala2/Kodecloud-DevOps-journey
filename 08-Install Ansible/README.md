# Day 8 — Install Ansible 🤖

> **100 Days of DevOps** | Challenge #8 | April 30, 2026

---

## What's This About?

This is where things start getting exciting. Up until now, everything I've done has been on individual machines, one command at a time. **Ansible changes that.** It's the bridge between doing things manually and automating infrastructure at scale.

Day 8 was about getting Ansible installed, understanding its architecture, and running that first satisfying `ansible -m ping` against a target host.

---

## The Problem

Imagine you need to create that non-interactive service account from Day 1 — but on 50 servers. Or apply the SSH hardening from Day 3 across your entire fleet. Doing it manually isn't just slow, it's inconsistent and error-prone.

Ansible lets you write that configuration **once** and run it **everywhere**, reliably.

---

## What Ansible Is (and Isn't)

| Feature | Ansible |
|---------|---------|
| **Agentless** | ✅ Uses SSH — nothing to install on targets |
| **Idempotent** | ✅ Running it twice = same result as running it once |
| **Language** | YAML (human-readable) |
| **Push-based** | ✅ Control node pushes to managed nodes |
| **Requires Python** | On managed nodes, yes |

---

## What I Did

### Installation — Control Node

```bash
# RHEL/CentOS/Fedora
sudo dnf install -y ansible

# Ubuntu/Debian
sudo apt update
sudo apt install -y ansible

# Install via pip (latest version, any OS)
pip3 install ansible --user

# Verify installation
ansible --version
```

### Setting Up Inventory

The inventory file tells Ansible which hosts to manage:

```bash
# Create a simple inventory file
sudo vim /etc/ansible/hosts

# Or create a local inventory for your project
cat > inventory.ini << 'EOF'
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
192.168.1.20

[all:vars]
ansible_user=devopsuser
ansible_ssh_private_key_file=~/.ssh/devops_key
EOF
```

### First Commands — Ad-Hoc Mode

```bash
# Test connectivity to all hosts
ansible all -i inventory.ini -m ping

# Expected output:
# 192.168.1.10 | SUCCESS => {
#     "changed": false,
#     "ping": "pong"
# }

# Run a shell command on all webservers
ansible webservers -i inventory.ini -m shell -a "uptime"

# Check disk space on all hosts
ansible all -i inventory.ini -m shell -a "df -h /"

# Gather system facts about a host
ansible 192.168.1.10 -i inventory.ini -m setup
```

---

## My First Playbook

```yaml
# verify_setup.yml
---
- name: Verify all managed hosts are healthy
  hosts: all
  become: yes

  tasks:
    - name: Check if Python is available
      command: python3 --version
      register: python_version

    - name: Print Python version
      debug:
        msg: "{{ python_version.stdout }}"

    - name: Ensure SSH is running
      service:
        name: sshd
        state: started
```

```bash
# Run the playbook
ansible-playbook -i inventory.ini verify_setup.yml

# Dry run first (check mode — no changes applied)
ansible-playbook -i inventory.ini verify_setup.yml --check
```

---

## What I Learned

- Ansible's **architecture**: control node → inventory → modules → managed nodes
- The difference between **ad-hoc commands** and **playbooks** (and when to use each)
- How Ansible uses SSH under the hood — no agents, no daemons on target hosts
- **Idempotency**: why `state: present` in Ansible is smarter than a raw `apt install` command
- The `setup` module — dumps an incredible amount of system facts you can use in playbooks
- `ansible-doc` — the built-in documentation tool: `ansible-doc yum`

---

## Gotcha I Hit

```bash
# This error means Python isn't found on the managed node
# "MODULE FAILURE: No module named 'json'"

# Fix: specify the Python interpreter explicitly
ansible all -i inventory.ini -m ping -e "ansible_python_interpreter=/usr/bin/python3"

# Or set it in inventory:
# [all:vars]
# ansible_python_interpreter=/usr/bin/python3
```

---

## What's Coming Next

Now that Ansible is installed, the real power comes in automating everything from the previous days:
- Day 1-2 tasks (user management) → `user` module
- Day 3-7 tasks (SSH hardening) → `lineinfile`, `template` modules
- This is where the 100-day journey starts compounding 🚀

---

## Key Takeaway

> Ansible is the great equalizer — it turns days of manual work into minutes of automated execution. **Installing it is easy. Understanding why it's designed the way it is — agentless, idempotent, YAML-based — is what makes you dangerous with it.**

---

## Environment

- **OS:** Linux (Control node: RHEL/Ubuntu, Managed nodes: any Linux)
- **Tools:** `ansible`, `ansible-playbook`, `ansible-doc`, `ansible-inventory`
- **Skill Area:** Configuration Management, Infrastructure Automation, Ansible

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
