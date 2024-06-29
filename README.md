# Ansible role: labocbz.install_grafana

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Grafana](https://img.shields.io/badge/Tech-Grafana-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)

An Ansible role to install and configure Grafana on your host.

This Ansible role is designed to facilitate the installation and default configuration of Grafana, an open-source monitoring and data visualization platform. The role aims to simplify the deployment process while allowing for some basic customization.

By default, the role sets up Grafana in the "production" app mode. It configures data storage paths, specifying the location for Grafana's data and temp data with a specified lifetime. The protocol and port for communication are also established, with an option to enable TLS 1.3 for enhanced security.

Grafana is configured to listen on the specified IP address and port, accessible via the defined domain. The role supports serving Grafana from a sub-path, and it allows you to set up router logging and enable gzip compression.

The role takes into account SSL/TLS settings, providing the option to use SSL certificates for secure communication. However, client authentication is not configured by default.

It is noted that the default password remains unchanged after installation, as it depends on the target installation. The role advises reviewing the grafana.ini configuration present in ./templates before further customization.

In summary, the Ansible role streamlines the installation and basic configuration of Grafana, while allowing for limited customization of key parameters. It is recommended to review the default configuration file before making further adjustments to ensure alignment with your deployment needs.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  ---
inv_prepare_host__system_users:
  - login: "grafana"
    group: "grafana"

inv_install_grafana__app_mode: "production"
inv_install_grafana__data: "/var/lib/grafana"
inv_install_grafana__temp_data_lifetime: "24h"
inv_install_grafana__protocol: "https"
inv_install_grafana__min_tls_version: ""
inv_install_grafana__http_addr: "0.0.0.0"
inv_install_grafana__http_port: 3000
inv_install_grafana__domain: "localhost"
inv_install_grafana__enforce_domain: false
inv_install_grafana__root_url: "%(protocol)s://%(domain)s:%(http_port)s/"
inv_install_grafana__serve_from_sub_path: false
inv_install_grafana__router_logging: false
inv_install_grafana__enable_gzip: true
inv_install_grafana__cert_file: "/etc/grafana/ssl/my-grafana-server.domain.tld/my-grafana-server.domain.tld.pem.crt"
inv_install_grafana__cert_key: "/etc/grafana/ssl/my-grafana-server.domain.tld/my-grafana-server.domain.tld.pem.key"

inv_install_grafana__log_mode: "console file"
inv_install_grafana__log_level: "info"
inv_install_grafana__log_max_days: 7
# Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_grafana__app_mode: "production"
install_grafana__data: "/var/lib/grafana"
install_grafana__temp_data_lifetime: "24h"
install_grafana__protocol: "http"
install_grafana__min_tls_version: "" #TLS1.3
install_grafana__http_addr: "0.0.0.0"
install_grafana__http_port: 3000
install_grafana__domain: "localhost"
install_grafana__enforce_domain: false
install_grafana__root_url: "%(protocol)s://%(domain)s:%(http_port)s/"
install_grafana__serve_from_sub_path: false
install_grafana__router_logging: false
install_grafana__enable_gzip: true
install_grafana__cert_file: ""
install_grafana__cert_key: ""

install_grafana__log_mode: "console file"
install_grafana__log_level: "info"
install_grafana__log_max_days: 7

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host__system_users:
  - login: "grafana"
    group: "grafana"

inv_install_grafana__app_mode: "production"
inv_install_grafana__data: "/var/lib/grafana"
inv_install_grafana__temp_data_lifetime: "24h"
inv_install_grafana__protocol: "https"
inv_install_grafana__min_tls_version: ""
inv_install_grafana__http_addr: "0.0.0.0"
inv_install_grafana__http_port: 3000
inv_install_grafana__domain: "localhost"
inv_install_grafana__enforce_domain: false
inv_install_grafana__root_url: "%(protocol)s://%(domain)s:%(http_port)s/"
inv_install_grafana__serve_from_sub_path: false
inv_install_grafana__router_logging: false
inv_install_grafana__enable_gzip: true
inv_install_grafana__cert_file: "/etc/grafana/ssl/my-grafana-server.domain.tld/my-grafana-server.domain.tld.pem.crt"
inv_install_grafana__cert_key: "/etc/grafana/ssl/my-grafana-server.domain.tld/my-grafana-server.domain.tld.pem.key"

inv_install_grafana__log_mode: "console file"
inv_install_grafana__log_level: "info"
inv_install_grafana__log_max_days: 7

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_grafana"
  tags:
    - "labocbz.install_grafana"
  vars:
    install_grafana__app_mode: "{{ inv_install_grafana__app_mode }}"
    install_grafana__data: "{{ inv_install_grafana__data }}"
    install_grafana__temp_data_lifetime: "{{ inv_install_grafana__temp_data_lifetime }}"
    install_grafana__protocol: "{{ inv_install_grafana__protocol }}"
    install_grafana__min_tls_version: "{{ inv_install_grafana__min_tls_version }}"
    install_grafana__http_addr: "{{ inv_install_grafana__http_addr }}"
    install_grafana__http_port: "{{ inv_install_grafana__http_port }}"
    install_grafana__domain: "{{ inv_install_grafana__domain }}"
    install_grafana__enforce_domain: "{{ inv_install_grafana__enforce_domain }}"
    install_grafana__root_url: "{{ inv_install_grafana__root_url }}"
    install_grafana__serve_from_sub_path: "{{ inv_install_grafana__serve_from_sub_path }}"
    install_grafana__router_logging: "{{ inv_install_grafana__router_logging }}"
    install_grafana__enable_gzip: "{{ inv_install_grafana__enable_gzip }}"
    install_grafana__cert_file: "{{ inv_install_grafana__cert_file }}"
    install_grafana__cert_key: "{{ inv_install_grafana__cert_key }}"
    install_grafana__log_mode: "{{ inv_install_grafana__log_mode }}"
    install_grafana__log_level: "{{ inv_install_grafana__log_level }}"
    install_grafana__log_max_days: "{{ inv_install_grafana__log_max_days }}"
  ansible.builtin.include_role:
    name: "labocbz.install_grafana"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-15: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-15: System users

* Role can now use system users and address groups

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
