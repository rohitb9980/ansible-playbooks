# Ansible Notes and Interview Questions

## Ansible Basics

### What is Ansible?
Ansible is an open-source automation tool used for:
- Configuration management
- Application deployment
- Infrastructure provisioning
- Orchestration

### Key Features
- Agentless
- Uses SSH
- YAML-based playbooks
- Idempotent operations
- Easy to learn

---

## Ansible Components

### Task
A single unit of work executed by Ansible.

### Module
Reusable code unit executed by Ansible.

### Inventory
Defines hosts and groups.

### Play
Collection of tasks executed on hosts.

### Playbook
Collection of plays written in YAML.

### Role
Reusable structure for organizing automation content.

---

## Inventory Example

```ini
[web]
server1 ansible_host=172.31.10.11
server2 ansible_host=172.31.10.12

[db]
server3 ansible_host=172.31.10.13
```

---

## Basic Playbook

```yaml
---
- name: Install nginx
  hosts: web
  become: yes

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes
```

---

## Useful Commands

### Install Ansible

```bash
sudo apt update
sudo apt install ansible -y
```

### Check Version

```bash
ansible --version
```

### Ping Hosts

```bash
ansible all -m ping
```

### Run Command

```bash
ansible all -a "uptime"
```

### Run Playbook

```bash
ansible-playbook site.yml
```

---

## Variables Example

```yaml
vars:
  package_name: nginx
```

Usage:

```yaml
name: "{{ package_name }}"
```

Default value:

```yaml
name: "{{ package_name | default('nginx') }}"
```

---

## Conditionals

```yaml
when: ansible_os_family == "Debian"
```

---

## Loops

```yaml
loop:
  - nginx
  - git
  - vim
```

---

## Handlers

```yaml
handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```

---

## Ansible Vault

### Create Vault

```bash
ansible-vault create secrets.yml
```

### Edit Vault

```bash
ansible-vault edit secrets.yml
```

### Run Playbook with Vault

```bash
ansible-playbook site.yml --ask-vault-pass
```

---

## Role Structure

```text
roles/
└── nginx/
    ├── tasks/
    ├── handlers/
    ├── templates/
    ├── files/
    ├── vars/
    ├── defaults/
    └── meta/
```

---

## Best Practices

- Use roles
- Keep playbooks idempotent
- Use meaningful task names
- Avoid shell module when possible
- Use dynamic inventory for cloud
- Store secrets in Ansible Vault

---

## Facts

Facts are system information gathered automatically by Ansible.

View facts:

```bash
ansible all -m setup
```

---

## Become Directive

Used for privilege escalation.

```yaml
become: yes
```

---

## Dynamic Inventory

Dynamic inventory fetches hosts automatically from cloud providers like AWS.

---

## Strategies

### Linear
Default strategy.

### Free
Runs tasks independently per host.

### Debug
Interactive execution.

---

## Error Handling

```yaml
ignore_errors: yes
```

Using blocks:

```yaml
block:
  - name: Install nginx
    apt:
      name: nginx
      state: present

rescue:
  - debug:
      msg: "Installation failed"
```

---

## Collections

Collections package:
- Modules
- Roles
- Plugins
- Documentation

Install collection:

```bash
ansible-galaxy collection install amazon.aws
```
