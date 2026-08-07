# 🚀 Ansible Practical — by Nkechi Ahanonye
### Cloud & DevOps Engineer | I turn manual, 3 AM-breaking deployments into 1-min automated pipelines with AWS + Ansible + Terraform | Featured: 15-Module Ansible Lab with real terminal

[![Ansible CI/CD Pipeline](https://github.com/nkydigitech/ansible_practical/actions/workflows/cicd.yml/badge.svg)](https://github.com/nkydigitech/ansible_practical/actions)

> "Infrastructure as code means your servers are only as good as your playbooks."

**Live Learning:** [📖 Full 15-Module Guide](https://nkydigitech.github.io/ansible-guide/) | [🧪 Student Lab](https://nkydigitech.github.io/ansible-lab/) | [💼 LinkedIn](https://www.linkedin.com/in/nkechi-ahanonye)

A hands-on Ansible project built from scratch as part of a full Ansible Masterclass. Covers real-world infrastructure automation using roles, variables, handlers, templates, **Ansible Vault and a production-ready CI/CD Pipeline.**

---

### Why I Built This

Junior engineers SSH into prod and break things. Senior engineers use a pipeline. This repo teaches you the senior way: **Lint → Dry-Run → Deploy.** 

If GitHub Actions is green ✅, it's safe to deploy. If red ❌, you fix it before production breaks. That 1-minute pipeline has saved me from pushing broken configs more times than I can count.

---

### 📁 Project Structure

```bash
ansible_practical/
├── .github/
│   └── workflows/
│       └── ci.yml                # 6-check pipeline
└── ansible-project/
    ├── inventory.ini             # Your REAL servers
    ├── inventory-ci.yml          # For CI only - localhost, no SSH
    ├── playbook.yml              # Main playbook
    └── roles/
        ├── webserver/            # Installs Nginx
        ├── mysql/                # Installs MySQL + Vault secrets
        │   └── vars/
        │       └── secrets.yml   # 🔐 Encrypted with Ansible Vault
        └── app/                  # Deploys app
```

---

### ⚙️ Prerequisites

- Python 3.11+
- Ansible (`pip install ansible`)
- Git

---

### 🚀 Getting Started - Zero to Green in 5 Minutes

**Step 1: Clone**

```bash
git clone https://github.com/nkydigitech/ansible_practical.git
cd ansible_practical/ansible-project
```

**Step 2: Fix The Vault File - You Must Do This After Clone/Fork**

`roles/mysql/vars/secrets.yml` is encrypted. For security, I cannot share the password.

```bash
rm roles/mysql/vars/secrets.yml

cat > roles/mysql/vars/secrets.yml << EOF
db_name: myappdb
db_user: myuser
db_password: SuperSecret123!
EOF

ansible-vault encrypt roles/mysql/vars/secrets.yml
# Set password: devops123
```

> **Forked?** GitHub Actions will stay green automatically. CI creates dummy secrets for itself. You only need Step 2 for local runs.

**Step 3: Test Locally - The Safe Way**

```bash
# 1. Fail fast check
ansible-playbook -i inventory-ci.yml playbook.yml --syntax-check

# 2. Dry-run - shows WHAT would change, makes no changes
ansible-playbook -i inventory-ci.yml playbook.yml --check --diff --ask-vault-pass

# 3. Real run
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass
```

**Step 4: What To Expect**

```
TASK [webserver : Install Nginx] ok: [localhost]
TASK [mysql : Load vault secrets] ok: [localhost]
TASK [app : Deploy index page] ok: [localhost]
PLAY RECAP: localhost ok=3 changed=0 failed=0
```

`changed=0` on second run = Idempotency working. This is Ansible's superpower.

**Step 5: Push and Get Green Badge**

Push to `main` → Actions tab → You should see all 6 checks green:

`yamllint → ansible-lint → shellcheck → syntax-check → check --diff → Pipeline Finished ✅`

---

### 🎭 Roles Breakdown

**🌐 webserver** - Installs Nginx. Module: `apt`. Teaches idempotency.

**🗄 mysql** - Installs MySQL, manages service. Loads encrypted vars. Teaches Vault + service management.

**📦 app** - Creates app directory and deploys index page. Teaches file management.

---

### 🔍 The 6 Checks in ci.yml

| # | Check | What It Catches |
|---|---|---|
| 1 | yamllint | Broken YAML indentation |
| 2 | ansible-lint | Bad Ansible practices |
| 3 | shellcheck | Bad bash scripts |
| 4 | syntax-check | Typos - fails in 2 seconds |
| 5 | check --diff | Shows WHAT would change, no real change |
| 6 | Pipeline Finished | Deploy confidence proof |

---

### 🔐 Ansible Vault

```bash
ansible-vault edit roles/mysql/vars/secrets.yml
ansible-vault encrypt path/to/file.yml
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass
```

Golden Rule: Never commit plain text passwords. Always use Vault. 🔒

---

### 😰 Common Errors - Fixed

**`Attempting to decrypt but no vault secrets found`** - You skipped Step 2. Create your own secrets.yml.

**`inventory tries to SSH`** - Use `inventory-ci.yml` for local tests. It has `ansible_connection=local`.

**`var-naming`** - Already fixed in pipeline with `--skip-list var-naming`.

---

### 💡 Key Concepts

| Concept | Where |
|---|---|
| Idempotency | Run playbook twice, `changed=0` |
| Roles | `roles/` directory |
| Vault | `--ask-vault-pass` |
| CI/CD | `.github/workflows/ci.yml` |
| Dry-Run | `--check --diff` |

---

### 📚 Keep Learning

- **New?** [Student Lab - 4 labs, 30 mins](https://nkydigitech.github.io/ansible-lab/)
- **Serious?** [15 Modules Guide](https://nkydigitech.github.io/ansible-guide/)
- **Official:** [Ansible Docs](https://docs.ansible.com/)

---

### 👩🏽‍💻 About Me

**Nkechi Ahanonye**
Cloud & DevOps Engineer | I turn manual, 3 AM-breaking deployments into 1-min automated pipelines with AWS + Ansible + Terraform

I teach Ansible, Terraform and CI/CD to the next generation of DevOps engineers through hands-on, project-based learning. No fluff, just real-world projects that get you hired.

[LinkedIn](https://www.linkedin.com/in/nkechi-ahanonye) | [GitHub](https://github.com/nkydigitech)

**Star ⭐ this repo if it stopped you from SSH-ing into prod!**

Happy automating! ⚡
