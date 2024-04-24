---
tags:
  - DevOps
date: 2024-04-23T17:40
---
<!-- 2024-04-23-1740 (April 23, 2024 05:40:46 PM) -->

# Ansible from Beginner to Advanced
- way to represent data like JSON, XML, etc.
- for IT automation and configuration management
- uses YAML
- agentless
- only needs ssh (for Linux/Unix machines) or winrm (for Windows machines)
![[Pasted image 20240423225037.png]]

## Configuration Files
- ordering is from specifics to general (as default):
```
- ANSIBLE_CONFIG (environment variable if set)
- ansible.cfg (in the current directory)
- ~/.ansible.cfg (in the home directory)
- /etc/ansible/ansible.cfg
```
- example:
  1. `/opt/something-playbook/ansible.cfg`
    - examples:
      - `/opt/web-playbook/ansible.cfg`
      - `/opt/db-playbook/ansible.cfg`
      - `/opt/network-playbook/ansible.cfg`
  2. `~/.ansible.cfg`
  3. `~/.ansible/`
  4. `/etc/ansible/ansible.cfg`


### default config file:
- `/etc/ansible/ansible.cfg`
  - for system-wide config files
- RECOMMENDED: `~/.ansible` or `~/ansible-proj` or in `~/dotfiles/ansible` (stored in home directory)
  - the "best" way to store config files (more organized, easy git)
  ```
  /home/user/.ansible/
  ├── inventories/
  ├── playbooks/
  ├── roles/
  ├── group_vars/
  ├── host_vars/
  └── README.md
  ```

### hosts
`/etc/ansible/hosts`:
- can be put inside `inventory` directory like this: `/etc/ansible/inventory/hosts`
- where all servers are (to be managed)
- sample:
```hosts
[archlinux]
server-1
server-2
server-3
eminence

[ubuntu]
hostname-01
hostname-02
hostname-03
db-web
db-internal
```

## YAML
- indentation is important
- are mostly key-value pairs
- can have dicts inside lists and all vice versa

- lists/arrays:
  - are denoted with "-"
```yaml
Color:
  - Red
  - Green
  - Blue
  - Yellow

```

- dictionaries/map:
  - key-value pair
```yaml
# Given a Car-related YAML config file
Color: Red
Model: # this is a dictionary/map
  Name: Corvette
  Year: 1995
Transition: Manual
Price: $20,000
```

## Ansible Inventory
<!-- TODO: fill info about ansible inventory  -->
- where ...

## Ansible Playbooks
- use YAML
### Variables
- denoted with `'{{ var_name }}'`
- can have separated file dedicated for variables
![[Pasted image 20240424034338.png]]
![[Pasted image 20240424034329.png]]

# More References and Resources
- [Ansible - Arch Wiki](https://wiki.archlinux.org/title/Ansible)
- [Ansible Beginners](https://www.youtube.com/watch?v=w9eCU4bGgjQ&t=0s)
- [Ansible for Homelab](https://www.youtube.com/watch?v=yoFTL0Zm3tw)
- [Techdufus dotfiles](https://github.com/techdufus/dotfiles)
- [Techdufus k9s setup via Ansible](https://github.com/TechDufus/dotfiles/blob/main/roles/k9s/handlers/main.yml)
- [DevEnv Dotfiles Manjaro i3 via Ansible](https://github.com/ALT-F4-LLC/dotfiles?tab=readme-ov-file#dotfiles)
- [dotfiles in yaml via ansible](https://github.com/logandonley/dotfiles/blob/main/dot_bootstrap/setup.yml)
