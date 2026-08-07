🚀 Ansible Practical — by Nkechi Ahanonye

    "Infrastructure as code means your servers are only as good as your playbooks."

A hands-on Ansible project built from scratch as part of a full Ansible Masterclass. This repository covers real-world infrastructure automation using roles, variables, handlers, templates, Ansible Vault and a production-ready CI/CD Pipeline.

[Ansible CI/CD Pipeline](https://github.com/nkydigitech/ansible_practical/actions)

Live Learning: 📖 Full 15-Module Guide | 🧪 Student Lab | 💼 LinkedIn
Why I Built This

Junior engineers SSH into prod and break things. Senior engineers use a pipeline. This repo teaches you the senior way: Lint → Dry-Run → Deploy. If GitHub Actions is green, it's safe to deploy. If red, you fix it before production breaks. That 1-minute pipeline has saved me from pushing broken configs more times than I can count.
📁 Project Structure
Code

ansible_practical/
├──.github/
│ └── workflows/
│ └── ci.yml # 6-check pipeline: yamllint, ansible-lint, shellcheck, syntax-check, check --diff
└── ansible-project/
    ├── inventory.ini # Your REAL servers (prod/staging)
    ├── inventory-ci.yml # For CI only - uses localhost, no SSH needed
    ├── playbook.yml # Main playbook - orchestrates all roles
    └── roles/
        ├── webserver/ # Installs and configures Nginx
        ├── mysql/ # Installs MySQL + encrypted credentials
        │ └── vars/
        │ └── secrets.yml # 🔐 Encrypted with Ansible Vault
        └── app/ # Creates app directory + deploys index page

⚙️ Prerequisites

    Python 3.11+
    Ansible (pip install ansible)
    Git

🚀 Getting Started - Zero to Green in 5 Minutes

Step 1: Clone
Bash

git clone https://github.com/nkydigitech/ansible_practical.git
cd ansible_practical/ansible-project

Step 2: Fix The Vault File - You Must Do This After Clone/Fork
roles/mysql/vars/secrets.yml is encrypted with Ansible Vault for security. I cannot share the password. You need to create your own.
Bash

rm roles/mysql/vars/secrets.yml
cat > roles/mysql/vars/secrets.yml << EOF
db_name: myappdb
db_user: myuser
db_password: SuperSecret123!
EOF
ansible-vault encrypt roles/mysql/vars/secrets.yml
# When prompted, set password: devops123 - remember it

Note for Forks: GitHub Actions will stay green automatically after you fork because the pipeline creates dummy secrets for CI. You only need Step 2 for running locally on your laptop.

Step 3: Test Locally - The Safe Way
Bash

# 1. Check syntax - fails fast if typo
ansible-playbook -i inventory-ci.yml playbook.yml --syntax-check

# 2. Dry-run - shows WHAT would change, makes no changes
ansible-playbook -i inventory-ci.yml playbook.yml --check --diff --ask-vault-pass
# Enter password: devops123

# 3. Real run to your servers
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass

Step 4: What To Expect
When you run it, you will see:
Code

TASK [webserver : Install Nginx] ok: [localhost]
TASK [mysql : Load vault secrets] ok: [localhost]
TASK [app : Deploy index page] ok: [localhost]
PLAY RECAP ****************************************************************
localhost : ok=3 changed=0 unreachable=0 failed=0

changed=0 on second run = Idempotency. This is Ansible's superpower. Run twice, second run does nothing because server is already correct.

Step 5: Push and Get Your Green Badge
Push to main branch, go to Actions tab. You will see:
Code

1. yamllint - ok
2. ansible-lint - ok
3. shellcheck - ok
4. syntax-check - ok
5. check --diff - ok
6. Pipeline Finished - ✅ All 6 checks passed - safe to deploy

Screenshot that green run for your portfolio.
🎭 Roles Breakdown

🌐 webserver - Installs and ensures Nginx is present. Module: apt. Demonstrates: idempotency.

🗄 mysql - Installs MySQL server, ensures running and enabled on boot. Loads encrypted credentials via include_vars. Demonstrates: secret management, service management.

📦 app - Creates application directory and deploys a basic index page. Modules: file, copy. Demonstrates: file management, content deployment.
🔍 The 6 Checks Explained
	

Check
	

What It Catches
	

Why It Matters

1
	

yamllint
	

Broken YAML - missing spaces, wrong indentation
	

YAML is picky, one space breaks prod

2
	

ansible-lint
	

Bad Ansible practices, deprecated modules
	

Teaches you to write like a senior

3
	

shellcheck
	

Bad bash - rm -rf \$var disasters
	

If you have shell scripts

4
	

syntax-check
	

Typos in playbook.yml
	

Fails in 2 seconds, not 2 minutes

5
	

check --diff
	

Shows WHAT would change before changing
	

The most important - safe preview

6
	

Pipeline Finished
	

Proof
	

Your deploy confidence
🔐 Ansible Vault - Golden Rule

Never commit plain text passwords. Always use Ansible Vault.
Bash

# View/edit encrypted file
ansible-vault edit roles/mysql/vars/secrets.yml

# Encrypt a new file
ansible-vault encrypt path/to/file.yml

# Run playbook with vault password
ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass

😰 Common Errors - Fixed

Error: Attempting to decrypt but no vault secrets found - You skipped Step 2. Delete my encrypted file and create yours.

Error: var-naming - Already fixed in pipeline with --skip-list var-naming. Pipeline will not fail on style.

Error: inventory tries to SSH and fails - Use inventory-ci.yml for local tests. It has ansible_connection=local so it runs on your laptop, not trying to SSH.
💡 Key Concepts Demonstrated

Concept
	

Where To Find It

Idempotency
	

Run playbook twice, observe changed=0

Roles
	

roles/ directory structure

Variables
	

roles/mysql/vars/secrets.yml

Vault Encryption
	

--ask-vault-pass flag

CI/CD Pipeline
	

.github/workflows/ci.yml

Dry-Run
	

--check --diff flag
📚 Keep Learning

    New to Ansible? Start here: Student Lab - 4 hands-on labs, 30 mins
    Want mastery? Full course: 15 Modules Guide - with real terminal output
    Official: Ansible Docs | Ansible Galaxy

👩🏽‍💻 About Me

Built by Nkechi Ahanonye - DevOps Engineer, Cloud Enthusiast and Educator passionate about making infrastructure automation simple for beginners.

I teach Ansible, Terraform and CI/CD to the next generation of DevOps engineers through hands-on, project-based learning. No fluff, just real-world projects that get you hired.

Let's connect on LinkedIn - I share daily DevOps tips and break down complex topics into simple guides.

Star ⭐ this repo if it stopped you from SSH-ing into prod!

Happy automating! ⚡️
