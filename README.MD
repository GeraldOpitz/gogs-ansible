# gogs-ansible - Configuration Management for Gogs

Ansible playbooks and roles to configure and deploy **Gogs (Go Git Service)** on provisioned infrastructure.

This repository complements the Terraform infrastructure project by handling **configuration management**, **application deployment**, and **service management** using Ansible.

---

## Project Overview

This repository contains Ansible code to install, configure, and manage a Gogs server after the infrastructure has been provisioned (for example using Terraform).

Main objectives:

- Automate Gogs installation and configuration
- Enforce idempotent server configuration
- Separate responsibilities between infrastructure (Terraform) and configuration (Ansible)
- Support integration with CI/CD pipelines (e.g. Jenkins)
- Enable repeatable and predictable deployments

---

## Repository Structure

```
gogs-ansible/
├── inventories/
│   ├── dev/
│   │   └── inventory.ini
│   └── prod/
│       └── inventory.ini
├── roles/
│   ├── common/
│   ├── gogs/
│   └── security/
├── playbooks/
│   └── site.yml
├── ansible.cfg
└── README.md
```

---

## Folder Description

### inventories/

Contains environment-specific inventories.

Each inventory defines:
- Target hosts
- SSH connection parameters
- Grouping of servers

Example:

```ini
[gogs]
gogs_server ansible_host=1.2.3.4 ansible_user=ubuntu
```

---

### roles/

Reusable Ansible roles that define configuration logic.

Typical roles include:

- common: base system configuration (packages, users, timezone)
- gogs: installation and configuration of Gogs
- security: firewall rules, permissions, hardening

Each role follows standard Ansible role structure:

```
tasks/
handlers/
templates/
files/
vars/
defaults/
```

---

### playbooks/

Contains high-level playbooks that orchestrate role execution.

Main playbook:

- site.yml: applies all required roles to the target hosts

Example:

```yaml
- hosts: gogs
  become: true
  roles:
    - common
    - gogs
    - security
```

---

## Pre-requisites

Before running this project, ensure you have:

- Ansible 2.10 or higher
- SSH access to target servers
- Python installed on target hosts
- Inventory files correctly configured


---

## Usage

### Test connectivity

```bash
ansible -i inventories/dev/inventory.ini all -m ping
```

---

### Run the main playbook

```bash
ansible-playbook -i inventories/dev/inventory.ini playbooks/site.yml
```

---

### Run for production

```bash
ansible-playbook -i inventories/prod/inventory.ini playbooks/site.yml
```

---

## Best Practices Implemented

- Idempotent configuration
- Role-based architecture
- Environment separation (dev / prod)
- Clear separation of infrastructure and configuration
- Ready for CI/CD automation

---

## Integration with Terraform

This repository is designed to work together with:

- Terraform (infrastructure provisioning)
- Jenkins (pipeline orchestration)

Typical workflow:

1. Terraform provisions infrastructure
2. Terraform outputs host IPs
3. Ansible inventory is generated dynamically or updated
4. Ansible configures and deploys Gogs

---

## Contributing

Contributions are welcome.

Steps:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Open a Pull Request
