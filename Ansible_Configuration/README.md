# Ansible

## What is ansible
Ansible is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality.

## Why ansible
Benefits of ansible include:
- Simple to set up and use 
- Powerful, lets you model complex workflows
- Flexible
- Agentless, this means you do not have to install any additional software on the machines managed by ansible. This removes a potential single point of failure which helps with high availability 
- Enables hybrid cloud integration
- With the use of an ansible controller you can control all nodes without SSHing into them

## Ansible dependencies 
- `sudo apt-get install software-properties-common`
- python 2.7 or above 
- Ansible repo link (`sudo apt-add-repository ppa:ansible/ansible`)


- install ansible (`sudo apt-get install ansible -y`)


- update & upgrade
**ONLY FOR CONTROLLER BECAUSE ANSIBLE IS AGENTLESS**


#### Ansible ad-hoc commands
ad-hoc commands allow us to run commands and get the response without SSHing into the machine

for example you can use this command to copy a file from controller to an agent:
`ansible web -m ansible.builtin.copy -a "src=/etc/ansible/test.txt dest=/home/vagrant/copyfold"`

## Ansible playbooks:

Run playbook with: `ansible-playbook nginx-play.yml`