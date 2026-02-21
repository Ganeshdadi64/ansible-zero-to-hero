# Ansible Concepts: Playbook, Play, Modules, Tasks, and Collections

## Playbook
A **Playbook** is a YAML file that defines a series of actions to be executed on managed nodes. It contains one or more "plays" that map groups of hosts to roles.

### Example
```
---
- name: Update web servers
  hosts: webservers
  remote_user: root

  tasks:
  - name: Ensure apache is at the latest version
    ansible.builtin.yum:
      name: httpd
      state: latest

  - name: Write the apache config file
    ansible.builtin.template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf

- name: Update db servers
  hosts: databases
  remote_user: root

  tasks:
  - name: Ensure postgresql is at the latest version
    ansible.builtin.yum:
      name: postgresql
      state: latest

  - name: Ensure that postgresql is started
    ansible.builtin.service:
      name: postgresql
      state: started
```

```
APT AND SERVICE MODULE EXPLANATION (ANSIBLE)

=====================================
APT MODULE
=====================================

apt is an Ansible module used to manage packages on Debian-based systems 
like Ubuntu.

It works with the APT package manager (Advanced Package Tool).

Example:
- name: Install Nginx
  apt:
    name: nginx
    state: present

Explanation:

name: nginx
→ This specifies the package name you want to manage.

state: present
→ Ensures the package is installed.
→ If nginx is not installed, it will install it.
→ If already installed, it does nothing.

Important states in apt:
- present → Install package if not installed.
- absent → Remove package.
- latest → Install or update to latest version.

Why apt module is better than using command:
Instead of writing:
  sudo apt install nginx -y

Ansible checks first:
- If nginx already exists → No change.
- If not → Install it.

This makes it idempotent (safe to run multiple times).


=====================================
SERVICE MODULE
=====================================

service is an Ansible module used to manage services 
(start, stop, restart, enable).

Example:
- name: Start Nginx service
  service:
    name: nginx
    state: started

Explanation:

name: nginx
→ The service name you want to control.

state: started
→ Ensures the service is running.
→ If nginx is stopped, it will start it.
→ If already running, it does nothing.

Common states in service module:
- started → Ensure service is running.
- stopped → Ensure service is stopped.
- restarted → Restart service.
- reloaded → Reload configuration.

You can also enable service at boot:
  enabled: yes

Example:
  service:
    name: nginx
    state: started
    enabled: yes


=====================================
SIMPLE UNDERSTANDING
=====================================

apt module → Manages software packages.
service module → Manages running services.

apt installs nginx.
service starts nginx.

Both modules are idempotent:
You can run the playbook multiple times safely.

```









## Play

A Play is a single, complete execution unit within a playbook. It specifies which hosts to target and what tasks to execute on those hosts. Plays are used to group related tasks and execute them in a specific order.

```
- name: Install and configure Nginx
  hosts: webservers
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```

## Modules

Modules are the building blocks of Ansible tasks. They are small programs that perform a specific action on a managed node, such as installing a package, copying a file, or managing services.
Example

The apt module used in a task to install a package:

```
- name: Install Nginx
  apt:
    name: nginx
    state: present
```

## Tasks

Tasks are individual actions within a play that use modules to perform operations on managed nodes. Each task is executed in order and can include conditionals, loops, and handlers.
      
```
- name: Install Nginx
  apt:
    name: nginx
    state: present

- name: Start Nginx service
  service:
    name: nginx
    state: started
```

```
ANSIBLE COLLECTIONS – DEEP AND EASY EXPLANATION

====================================================
WHAT IS A COLLECTION?
====================================================

A Collection is a packaging format in Ansible.

It is used to bundle and distribute:
- Roles
- Modules
- Plugins
- Playbooks
- Documentation
- Other Ansible content

Think of Collection like:

Java → Maven dependency (jar file)
Node.js → npm package
Python → pip package
Ansible → Collection

Instead of sharing roles and modules separately,
we bundle everything into one structured package.

====================================================
WHY COLLECTIONS ARE NEEDED?
====================================================

Before Collections:
- Roles and modules were shared separately
- Hard to manage versions
- No proper namespace
- Conflicts between modules

After Collections:
- Everything grouped together
- Version controlled
- Proper naming structure
- Easy to install and reuse

====================================================
COLLECTION STRUCTURE EXAMPLE
====================================================

my_collection/
├── roles/
│   └── my_role/
│       └── tasks/
│           └── main.yml
├── plugins/
│   └── modules/
│       └── my_module.py
└── README.md

Explanation:

roles/
→ Contains reusable automation logic (like mini projects).

plugins/modules/
→ Custom Python modules written for Ansible.

README.md
→ Documentation of the collection.

====================================================
REAL-WORLD EXAMPLE
====================================================

Suppose you are working in a company.

You created:
- A role to install Docker
- A custom module to check disk usage
- A plugin to validate config

Instead of sharing them separately,
you bundle everything into:

company.devops collection

Now anyone in the company can install and use it.

====================================================
HOW TO USE A COLLECTION
====================================================

Example Playbook:

- name: Use a custom module from a collection
  community.general.my_module:
    option: value

Explanation:

community.general
→ Collection name (namespace.collection_name)

my_module
→ Module inside that collection

option: value
→ Module parameters

Full Format:
namespace.collection.module

Example:
amazon.aws.ec2_instance

Here:
amazon → Namespace
aws → Collection
ec2_instance → Module

====================================================
HOW TO INSTALL A COLLECTION
====================================================

Install from Ansible Galaxy:

ansible-galaxy collection install community.general

After installation, you can use modules from it.

====================================================
WHY THIS IS IMPORTANT IN REAL DEVOPS
====================================================

In real companies:

- Teams build reusable automation
- Multiple projects use same roles
- Cloud providers maintain official collections

Examples:
amazon.aws → AWS automation
community.docker → Docker automation
kubernetes.core → Kubernetes automation

Instead of writing everything manually,
you reuse tested modules from collections.

====================================================
SIMPLE ANALOGY
====================================================

Role = Single Tool
Module = Single Function
Collection = Toolbox containing many tools

====================================================
ONE LINE SUMMARY
====================================================

Ansible Collection is a structured package that bundles roles, modules, and plugins together so they can be versioned, shared, and reused easily.


```
