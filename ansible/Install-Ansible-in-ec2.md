# Install Ansible in EC2 (LINUX)

## Update system

1. `sudo apt-get update`

## Add ansible repository

2. `sudo apt-add-repository -y ppa:ansible/ansible`

## Update again

3. `sudo apt-get update`

## Install ansible

4. `sudo apt-get install -y ansible`

## Check installed ansible version

5. `ansible --version`

# Connfigure Ansible

## on master machine generate ssh key

**Note that if you have already generated before, skip it**

1. `ssh-keygen`
2. `sudo cat ~/.ssh/id_rsa.pub`
   **Copy the output below**

## Paste the ssh key to your target node

`sudo vim /home/ubuntu/.ssh/authorized_keys`
**please do not delete any existing values in this file, press enter then paste it below**

## Add inventory

`sudo vim /etc/ansible/hosts`

### Add the following below

**Note that you must replace xxx with your public IP address of target node**
[Host_Group]  
xxx.xxx.xxx.xxx ansible_ssh_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_python_interpreter=/usr/bin/python3

## make playbooks folder

1. `mkdir playbooks`
2. `cd ~/playbooks`

## create your playbook

`sudo vim installJava11.yml`

```
---
- hosts: Host_Group
  tasks:
    - name: Task - 1 Update APT package manager repositories cache
      become: true
      apt:
        update_cache: yes
    - name: Task -2 Install Java using Ansible
      become: yes
      apt:
        name: "{{ packages }}"
        state: present
      vars:
        packages:
           - openjdk-11-jdk
```

## Execute ansible playbook

`sudo ansible-playbook installJava11.yml`

## Finaly you can go to target node server to chech

`java -version`
