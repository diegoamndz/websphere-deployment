# WebSphere Deployment Automation

Ansible automation for WebSphere application deployment with Java compilation, backup management, and rollback capabilities.

## Features

- **Java Compilation**: Automatic compilation of source code with proper encoding
- **WAR Creation**: Package applications into deployable WAR files
- **Backup Management**: Automatic backup creation with timestamp
- **WebSphere Integration**: Native wsadmin commands for deployment
- **Rollback Support**: Quick rollback to previous versions
- **Error Handling**: Comprehensive error checking and validation

  ## Project Structure 
websphere-deployment/
├── playbooks/
│   ├── deploy.yml
│   └── rollback.yml
├── roles/
│   ├── java-compiler/
│   ├── websphere-deploy/
│   └── backup-manager/
├── inventory/
│   ├── hosts
│   └── group_vars/all.yml
└── vars/deployment.yml

## Prerequisites

- Ansible 2.9+
- Java Development Kit on build servers
- WebSphere Application Server with wsadmin access
- SSH access to target servers

## Quick Start

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/websphere-deployment.git
cd websphere-deployment

- Configure your environment
# Edit inventory
vi inventory/hosts
vi inventory/group_vars/all.yml

- Deploy application
ansible-playbook playbooks/deploy.yml
ansible-playbook playbooks/rollback.yml

- Configuration
- Required Variables
- Update these in inventory/group_vars/all.yml:
# Application
app_name: your_application_name
context_root: /your_app

# WebSphere
was_username: your_was_admin
was_password: your_secure_password
was_cell: your_cell_name
was_node: your_node_name

# Paths
source_base_dir: /path/to/source
backup_dir: /path/to/backups

- Server Inventory
- Update inventory/hosts:
[websphere_servers]
your_server ansible_host=192.168.1.100

[build_servers]
build_server ansible_host=192.168.1.101

- Usage Examples
- Standard Deployment

ansible-playbook playbooks/deploy.yml
ansible-playbook playbooks/deploy.yml --tags "compile,deploy"
ansible-playbook playbooks/deploy.yml -l production
ansible-playbook playbooks/rollback.yml
ansible-playbook playbooks/rollback.yml -e "confirm_rollback=false"

Security

Use Ansible Vault for sensitive variables
Store passwords in encrypted vault files
Implement proper SSH key management
Follow principle of least privilege

# Create encrypted variables
ansible-vault create inventory/group_vars/vault.yml

# Edit encrypted file
ansible-vault edit inventory/group_vars/vault.yml

# Run with vault password
ansible-playbook playbooks/deploy.yml --ask-vault-pass

#  Customization
This framework can be customized for your environment:

Java Packages: Update java_packages in roles/java-compiler/defaults/main.yml
Paths: Modify paths in inventory/group_vars/all.yml
WebSphere Settings: Adjust WebSphere configuration variables
Health Checks: Add custom application health validation
Notifications: Implement deployment notifications

#  Troubleshooting
Common Issues
Compilation Errors:

Check Java version and classpath
Verify source code encoding
Ensure proper file permissions

# WebSphere Connection Issues:

Verify wsadmin path and credentials
Check WebSphere server status
Validate network connectivity

# Deployment Failures:

Check application logs
Verify WAR file integrity
Review WebSphere deployment logs

# Debug Mode
ansible-playbook playbooks/deploy.yml -vvv


