# Ansible Cheat sheet 

Key Highlights:
- Ansible is open-source, IT automation engine that automates cloud provisioning, intra-service orchestration, configuration management, application deployment, and many other IT needs in day to day life in Software engineer.
- Ansible is agentless configuration management tool.
- Ansible is Idempotent in Nature.
- Ansible works on Push Mechanism and uses SSH.
- Ansible Playbooks are written in YAML.

## What is Playbook: 

An Ansible playbook is a YAML file containing a series of tasks and configurations, written in a human-readable format. It defines the automation steps to be executed on target hosts, facilitating infrastructure management and application deployment using Ansible.

## Playbooks

Defination: YAML files defining automation tasks.

Example:

```---
- name: Install Apache
  hosts: web
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Apache
      apt:
        name: apache2
        state: present
```

## Playbook Execution:
Syntax: ansible-playbook <playbook.yml>
Example:
```
ansible-playbook site.yml

```

## Variables and Facts:
Variables:
Definition: Store reusable values.
Example:
```
---
vars:
  http_port: 80

```
Facts:
Definition: Information about the target system.
Example:
```
---
tasks:
  - name: Display Facts
    debug:
      var: ansible_facts
```

## Handlers:
Definition: Tasks triggered by other tasks.
Example:
```
---
tasks:
  - name: Restart Apache
    service:
      name: apache2
      state: restarted
    notify: Restart Apache
  
handlers:
  - name: Restart Apache
    service:
      name: apache2
      state: restarted

```

## Roles:
Definition: Organized structure for tasks, variables, and handlers.
Example Directory Structure:
```
roles/
  common/
    tasks/
    handlers/
    vars/
    defaults/
```

## Modules:
Common Modules:
apt, yum, service, copy, file, etc.
Example:
```
---
tasks:
  - name: Install Nginx
    apt:
      name: nginx
      state: present
```

Custom Modules:
Example:
```
---
tasks:
  - name: Execute Custom Module
    custom_module:
      param1: value1
      param2: value2

```

## Ansible Vault:
Commands:
```
Encrypt: ansible-vault encrypt <file>
Decrypt: ansible-vault decrypt <file>
Edit: ansible-vault edit <file>
```

## Dynamic Inventory:
Definition: Use external scripts or plugins for dynamic host lists.
Example:
```
# ansible.cfg
[inventory]
enable_plugins = script

[inventory/external]
args = /path/to/inventory_script.sh
```

## Loops and Conditionals:
Loops:
Loop over a List:
```
---
tasks:
  - name: Loop over packages
    apt:
      name: "{{ item }}"
      state: present
    with_items:
      - nginx
      - apache2

```
Loop with range:
```
---
tasks:
  - name: Loop with range
    debug:
      msg: "Index is {{ item }}"
    with_sequence: start=0 end=5

```

Conditionals:
Simple Conditional:

```---
tasks:
  - name: Install package if Ubuntu
    apt:
      name: nginx
      state: present
    when: "'ubuntu' in ansible_distribution"
```

## Group Vars and Host Vars:
Group Vars:

Definition: Variables applied to a group of hosts.  
Location: group_vars/group_name.yml or specified in the playbook.  
Example:  
```
# group_vars/web_servers.yml
http_port: 8080
```

Host Vars:  
Definition: Variables applied to a specific host.  
Location: host_vars/host_name.yml or specified in the playbook.
Example:
```
# host_vars/server1.yml
app_version: "v1.2.3"
```

## Important Options when Running Playbooks:

Check Mode (Dry Run):

```
ansible-playbook --check <playbook.yml>
```

Limit Execution to Specific Host or Group:
```
ansible-playbook --limit <host_or_group> <playbook.yml>
```

Specify Inventory File:
```
ansible-playbook -i <inventory_file> <playbook.yml>
```

Set User for SSH Connections:
```
ansible-playbook -u <username> <playbook.yml>
```

Specify Private Key File:
```
ansible-playbook --private-key=<private_key_file> <playbook.yml>
```

Run a Specific Task by Name:
```
ansible-playbook <playbook.yml> --start-at-task="<task_name>"
```

Run Playbook in the Background:
```
ansible-playbook <playbook.yml> --forks=<number_of_parallel_processes>
```

Skip Tasks with Tags:
```
ansible-playbook <playbook.yml> --skip-tags "<tag_name>"
```

Run Playbook with Different User Privileges (sudo):
```
ansible-playbook <playbook.yml> --become --ask-become-pass
```

Specify Extra Variables (Overrides):
```
ansible-playbook <playbook.yml> -e "variable_name=value"
```

Prompt for Vault Password:
```
ansible-playbook <playbook.yml> --ask-vault-pass
```

Important links:
- https://collabnix.com/ansible-cheatsheet/