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

### Playbook which installs nginx on agent machines:

- Create a .YML file
- 
![image](https://user-images.githubusercontent.com/110176257/188586929-ed3f17b4-4a73-40d1-9367-f30f1a1a423d.png)

- Three dashes `---` starts the file

- `-vvv` gives a lot more log info

- adding `--syntax-check` on the end of the `ansible-playbook` command performs syntax check

### Playbook to do reverse proxy
```
# add hosts or name of host (web)
- hosts: web
# gather information live
# indentation is very big
  gather_facts: yes
# we need admin perms
  become: true
# add the instructions
  tasks:
  - name: copy rev prox file
    copy:
      src: /etc/ansible/revprox
      dest: /etc/nginx/sites-available/default
      
```
![image](https://user-images.githubusercontent.com/110176257/188588007-66d6e88b-ff12-4e93-81ef-a4006ce3ee84.png)

### Playbook to install nodejs

![image](https://user-images.githubusercontent.com/110176257/188588434-df603570-aa95-42f0-bba1-3620444d78ce.png)

### Playbook to copy app files

![image](https://user-images.githubusercontent.com/110176257/188588654-57a5934c-0467-460d-be01-175fb5196368.png)

#### Running clone playbook:

![image](https://user-images.githubusercontent.com/110176257/188589114-81bb4cc5-7203-4a21-8c2c-31dc7959794b.png)
