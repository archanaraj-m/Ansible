--------------------------------------------------------------------------------------------------------------------------------------
################################# ANSIBLE TASKS ########################################
---------------------------------------------------------------------------------------------------------------------------------------
Task-1, 30-05-2023
------------------
[referhere](https://directdevops.blog/2023/05/27/devops-classroomnotes-27-may-2023/) for ansible , yaml&json diference
 1. Ansible controlNode and Node configuration
        1. take 3 machines
            1. controlNode(ubuntu)
            2. node1 (ubuntu)
            3. node2 (re dhat/centos)
* First i can create 3 instances 2 are ubuntu 1 is centos
* connect that instances then execute the below commands in 2 nodes (ACN,node1)
```
sudo apt update
sudo vi /etc/ssh/sshd_config     #it is for edit the password atuntication 'no' to 'yes'
sudo systemctl restart sshd
sudo adduser devops              #devops is username
sudo visudo                      #devops ALL=(ALL:ALL) NOPASSWD:ALL
ctrl+x,ENTER
same process for all 2 nodes for centos another commands
```
![preview](../../ansibleimages/ans1.png)
![preview](../../ansibleimages/ans2.png)
![preview](../../ansibleimages/ans3.png)
![preview](../../ansibleimages/ans4.png)

In ACN
-------

* After above process in ACN execute below commands in only ACN

```
su devops
ssh-keygen
cd .ssh
ls
cd 
```
* connect ACN to node1 commands are
```
ssh-copy-id username@anothernode privateIP address       
#like ssh-copy-id devops@172.31.14.123
echo 172.31.14.123(anothernode private IPaddress) > inventory 
#like this echo 172.31.14.123 > inventory
cat inventory
```
* Ansible Installation commands are
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
ansible --version
```
![preview](../../ansibleimages/ans5.png)
* To check the ansible inventory`` ansible -i inventory -m ping all``
* see previews
![preview](../../ansibleimages/ans6.png)
* In node 1 we can create only user and passwd authntication yes and give sudoers permission
![preview](../../ansibleimages/ans7.png)


---------------------------------------------------------------------------------------------------------------------------------------

Task-2, 31-05-2023 and 01-06-2023
-------------------------------------------

1. Install apache,nginx and java11 manually and later with playbook.
* create 3 instances 2 are nodes for install apache and nginx this 2 are run on same port so we take 2 different nodes and in that only we do install java & 1 is ACN.
* write yaml files after that copy and paste it in ACN with names .yml(ex:apache.yml)

nginx:
------
* Install nginx on ubuntu manual commands are
```
sudo apt update
sudo apt install nginx
systemctl status nginx
```
* And copy the instance publicIP and paste it in new tab nginx page came.
* playbook for nginx
```yml
---
- name: install nginx on ubuntu
  hosts: all
  become: yes
  tasks:
  - name: install nginx
    ansible.builtin.apt:
      name: nginx
      update_cache: yes
      state: present
     
```
* And nodes private IP address paste in ``vi hosts``
* playbook syntax check with this command``ansible-playbook -i hosts --syntax-check nginx.yml``
* Run playbook with this command``ansible-playbook -i hosts nginx.yml``
![preview](../../ansibleimages/ans8.png)
* copy another node publicIp and paste it in new tab then nginx pag ecame
![preview](../../ansibleimages/ans9.png)

JAVA
----
* Install java on ubuntu manual commands are
```
sudo apt update
sudo apt install openjdk-11-jdk -y
java-- version
```
* playbook for java

```yml
---
- name: install java on ubuntu
  hosts: all
  become: yes
  tasks:
    - name: install java
      ansible.builtin.apt:
        name: openjdk-11-jdk
        update_cache: yes
        state: present                           
```
* First paste playbook``vi java.yml``&&``vi hosts``
* run the play book commands are same ``ansible-playbook -i hosts --syntax-check java.yml``&&``ansible-playbook -i hosts java.yml``
![preview](../../ansibleimages/ans10.png)
* check the java version in another node ``java--version``
![preview](../../ansibleimages/ans11.png)

apache:
------
* Install apache2 on ubuntu manual commands are
```
sudo apt update
sudo apt install apache2
sudo systemctl status apache2
```
* And copy the instance publicIP and paste it in new tab apache page came.
* playbook for java
```yml
---
- name: install apache on ubuntu
  hosts: all
  become: yes
  tasks:
    - name: install apache
      ansible.builtin.apt:
        name: 
        update_cache: yes
        state: present 
```

* First paste playbook``vi apache.yml``&&``vi hosts``
* run the play book commands are same ``ansible-playbook -i hosts --syntax-check apache.yml``&&``ansible-playbook -i hosts apache.yml``
 
2. now write a playbook for each node to get php info page (you can use handler) 

Handlers: running operations on change
---------
* Sometimes you want a task to run only when a change is made on a machine. For example, you may want to restart a  service if a task updates the configuration of that service, but not if the configuration is unchanged. Ansible uses handlers to address this use case. Handlers are tasks that only run when notified.
[referhere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html#handlers)for the handlers documentation
* Install php with apache2 on ubuntu manual commands are
```
sudo apt update
sudo apt install php libapache2-mod-php
sudo systemctl restart apache2
```
* Install php with nginx on ubuntu manual commands are
```
sudo apt update
sudo apt install php-fpm
systemctl status php-fpm
```
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
---------------------------------------------------------------------------------------------------------------------------------------
Task-3, 02-06-2023
-----------------------
 1. install metricbeat installation on ubuntu vm.(Manually)
 2. install metricbeat installation on ansible nodes by playbook.
----------------------------------------------------------------------------------------------------------------------------------------
Task-4, 03-06-2023
-----------------------
 1. install NOPCommerce Manually.
 2. write a playbook for NOPCommerce and make it install on nodes.
----------------------------------------------------------------------------------------------------------------------------------------
Task-5, 04-06-2023
-----------------------
 1. Install the below applications manually.
    1. Broadleaf
    2. OpenMRS
 2. After successful Installations of above applications write playbooks execute them.
---------------------------------------------------------------------------------------------------------------------------------------