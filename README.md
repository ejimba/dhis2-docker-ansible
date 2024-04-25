
# DHIS2 Docker Configuration with Ansible

This repository provides an Ansible playbook and inventory file for configuring a DHIS2 Docker environment.

## Overview of Files
- **playbook.yaml**: The main Ansible playbook that outlines tasks to be performed.
- **inventory.yaml**: Lists the hosts where the playbook will be executed.

## Prerequisites

### Installing Ansible

You must have Ansible installed on your local machine to use the playbook.

#### For macOS:
```bash
brew install ansible
```

#### For Ubuntu:
```bash
sudo apt update
sudo apt install ansible
```

### Configuring Access Credentials
Update the `hosts` and `vars` sections in the playbook.yaml file with the appropriate IP address, username, and password.

```yaml
  hosts:
    dhis_01:
      ansible_host: 000.000.000.000
  vars:
    ansible_ssh_user: your_username
    ansible_ssh_pass: your_password
```

### Setting Resource Limits for DHIS2
To adjust the amount of RAM available to DHIS2, modify the JAVA_OPTS setting. The default is set at 2GB, and the example below increases it to 10GB.

```yaml
line: '      JAVA_OPTS: -Xms10240m -Xmx10240m'
```

## Running the Playbook
Execute the playbook with the following command:

```bash
ansible-playbook -i inventory.yaml playbook.yaml
```
