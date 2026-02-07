## Ansible Playbook — Install and Start Apache 2

This playbook is used to **install Apache2**, **deploy a custom index.html page**, and **start the Apache service** automatically on target hosts using Ansible.


```
# Create directory
mkdir playbook-basic/
# Enetering directory 
# Create ansible configuration file
vim ansible.cfg
[defaults]
inventory = ./inventory
remote_user =student

# Create file inventory to defint managed host
[web]
server-managed1

# Create subdirectory files, for web content
echo "This is a test page." > files/index.html
```

### 📜 site.yml Content

This playbook performs the following tasks:
1. Installs Apache2
2. Copies a custom `index.html` file to the web root directory
3. Starts and enables the Apache service

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
# Following tis command to running playbook
ansible-playbook site.yml
