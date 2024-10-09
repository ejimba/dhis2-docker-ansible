
# DHIS2 Docker Configuration with Ansible (Multiple DHIS2 Instances)

This repository provides an Ansible playbook and inventory file for configuring a multiple DHIS2 Docker environment with letsencrypt certificates.

## Overview of Files
- **playbook.yaml**: The main Ansible playbook that outlines tasks to be performed.
- **inventory.yaml**: Lists the hosts where the playbook will be executed.

## Prerequisites

### Installing Ansible

You must have Ansible installed on your local machine to use the playbook.

#### For macOS:
```bash
brew install ansible
brew install hudochenkov/sshpass/sshpass
```

#### For Ubuntu:
```bash
sudo apt update
sudo apt install ansible
sudo apt-get install sshpass
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

### Update your DHIS2 instances configurations
Update the `dhis2_instances` variables under the `vars` section in the playbook.yaml file with the domains, usernames, and passwords. To adjust the amount of RAM available to DHIS2, modify the `java_opts` setting (min max). Remove unnecessary instances. By default, we have three instance, 2 with domains, 1 without. Remove any unnecessary instances. You should have at least one instance. 

```yaml
---
- hosts: dhis
  become: yes
  vars:
    dhis2_instances:
      - instance_name: dhis2_instance1
        domain: "instance1.yourdomain.com"
        email: "instance1@yourdomain.com"
        port: "8080"
        ssl_enabled: "true"
        timezone: "Africa/Nairobi"
        java_opts: "-Xms2048m -Xmx4096m"
        postgres_host: "dhis2_instance1_postgres"
        postgres_port: "5432"
        postgres_db: "dhis2_db1"
        postgres_user: "dhis_user1"
        postgres_password: "dhis_password1"

      - instance_name: dhis2_instance2
        domain: null
        email: null
        port: "8081"
        ssl_enabled: "false"
        timezone: "Africa/Nairobi"
        java_opts: "-Xms4096m -Xmx4096m"
        postgres_host: "dhis2_instance2_postgres"
        postgres_port: "5432"
        postgres_db: "dhis2_db2"
        postgres_user: "dhis_user2"
        postgres_password: "dhis_password2"

      - instance_name: dhis2_instance3
        domain: "instance3.yourdomain.com"
        email: "instance3@yourdomain.com"
        port: "8082"
        ssl_enabled: "true"
        timezone: "Africa/Nairobi"
        java_opts: "-Xms1024m -Xmx2048m"
        postgres_host: "dhis2_instance3_postgres"
        postgres_port: "5432"
        postgres_db: "dhis2_db3"
        postgres_user: "dhis_user3"
        postgres_password: "dhis_password3"

```

## Running the Playbook
Execute the playbook with the following command:

```bash
ansible-playbook -i inventory.yaml playbook.yaml
```

## Credits
Used a number of open source containers:
- [DHIS2 Core](https://github.com/dhis2/dhis2-core)
- [PostGIS Docker](https://github.com/postgis/docker-postgis)
- [Nginx Proxy](https://github.com/nginx-proxy/nginx-proxy)
- [Let's Encrypt Nginx Proxy Companion](https://github.com/jwilder/docker-letsencrypt-nginx-proxy-companion)
- [Ansible](https://github.com/ansible/ansible)
- [Docker Compose](https://github.com/docker/compose)
