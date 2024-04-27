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
- where hosts (`hosts` file) are

### hosts file
- all playbooks are executed in all hosts, when `hosts: all` is defined
  - this means you can specify a specific host with its name via `hosts`

`/etc/ansible/hosts`:
- can be put inside `inventory` directory like this: `/etc/ansible/inventory/hosts`
- where all servers are (to be managed)
- can have `host-level variables`
- sample:
```hosts
[archlinux]
server-1 ansible_host=172.20.1.100
server-2 ansible_host=172.20.1.101 dns_server=10.5.5.4
server-3 ansible_host=172.20.1.102
eminence

[ubuntu]
hostname-01
hostname-02
hostname-03
db-web
db-internal
```


## Variables
- denoted with `'{{ var_name }}'`
- can have separated file dedicated for variables
![[Pasted image 20240424034338.png]]
![[Pasted image 20240424034329.png]]

### Variable Precedence
- also ordered by specific first to general
- see full complete variable precedence (from lowest prio to highest prio):
```
Role Defaults - lowest prio (most general)
Group Vars
Host Vars
Host Facts
Play Vars
Role Vars
Include Vars
Set Facts
Extra Vars - highest priority (most specfic)
```
![[Pasted image 20240424180836.png]]

### Variable Scopes
- see complete `/etc/ansible/hosts` file (same as in image below):
```ini
web1 ansible_host=172.20.1.100
web2 ansible_host=172.20.1.101 dns_server=10.5.5.4
web3 ansible_host=172.20.1.102

[web_servers]
web1
web2
web3

[web_servers:vars]
dns_server=10.5.5.3
```
![[Pasted image 20240424175419.png]]

*on `/etc/ansible/hosts`, we have:*
#### 1. group-level variables
```ini
[web_servers:vars]
dns_server=10.5.5.3
```

#### 2. host-level or host scope variables:
- web2 will have a `dns_server=10.5.5.4` instead of `10.5.5.3` (defined on group-level `web_servers:vars`)
```ini
web1 ansible_host=172.20.1.100
web2 ansible_host=172.20.1.101 dns_server=10.5.5.4
web3 ansible_host=172.20.1.102
```

#### 3. play scope variables
- variables that are defined in specific plays
![[Pasted image 20240424191500.png]]
- here we have a `ntp_server` variable defined in the first play, which is only available in the FIRST PLAY ONLY

#### 4. GLOBAL-level variables like Extra Vars:
- highest precedence (highest priority)
- mostly defined when running `ansible-playbook` in CLI
  - passing variables as extra-vars/extra variables in the CLI when running a playbook
```bash
ansible-playbook playbook.yml --extra-vars "dns-server=10.5.5.6"
```

### Playbook Variables via `register`
- can have a host-file specific variable via `register`
  - lives as long as the playbook runs
- useful when using the `debug` module
- see example below:
  - the output of `shell: cat /etc/hosts` is stored in `register: result` (like piping)
  - then we use the `result` variable in debug module `var` to get data of `result.rc` (see more data variables below in sample output)
  - `rc` or `return code` -> means 0 if success, otherwise have an error
```yaml
---
- name: Check /etc/hosts file
  hosts: all
  tasks:
  - shell: cat /etc/hosts
    register: result

  - debug:
      var: result.rc

- name: Play2
  hosts: all
  tasks:
  - debug:
      var: result.rc
```
![[Pasted image 20240424181704.png]]

- sample output of `result` for context
  - NOTE THAT: different modules return different output format
  - below we see output of the `shell` module:
![[Pasted image 20240424182717.png]]

### Magic Variables
- useful when accessing a variable that is defined somewhere else.
- see more magic variables here in docs: https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html

*example scenario:*
- what if we want to access `dns_server` of host `web2` in `web1` and `web3`?
```ini
web1 ansible_host=172.20.1.100
web2 ansible_host=172.20.1.101 dns_server=10.5.5.4
web3 ansible_host=172.20.1.102

[web_servers]
web1
web2
web3

[web_servers:vars]
dns_server=10.5.5.3
```

*some magic variables / special variables*
- see full list in the [official special_variables docs](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html)

#### hostvars
- in a playbook, we can access the web2 dns_server with:
```yaml
'{{ hostvars['web2'].dns_server }}'
```
- more here (hostvars):
![[Pasted image 20240424193806.png]]

#### groups
- the values of the `[group_name]`
- example: `web1`, `web2`, `web3`
![[Pasted image 20240424193946.png]]

#### group_names
- defined in `[]`
- example: `[group_name]`
![[Pasted image 20240424194018.png]]

#### inventory_hostname
- the host names itself (at the top of the `hosts` file)
- example: `web1`, `web2`, `web3`, `server-1`
![[Pasted image 20240424194321.png]]

## Ansible Facts
- these are the *basic system information* values like:
  - System architecture, version of OS
  - processor details, memory details, serial numbers, etc.
  - host's network connectivity, different interfaces
  - IP addresses, MAC addresses, FQDN, etc.
  - Device information: Disks, Mounts, volumes, amount of space
  - date and time of the systems, and other settings

- *all of these are gathered using the `setup` module*
  - is stored in a variable called `ansible_facts`

**NOTE: is run every time a playbook is run automatically**
  - to gather information of the host
  - ONLY hosts defined in the PLAYBOOK (regardless of the INVENTORY) will be gathered (when gathering facts):
  ![[Pasted image 20240424200811.png]]

### How to use `ansible_facts`?
```yaml
---
- name: Print hello message
  hosts: all
  tasks:
  - debug:
      var: ansible_facts
```
![[Pasted image 20240424195802.png]]
![[Pasted image 20240424195812.png]]

### `gathering` variable and `gather_facts`

*what if you want Ansible to not gather facts?*
- use the `gather_facts: no`

*you can also configure the gathering variable in `ansible.cfg`:*
![[Pasted image 20240424200051.png]]
- `gathering = implicit` -> will gather facts AUTOMATICALLY, unless `gather_facts: False`
- `gathering = explicit` -> will not gather facts, unless `gather_facts: True`

**NOTE: the setting in a PLAYBOOK takes higher precedence than in `ansible.cfg`**

## Ansible Playbooks
- use YAML
- playbooks can be simple to complex:
![[Pasted image 20240425013424.png]]
**Familiarize:**
- *Playbook - a single YAML file*
- *Play - defines a set of activities (tasks) to be run on hosts*
- *Task - an action to be performed on the host*
  *├── execute a command*
  *├── run a script*
  *├── install a package*
  *└── shutdown/restart*

### Playbook
- all keys here are at `play-level` scope
example `playbook.yml`:
```yaml
--- 
name: Play 1
hosts: localhost
tasks:
  - name: Execute command 'date'
    command: date

  - name: Execute script on server
    script: test_script.sh

  - name: Install httpd service
    apt:
      name: httpd
      state: present

  - name: Start a web server
    service:
      name: httpd
      state: started
```

#### Tasks
- `tasks` should be in order since it is an array

*see same YAML `playbook.yml` but separated into 2 Plays:*
```yaml
--- 
name: Play 1
hosts: localhost
tasks:
  - name: Execute command 'date'
    command: date

  - name: Execute script on server
    script: test_script.sh

--- 
name: Play 2
hosts: localhost
tasks:
  - name: Install httpd service
    apt:
      name: httpd
      state: present

  - name: Start a web server
    service:
      name: httpd
      state: started
```

#### Hosts
- `hosts` can be anything from the inventory file
*NOTE: HOSTS SHOULD BE EXPLICITLY DEFINED IN THE INVENTORY FILE `~/.ansible/inventory/hosts`*
- `hosts` can have groups as a value
  - example: `hosts: db` -> selects every hosts in the `db` group
![[Pasted image 20240425015302.png]]

#### Modules
- running `ansible-doc -l` will list available modules (there are A LOT of modules, refer to the docs)
- running `ansible-doc -l` will list available modules (there are A LOT of modules, refer to the docs)
- these are the `apt`, `service`, `command`, `script`, `shell`, `debug`, etc.
![[Pasted image 20240425015739.png]]

#### How to Run an Ansible Playbook?
- just do: `ansible-playbook <playbook-name>.yml`
- see more options with: `ansible-playbook --help`

# More References and Resources
- [Ansible - Arch Wiki](https://wiki.archlinux.org/title/Ansible)
- [Ansible Beginners](https://www.youtube.com/watch?v=w9eCU4bGgjQ&t=0s)
- [Ansible for Homelab](https://www.youtube.com/watch?v=yoFTL0Zm3tw)
- [Techdufus dotfiles](https://github.com/techdufus/dotfiles)
- [Techdufus k9s setup via Ansible](https://github.com/TechDufus/dotfiles/blob/main/roles/k9s/handlers/main.yml)
- [DevEnv Dotfiles Manjaro i3 via Ansible](https://github.com/ALT-F4-LLC/dotfiles?tab=readme-ov-file#dotfiles)
- [dotfiles in yaml via ansible](https://github.com/logandonley/dotfiles/blob/main/dot_bootstrap/setup.yml)
