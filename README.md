# 🚀 Ansible Practical — by Nkechi Ahanonye

> *"Infrastructure as code means your servers are only as good as your playbooks."*

A hands-on Ansible project built from scratch as part of a full Ansible Masterclass. This repository covers real-world infrastructure automation using roles, variables, handlers, templates, and Ansible Vault for secrets management.

---

## 📁 Project Structure

```
ansible_practical/
└── ansible-project/
    ├── inventory.ini          # Defines target servers
    ├── playbook.yml           # Main playbook — orchestrates all roles
    └── roles/
        ├── webserver/         # Installs and configures Nginx
        │   └── tasks/
        │       └── main.yml
        ├── mysql/             # Installs MySQL + manages encrypted credentials
        │   ├── tasks/
        │   │   └── main.yml
        │   └── vars/
        │       └── secrets.yml  # 🔐 Encrypted with Ansible Vault
        └── app/               # Creates app directory + deploys index page
            └── tasks/
                └── main.yml
```

---

## ⚙️ Prerequisites

- Python 3.x
- Ansible (`pip install ansible`)
- Git

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/nkydigitech/ansible_practical.git
cd ansible_practical/ansible-project
```

### 2. Install Ansible
```bash
pip install ansible
ansible --version
```

### 3. Run the playbook
```bash
# Standard run
ansible-playbook -i inventory.ini playbook.yml

# With Ansible Vault (required for MySQL role)
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass
```

### 4. Dry run (no changes made)
```bash
ansible-playbook -i inventory.ini playbook.yml --check
```

---

## 🎭 Roles Breakdown

### 🌐 webserver
Installs and ensures Nginx is present on the target server.
- Module used: `apt`
- Demonstrates: idempotency (run twice, second run shows `changed=0`)

### 🗄️ mysql
Installs MySQL server, ensures it's running and enabled on boot.
- Loads encrypted credentials via `include_vars`
- Credentials stored in `vars/secrets.yml` — encrypted with **Ansible Vault**
- Demonstrates: secret management, service management

### 📦 app
Creates the application directory and deploys a basic index page.
- Module used: `file`, `copy`
- Demonstrates: file management, content deployment

---

## 🔐 Ansible Vault

Sensitive credentials (database passwords, usernames) are encrypted using Ansible Vault.

```bash
# View/edit encrypted file
ansible-vault edit roles/mysql/vars/secrets.yml

# Encrypt a new file
ansible-vault encrypt path/to/file.yml

# Run playbook with vault password
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass
```

**Golden Rule:** Never commit plain text passwords. Always use Ansible Vault. 🔒

---

## 💡 Key Concepts Demonstrated

| Concept | Where to find it |
|---------|-----------------|
| Idempotency | Run the playbook twice and observe `changed=0` |
| Roles | `roles/` directory structure |
| Variables | `roles/mysql/vars/secrets.yml` |
| Vault encryption | `--ask-vault-pass` flag |
| Service management | `roles/mysql/tasks/main.yml` |
| File management | `roles/app/tasks/main.yml` |

---

## 📚 Learning Resources

- [Ansible Official Docs](https://docs.ansible.com/)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Ansible Vault Guide](https://docs.ansible.com/ansible/latest/vault_guide/)

---

## 👩🏽‍💻 About

Built by **Nkechi Anna Ahanonye** — DevOps Engineer & Educator.

Currently teaching Ansible to the next generation of DevOps engineers. 💪

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/nkechi-anna-ahanonye)

---

*Happy automating!* ⚡
