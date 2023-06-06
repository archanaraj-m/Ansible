--------------------------------------------------------------------------------------------------------------------------------------
################################# ANSIBLE TASKS ########################################
---------------------------------------------------------------------------------------------------------------------------------------
Task-1, 30-05-2023
-----------------
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
 2. now write a playbook for each node to get php info page (you can use handler)
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