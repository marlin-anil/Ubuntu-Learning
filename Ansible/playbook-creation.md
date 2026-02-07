## Ansible Playbook — Install and Start Apache 2

This playbook is used to **install Apache2**, **deploy a custom index.html page**, and **start the Apache service** automatically on target hosts using Ansible.


### Create Structure under directory playbook-basic/ with file configuration, inventory, and file for web content 
```
# Create directory
mkdir playbook-basic/

# Enetering directory
cd playbook-basic/

# Create ansible configuration file
vim ansible.cfg
[defaults]
inventory = ./inventory
remote_user =student

# Create file inventory to defint managed host
[web]
server-managed1

[webserver]
server-managed2

# Create subdirectory files, for web content
echo "This is a test page." > files/index.html
```

### 📜 Playbook Creation

This playbook performs the following tasks:
1. Installs Apache2
2. Copies a custom `index.html` file to the web root directory
3. Starts and enables the Apache service

```
# This is simple playbook, with name site.yaml, following this command to reate file playbook
vi site.yml
```
```yaml
---
- name: Install and start Apache 2
  hosts: web
  become: true
  tasks:
    - name: apache2 package is present
      apt:
        name: apache2
        state: latest
        update_cache: yes
        force_apt_get: yes

    - name: correct index.html is present
      copy:
        src: ./files/index.html
        dest: /var/www/html/index.html

    - name: Apache 2 is started
      service:
        name: apache2
        state: started
        enabled: true
```

```
# This is other creation playbook with using variable, following this command to create file playbook
vi playbook.yml
---
- name: Install and Ensure the Apache2 service started
  hosts: webserver
  become: true
  vars:
    web_pkg: apache2
    web_service: apache2
    python_pkg: python3-urllib3

  tasks:
    - name: Required packages are installed and up to date
      apt:
        update_cache: yes
        force_apt_get: yes
        name:
          - "{{web_pkg}}"
          - "{{python_pkg}}"
        state: latest

    - name: The {{web_service}} service is started and enabled
      service:
         name: "{{web_service}}"
         enabled: true
         state: started

    - name: Web content is in place
      copy:
        content: "Hello World! ansible is fun."
        dest: /var/www/html/index.html

- name: Verify the web server is accessible
  hosts: localhost
  tasks:
    - name: Testing web server
      uri:
        url: http://pod-username-managed2
        status_code: 200
        return_content: yes
      register: Result

    - name: Print Ouput web server
      debug:
        var: Result.content
```

```
# This is other creation playbook with using jinja-jina template, following this command to create file playbook
vi playbook-jinja.yml
---
- name: install and start apache2
  hosts: webservers
  become: true

  tasks:
    - name: ensure apache2 package is present
      apt:
        name: apache2
        state: present
        update_cache: yes
        force_apt_get: yes

    - name: restart apache2 service
      service:
        name: apache2
        state: restarted
        enabled: yes

# Replace lina with ur real name/case
    - name: copy index.html
      template:
        src: lina.html.j2
        dest: /var/www/html/lina.html

```
```
#  Create Jinja 2 template. Replace lina with ur name/case
vim vim lina.html.j2
Hello World!
This is lina site.

```

```
# Check the syntax of playbook creation
ansible-playbook --syntax-check site.yml
ansible-playbook --syntax-check playbook.yml
ansible-playbook --syntax-check playbook-jinja.yml

# Following tis command to running playbook
ansible-playbook site.yml
ansible-playbook playbook.yml
ansible-playbook playbook-jinja.yml
