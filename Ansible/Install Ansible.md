#### Install Required Packages Before Installing Ansible

```bash
# Update the list of available software packages from Ubuntu repositories
sudo apt update

# Install required package to enable adding external repositories (PPA)
sudo apt install -y software-properties-common

# Add the official Ansible PPA repository and update package list
sudo add-apt-repository --yes --update ppa:ansible/ansible
```

#### Install Ansible, create configuration directory and file for manage host

```bash
# Follow the command to install ansible
sudo apt install ansible -y

# Chcek the vrsion of ansible
ansible --version 

# Create the parent directory for ansible configuration 
mkdir -p /etc/ansible

# Create file in di rectory /etc/ansible to define all host for manage, adding hostname/domain/ip address to define server for manage
vim /etc/ansible/hosts

pod-marlin-managed1

[webserver] #for groupping
pod-marlin-managed1

```

#### Create connection between server controller and server managed

```
# Use ssh-key for connect or access with passwordless, this create pair key private and public key 
ssh-keygen

# Share public key to server for manage
ssh-copy-id server@managed1
ssh-copy-id server@managed2

# Chek the connection with module ping, this use inventory from file /etc/ansible/hosts
ansible all -m ping



