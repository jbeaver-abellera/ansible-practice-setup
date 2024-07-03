# ansible-practice-setup

This repository contains the playbook YAML file I created as practice on deploying an nginx web serer using Ansible. See below for steps on how I setup an EC2 instances for the controller and managed host, as well as how to setup and execute the playbook in the controller host.

## Setting up EC2/VM Instances
For this project, EC2 should be configured to have (1) Public IP enabled so the instances can access the internet and install files such as nginx and ansible, (2) configure security groups, and (3) make sure to create a key pair for the controller and another for the managed hosts. 
<p align="center">
<img src="https://github.com/jbeaver-abellera/ansible-practice-setup/assets/108796284/6c80c557-63c1-4801-a9f8-fe6a6ea8fed5" height=400> <img src="https://github.com/jbeaver-abellera/ansible-practice-setup/assets/108796284/d0d9eea0-11d2-41f9-9f0e-c09e72e49aa6" height=400>
</p>

For the security group, please see below on neccessary rules for each hosts.
`Controller Host`: Its Inbound rules should allow SSH connection (port 22) from any IP address so our local machine can access and configure the controller
`Managed Host`: It should have inbound rules allowing SSH connection only from the controller hosts' security group. It should also allow HTTP/S connection from any IP address, so it can serve our website to the public.

## Setting up Ansible on controller host
1. Copy the private key of the managed hosts. This will be used by Ansible to SSH into the managed hosts.
2.  Install ansible on the controller host. Simply type in the command below.
``` bash
$ sudo yum update
$ sudo yum install software-properties-common
$ sudo add-yum-repository --yes --update ppa:ansible/ansible
$ sudo yum install ansible
```
2. Upon installation, a new directory will be created called `/etc/ansible`. And in here we can add the playbooks file or configure the hosts file.
3. Configure hosts file to group target managed hosts as well as indicate various variables such as username, and path of the private key to ssh into the hosts. See template host file below:
``` bash
[group1]
<host1-ip-addres>
<host1-ip-addres>

[group1:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=/path/to/managed-host/private-key.pem
```
4. Now that we configured our managed hosts, we can now create a playbook file to automate building an nginx server and deploy my portfolio website. Copy the setup-playbook.yml file in this repository in the controller node.
> Please also note that this playbook expects that you copy your website files and asset in the controller node. There are various ways to install website assets in the controller but you can try using the 'scp' command in your local terminal. This allows for scure copying of files through SSH. 
6. run the **ansible-playbook** command in the controller shell to execute the playbook.
``` bash
sudo ansible-playbook <playbook-filename.yml>
```
6. To check if all tasks and plays have been successful, the bash shell will display an output which can indicate if it has failed, succeeded or if nothing has changed if the state has been premodified.
7. You can also confirm if we have successfully started an nginx server by copying the public ips of the managed host and entering it in your local browser. It should have loaded your website


## Check out my website!
I am yet to find and setup a public and human readable link for this website but you can check out these public IPs of my EC2 instances that hosts my website. Let me know what you think!
13.212.188.238
54.179.254.204


