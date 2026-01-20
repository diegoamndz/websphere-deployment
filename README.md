# WebSphere Deployment Automation

Automated deployment pipeline for WebSphere applications using Ansible. This project demonstrates how Ansible can orchestrate complex Java application deployments, even though it's not natively designed for it.

## Overview

This was part of a larger CI/CD pipeline integrating **Ansible + Jenkins + Bitbucket + Jira**. My focus was the Ansible automation layer and its integration with the pipeline tools.

**What it does:**
- Compiles Java source code with proper encoding
- Packages applications into WAR files
- Creates timestamped backups before deployment
- Deploys to WebSphere using native wsadmin commands
- Provides quick rollback capabilities

**The Flow:**
```
Bitbucket Push → Jenkins Trigger → Ansible Playbook → WebSphere Deployment
                                          ↓
                                    Jira Update
```

## Project Structure
```
├── playbooks/
│   ├── deploy.yml      # Main deployment orchestration
│   └── rollback.yml    # Rollback to previous version
├── roles/
│   ├── java-compiler/  # Handles source compilation
│   ├── websphere-deploy/  # WebSphere integration via wsadmin
│   └── backup-manager/ # Backup creation and restoration
├── inventory/
│   ├── hosts           # Server definitions
│   └── group_vars/     # Environment variables
└── vars/
    └── deployment.yml  # Deployment-specific config
```

## Quick Start

**1. Configure your environment**

Edit `inventory/hosts`:
```ini
[websphere_servers]
prod-was ansible_host=10.0.1.50

[build_servers]
build-01 ansible_host=10.0.1.100
```

Edit `inventory/group_vars/all.yml`:
```yaml
app_name: myapp
context_root: /myapp
was_username: wasadmin
was_cell: Cell01
was_node: Node01
source_base_dir: /opt/source
backup_dir: /opt/backups
```

**2. Deploy**
```bash
# Full deployment
ansible-playbook playbooks/deploy.yml

# Specific environment
ansible-playbook playbooks/deploy.yml -l production

# Rollback if needed
ansible-playbook playbooks/rollback.yml
```

## How It Works

**Compilation Phase** (java-compiler role)
- Pulls source from specified directory
- Compiles with proper Java encoding
- Handles classpath dependencies

**Packaging** (java-compiler role)
- Creates WAR structure
- Packages compiled classes
- Validates archive integrity

**Backup** (backup-manager role)
- Creates timestamped backup of current deployment
- Stores in backup directory
- Keeps configurable retention

**Deployment** (websphere-deploy role)
- Connects to WebSphere via wsadmin
- Executes deployment commands
- Validates deployment status

**Rollback** (backup-manager + websphere-deploy)
- Retrieves latest backup
- Redeploys previous version
- Confirms application startup

## CI/CD Integration

This Ansible layer was designed to integrate with:

- **Jenkins**: Called as a build step in Jenkinsfile
- **Bitbucket**: Triggered on merge to specific branches
- **Jira**: Updated deployment status via Jenkins (not directly from Ansible)

The production version included more sophisticated error handling and reporting back to the pipeline.

## Requirements

- Ansible 2.9+
- Java JDK on build servers
- WebSphere Application Server with wsadmin access
- SSH access to target servers

## Security Note

Use Ansible Vault for credentials:
```bash
ansible-vault create inventory/group_vars/vault.yml
ansible-playbook playbooks/deploy.yml --ask-vault-pass
```

## Context

This is a simplified version of what I implemented professionally. The production system had additional features like:
- Multi-environment deployments (dev/qa/prod)
- Notification integrations
- Advanced health checks
- Deployment approval workflows

The goal was to show how Ansible can handle complex Java deployments despite not being its primary use case.
