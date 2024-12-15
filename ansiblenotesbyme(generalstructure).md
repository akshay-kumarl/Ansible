# Ansible

Ansible is an automation tool designed for configuration management, application deployment, and task orchestration. 

## 1. Inventory

The inventory file lists the hosts (or groups of hosts) Ansible will manage. It can be a static file or dynamically generated.

Example:
```
# Static inventory file (hosts.ini)
[webservers]
web1.example.com
web2.example.com

[databases]
db1.example.com
db2.example.com
```

#### Dynamic Inventory:
Can be a script or plugin that fetches hosts from sources like AWS, Azure, or Kubernetes.


## 2. Playbooks
Playbooks are YAML files that define the tasks to execute on the hosts.

#### Structure:
Play: Maps tasks to a group of hosts. <br/>
Tasks: Define actions to perform (e.g., install packages, start services).<br/>
Modules: Built-in or custom functionality used to perform tasks.<br/>

#### Example Playbook:

```
- name: Configure web servers
  hosts: webservers
  become: yes  # Run tasks with elevated privileges
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    - name: Start Nginx service
      service:
        name: nginx
        state: started

```

## 3. Roles


Roles organize playbooks into reusable, modular components. They contain:

Tasks: A list of tasks to execute.<br/>
Handlers: Special tasks triggered by notifications (e.g., restarting a service).<br/>
Templates: Jinja2 templates for configuration files.<br/>
Files: Static files to copy to hosts.<br/>
Vars: Variables specific to the role.<br/>
Defaults: Default variables for the role.<br/>
Meta: Metadata about the role.
#### Role Directory Structure:
```
roles/
  myrole/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      config.j2
    files/
      somefile.txt
    vars/
      main.yml
    defaults/
      main.yml
    meta/
      main.yml

```



## 4. Variables

Variables allow you to customize tasks and are defined in:

Inventory files.
Playbooks.
Role files (vars/, defaults/).
Extra vars passed on the command line (--extra-vars).

#### Example:
```
# Playbook with variables
- name: Configure Nginx
  hosts: webservers
  vars:
    nginx_port: 8080
  tasks:
    - name: Configure Nginx port
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
```

## 5. Handlers 

Handlers are tasks triggered by other tasks when changes occur.

#### Example:
```
tasks:
  - name: Copy configuration file
    copy:
      src: nginx.conf
      dest: /etc/nginx/nginx.conf
    notify:
      - Restart Nginx

handlers:
  - name: Restart Nginx
    service:
      name: nginx
      state: restarted

```


## 6. Templates

Templates are configuration files written in Jinja2 and rendered with variables.

#### Example:
##### nginx.conf.j2:
```
server {
    listen {{ nginx_port }};
    server_name localhost;
}

```


## 7. Plugins
Plugins extend Ansible functionality and include:

Callback plugins: Customize Ansible output.<br/>
Connection plugins: Define how Ansible connects to hosts.<br/>
Lookup plugins: Fetch external data.
## 8. Modules
Modules are the building blocks for tasks. Ansible provides modules for tasks like:

File operations: `copy, file, template.`<br/>
Package management: `apt, yum.`<br/>
System operations: `service, user.`

## 9. Execution
Run playbooks with the `ansible-playbook` command:
```
ansible-playbook -i inventory.ini playbook.yml
```
