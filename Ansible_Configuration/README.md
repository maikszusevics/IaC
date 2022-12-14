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

- `web` is the name of the group/machine we wish to run this command on
- `-m` stand for module, in the above command we use the built-in ansible copy modudle.
- `-a` stands for argument, after this you put the argument.

another example:

`sudo ansible web -m shell -a "systemctl status nginx"`

In this example we are using the `shell` module which allows us to run normal linux commands as we would type them in the shell of the machine.

So after the `-a` our argument is the shell command we wish to run.


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
- To make the rev proxy playbook I made a nginx config file in my controller machine which has the details for reverse proxy, then used the built-in copy module in ansible to overwrite the default `default` file in `/etc/nginx/sites-available/`

![image](https://user-images.githubusercontent.com/110176257/188588007-66d6e88b-ff12-4e93-81ef-a4006ce3ee84.png)

### Playbook to install nodejs

- Here I use the `shell` module again to run the curl command in order to download the specific version of NodeJs that I want

![image](https://user-images.githubusercontent.com/110176257/188588434-df603570-aa95-42f0-bba1-3620444d78ce.png)

### Playbook to copy app files

![image](https://user-images.githubusercontent.com/110176257/188588654-57a5934c-0467-460d-be01-175fb5196368.png)

#### Running clone playbook:

![image](https://user-images.githubusercontent.com/110176257/188589114-81bb4cc5-7203-4a21-8c2c-31dc7959794b.png)


# Ansible on AWS

### Moving pem key to AWS

Open pem key in notepad and copy content to `~/.ssh` folder.

Generate private and public ssh key in .ssh folder using `ssh-keygen -t rsa -b 4096`

## Ansible vault

dependencies:
- Python 3.7
- pip3
- awscli

After installing python 3, run command `alias python=python3` to set new python version

to install pip3 run `sudo apt-get install python3-pip`

to get awscli run `pip3 install awscli `

ansivle vault folder structure:

`/etc/ansible/group_vars/all/file.yml`

encrypt AWS keys

`sudo ansible-palybook playbook.yml --ask-vault-pass --tags ec2_create`

automate ssh key access
copy eng122.pem and generate anothe keypair called eng122

Create directory `/etc/ansible/group_vars/all`

in the playbook copy the .pub file to ec2

`sudo ansible-vault create pass.yml` 
This open vim editor.

Press `i` to go to insert mode so you can type

Type `aws_access_key: XXXXXXXX` `aws_secret_key: XXXXXXXX`

`esc` then `:` then `wq!` and `enter` to save

install python 3, pip, and awscli dependendies using `apt` and `pip3`



## Creating an amazon ec2 instace using ansible controller

Explanation and code block template [here](https://medium.datadriveninvestor.com/devops-using-ansible-to-provision-aws-ec2-instances-3d70a1cb155f)

Ansible playbook completed to create ec2 instance:

```yml
# AWS playbook
---

- hosts: localhost
  connection: local
  gather_facts: True
  become: True

  vars:
    key_name: eng122
    region: eu-west-1
    image: ami-02bf7ff7ff0bd6e98
    id: "web-app"
    sec_group: "sg-0f07abe9db40cbde8"
    subnet_id: "subnet-0429d69d55dfad9d2"
    ansible_python_interpreter: /usr/bin/python3

  tasks:

    - name: Facts
      block:

      - name: Get instances facts
        ec2_instance_facts:
          aws_access_key: "{{aws_access_key}}"
          aws_secret_key: "{{aws_secret_key}}"
          region: "{{ region }}"
        register: result


    - name: Provisioning EC2 instances
      block:

      - name: Upload public key to AWS
        ec2_key:
          name: "{{ key_name }}"
          key_material: "{{ lookup('file', '~/.ssh/{{ key_name }}.pub') }}"
          region: "{{ region }}"
          aws_access_key: "{{aws_access_key}}"
          aws_secret_key: "{{aws_secret_key}}"

      - name: Provision instance(s)
        ec2:
          aws_access_key: "{{aws_access_key}}"
          aws_secret_key: "{{aws_secret_key}}"
          key_name: "{{ key_name }}"
          id: "{{ id }}"
          vpc_subnet_id: "{{ subnet_id }}"
          group_id: "{{ sec_group }}"
          image: "{{ image }}"
          instance_type: t2.micro
          region: "{{ region }}"
          wait: true
          count: 1
          instance_tags:
            Name: eng122_maiks_ansible

      tags: ['never', 'create_ec2']
```



After creating ec2 instances from the controller, add their IPs to the hosts file in `/etc/ansible/hosts`

```yml
[app]
ec2-instance ansible_host=172.31.29.219 ansible_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/.ssh/eng122_maiks_key

[db]
ec2-instance ansible_host=172.31.31.46 ansible_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/.ssh/eng122_maiks_key
```
