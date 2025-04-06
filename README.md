                                            Ansible Web Application Deployment Project
                                            
This project demonstrates how to deploy a simple web application using Ansible. The application uses Nginx as a web server and deploys a static webpage (index.html) on a set of remote servers. This project also includes the automation of installing and configuring Nginx on the target servers.

Table of Contents
Project Overview
Requirements
Installation
Usage
Playbooks
saber
Folder Structure
License
Project Overview
This project automates the deployment of a static web application on multiple servers using Ansible. The following steps are automated:

Installing and configuring Nginx on the remote servers.
Deploying a static index.html page to the servers.
The project is designed to be simple yet effective for learning Ansible playbooks and web server deployment.

Requirements
Before running this project, ensure you have the following:

Ansible installed on your control machine (local machine).
SSH access to the target servers (either through EC2, VirtualBox VMs, etc.).
Python installed on both your local and remote machines.
Target servers configured and reachable via SSH.
A valid private SSH key if required for authenticating to the remote machines.
Installation
Step 1: Install Ansible
If Ansible is not already installed, follow these steps:

On your Ubuntu control machine:
bash
Copier le code
sudo apt update
sudo apt install ansible
Step 2: Set up Inventory File
Ensure that your hosts inventory file is correctly configured with the IPs of your remote servers. You can modify the default /etc/ansible/hosts or create a new inventory file. Example:

ini
Copier le code
[web_servers]
192.168.1.10
192.168.1.11
Step 3: Copy Private Key
If you use a private key to connect via SSH, ensure it's available on the control machine and referenced in your playbook or inventory.

Usage
Step 1: Install Nginx on Remote Servers
Run the install_nginx.yaml playbook to install Nginx on the target servers:

bash
Copier le code
ansible-playbook install_nginx.yaml -i hosts
Step 2: Deploy Web Application
Run the deploy.yaml playbook to deploy the web application (the index.html file) to the remote servers:

bash
Copier le code
ansible-playbook deploy.yaml -i hosts
After running the playbook, you should be able to access the web application by visiting the IP addresses of the remote servers in a web browser.

Playbooks
install_nginx.yaml
This playbook installs Nginx on the target servers.

yaml
Copier le code
---
- name: Install Nginx on web servers
  hosts: all
  become: true
  tasks:
    - name: update apt cache
      apt:
        update_cache: yes

    - name: install nginx
      apt:
        name: nginx
        state: present
deploy.yaml
This playbook installs Nginx and deploys a simple index.html file to the web servers.

yaml
Copier le code
---
- name: Deploy Web App
  hosts: web_servers
  become: true
  tasks:
    - name: Include the nginx installation playbook
      import_playbook: install_nginx.yaml

    - name: Copy index.html to the web server's root directory
      copy:
        src: index.html
        dest: /var/www/html/index.html
Folder Structure
Here is the recommended folder structure for this project:

bash

Copier le code
/project-directory/
├── deploy.yaml           # Main playbook to deploy the web app
├── install_nginx.yaml    # Playbook to install Nginx
└── index.html            # Web application HTML file to deploy
License
This project is open-source and available under the MIT License. See the LICENSE file for more details.
