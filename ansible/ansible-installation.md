# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

1. change hostname to ansible-server
   vi /etc/hostname (type ansible-server)
2. Reboot server

<!-- 3. Install python and python-pip
   ```sh
   yum install python
   yum install python-pip
   ```
4. Install ansible using pip check for version
    ```sh
    pip install ansible
   ansible --version
   ``` -->
   
5. Create a user called ansadmin (on Control node and Managed host)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
6. Below command grant sudo access to ansadmin user. But we strongly recommended using "visudo" command if you are aware vi or nano editor.  (on Control node and Managed host)
   ```sh
   echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
   ```
   
Enable password based authentication
1. `vi /etc/ssh/sshd_config`
```
 PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication no
```
2. `service sshd reload`

7. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
8. Copy keys onto all ansible managed hosts (on Control node)
   ```sh 
   ssh-copy-id ansadmin@<target-server>
//
   ssh-copy-id username-created-to-target-server@<target-server>
   ```

9.  Ansible server used to create images and store on docker registry. Hence install docker, start docker services and add ansadmin to the docker group. 
   ```sh
   yum install docker
   
   # start docker services 
   service docker start
   service docker start 
   
   # add user to docker group 
   usermod -aG docker ansadmin

   ```
11. Create a directory /etc/ansible and create an inventory file called "hosts" add control node and managed hosts IP addresses to it. 
 
### Validation test

   
1. Run ansible command as ansadmin user it should be successful (Master)
   ```sh 
   ansible all -m ping
   ```
