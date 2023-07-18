# DevOps Classroomnotes Ansible(23/May/2023 to june14/2023)
* Example Application: Story of ToyCraft
* ToyCraft is an enterprise which is global and famous for its cheap toys
* This application is accessed globally.
* The architecture of the application is as follows
![Preview](../ansibleimages/ans27.png)
* Every 15 days toycraft gives a new release and it has to update in all the servers.
* Toycraft has minimum one deployment per country.
* Toycraft is available in 15 countries.
* To do this i have 3 possible options
# deployment options
![preview](../ansibleimages/ans23.png)
* Declarative vs Procedural
* ![preview](../ansibleimages/ans24.png)
* Configuration Management is all about declarative deployment of applications which ensures
   * idempotence: Run this once or n times you will have same result
   * Desired state: We express configuration to acheive a desired state.
   * reusable
# Configuration Management (CM)
Configuration Management is all about declarative deployment of applications which ensures
   * idempotence: Run this once or n times you will have same result
   * Desired state: We express configuration to acheive a desired state.
* Direction of Communication
   * PULL => Node to CM server
   * Push => CM Server to Node
* Ansible is a configuration management platform that automates storage, servers, and networking. When you use Ansible to configure these components, difficult manual tasks become repeatable and less vulnerable to error.
* There are two types of CM
Pull based CM:
--------------
* In this type of configuration management tool, the nodes pull the configuration information from the server (hence, the name).A small software (called agent or client) is installed on every node.
* This agent/client will:at regular intervals, get the configuration from the server compare the configuration received from the server with the current configuration of the node, if there is any mis-match, take the steps required to match the configuration of the node with the configuration received from the server.
  * Chef & 
  * Puppet are good examples for pull based configuration management tools.
![preview](../ansibleimages/ans.png)
Push Based CM:
-------------- 
* In this type of configuration management tool, the main server (where the configuration data is stored) pushes the configuration to the node (hence, the name). So, it is the main server that initiates communication, not the nodes. Which means that an agent/client may or may not be installed on each node.
  * Ansible & 
  * SaltStack are good examples for push based configuration management tools.
* What is required in PULL Based CM?
  * Agent needs to be installed with necessary credentials to connect to CM Server
* What is required in Push Based CM
  * List of nodes (inventory)
  * Credentials to login into node
Architecture of Ansible
------------------------
* Ansible works by connecting to your nodes and pushing out scripts called “Ansible modules” to them. Most modules accept parameters that describe the desired state of the system. Ansible then executes these modules (over SSH by default), and removes them when finished. Your library of modules can reside on any machine, and there are no servers, daemons, or databases required.
![preview](../ansibleimages/ans0.png)
* Ansible control node can execute desired state on nodes using
    * adhoc commands
    * playbooks
* Playbooks are YAML files.

# How Operations Team work on multiple servers
* Organizations will have lot of servers and lot of admins
* Creating individual logins on each server for every admin is not a feasible solution.
* An effective way is organization creates a service account for the admins to login and   perform administration.
![preview](../ansibleimages/ans00.png)
* For the lab activities our service account’s name would be devops
* Having username and password is not a sensible option then how to solve this problem

# How to setup key pair based authentication in linux machines
* Key pair is combination of two keys public and private using alogrithms, we will be using RSA
* Create a key pair ``ssh-keygen``
* after that Copy the public key to linux machine <ssh-copy-id username@ipaddress> example``ssh-copy-id devops@172.168.123.11``
* connect to the machine using private key ``ssh -i <path-to-private key> username@ipaddress``
* Generally private keys created will have extension of .pem
* i.e we create a Service account public and private key. Copy the service account public key to all the servers. disable password based authentication
# Setting up sudo permissions
* We need to add devops user to the sudoers group (Wheel)
* Execute ``sudo visudo``in that insert`` devops ALL=(ALL:ALL) NOPASSWD:ALL``
* Environment
* We need atleast two linux machines
    * one is Ansible control node
    * others is/are nodes
* We will be creating a service account called as devops in all machines
* We will be creating a key pair in Ansible control node
* Copy the public key into the nodes
* Optional: Disable password based authentication

# Installing and Configuring Ansible
We will create two ubuntu vms
Create a user called devops in two vms with sudo permissions
Create a key-pair in ansible control node & copy the public key to other vm from ansible control node
* Installing ansible
```
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```
* Verify ansible version ``ansible --version``
* Now lets add inventory. Create a file called as ``vi hosts`` with one entry <ipaddress>
* Check connectivity by executing ``ansible -i hosts -m ping all``
# Note:
* To enable password authentications edit config ``sudo vi /etc/ssh/sshd_config`` and set PasswordAuthentication to yes from no.
* restart sshd service ``sudo service sshd restart``

# Ansible
Ansible can communicate with nodes by using two approaches
1. adhoc commands:
* We build a command for desired state
* It's not reuseable,and it is used for quick teast and changes
* it is used for a single task execute quickly,it's running without write a playbook.

# playbook:
* We create a file where we express desired state
* This is recommended approach for repetitive activites

* We have taken a sample playbook
```yml
---
- name: hello ansible
  hosts: all
  become: yes
  tasks:
    - name: update packages and install tree
      apt:
        name: tree
        state: present
        update_cache: yes
```
* Basic Playbook semantics
![preview](../ansibleimages/ans22.png)
* In ansible the smallest unit of work is perfomed by "module"

# YAML
* YAML vs JSON
![preview](../ansibleimages/ans25.png)
Refer Here for yaml syntax from ansible
# Ways of Working (WoW):
* list down all the manual steps
* Ensure all the steps are working
* For each step find a module and express the desired state
# Activity 1: Install apache server
* Manual steps
```
sudo apt update
sudo apt install apache2 -y
```
* Verify the installation in new tab ``http://<public-ip>``
* [ReferHere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html) for ansible yaml syntax for playbook
* Finding the module:
    * search google with <command> in ansible, example command name copy or search "wget module in ansible"
    * search from ansible modules docs [ReferHere](https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html)
* all the commands for installation are executed with sudo so we can give ``become: yes`` in playbook.
* create inventory``vi hosts``
* Run the command ansible-playbook -i <inventory-path> <playbook-path>
* If we create 2directories with names inventory and playbook we need to give path otherwise we can run playbook directly.
![preview](../ansibleimages/ans26.png)

# Activity 2: Installing lamp server on ubuntu
* Note: we will skip mysql installation for now
* Manual steps are
```
sudo apt update
sudo apt install apache2 -y
sudo apt install php libapache2-mod-php php-mysql -y
# Create a file called as /var/www/html/info.php with below content
# <?php phpinfo(); ?>
sudo -i
echo '<?php phpinfo(); ?>' > /var/www/html/info.php
exit
sudo systemctl restart apache2
```
* Refer in my ansible yamlfiles folder for the changeset containing playbook(php.yaml), inventory and file
* Check for syntax of playbook ``ansible-playbook -i <inventory-path> --syntax-check <playbook-path>``
* Now lets run the playbook ``ansible-playbook -i <inventory-path> <playbook-path>``
* Verify the installation with ``<IPaddress>/info.php``
* Problems to be addressed
    * during every playbook execution the apache service is getting restarted

# Activity 3: Install lamp stack on Redhat 9
* Manual steps
```
sudo yum install httpd -y
sudo systemctl enable httpd
sudo systemctl start httpd
sudo yum install php -y
sudo -i
echo '<?php phpinfo(); ?>' > /var/www/html/info.php
exit
sudo systemctl restart httpd
```
* Refer in my ansible yamlfiles folder for the changeset containing playbook(redhat.yaml)
* Problems
    * during every playbook execution the apache service is getting restarted
    * Why should we have two inventory files one for ubuntu and one for redhat
    * Can we have one playbook for both redhat and ubuntu
    * Bailout with proper failure message for unsupported operating systems

# Ansible Handlers
* Handlers: Handlers are tasks that only run when notified.
* For the documentation [ReferHere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html#handlers)
* Refer in my yamlfiles handlers folder for the changes with handlers included
* Info.php copying step changed i.e. copied the file to meet desired state so restart executed
* Info.pyhp copying step was ok i.e. desired state is met so task did nothing, so restart not executed
* Inventory in Ansible represents the hosts which we need to connect to.
* Ansible inventory is broadly classified into two types
   * static inventory: where we mention the list of nodes to connect to in some file
   * dynamic inventory: where we mention some script/plugin which will dynamically find    out the nodes to connect to
* As of now lets focus on static inventory
* [Refer Here](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#inventory-basics-formats-hosts-and-groups) for official docs on Ansible inventory
* Static inventory can be mentioned in two formats
      ini
      yaml
* Ini format: In this format we can use commands with -i means inventory use that inventory file we can mentioned the details of web servers and app servers in that.
* command is ``ansible-playbook -i <inventorypath> <playbook path>``
* To check the list of hosts ``ansible-playbook -i <inventorypath> <playbook path> --list-hosts``
* example ``ansible-playbook -i inventory/hosts yamlfiles/handlers/ubuntu.yaml``
* Refer for the changes to include groups and same inventory across redhat and ubuntu instances in my yamlfiles =>handlers=>hosts

* YAML format:
* for the inventory in yaml format
```
---
all:
  children:
    ubuntu:
      hosts:
        172.31.27.136:
    redhat:
      hosts:
        172.31.23.22:
    webserver:
      hosts:
        172.31.27.136:
        172.31.23.22:
```
# Facts:
* If we want to execute or skip a task based on facts. Facts are attributes of individual hosts, including IP address, operating system, the status of a filesystem, and many more. With conditionals based on facts.
* ansible collects information about the node on which it is executing by the help of module called as setup
* Ansible playbook by default collects information about nodes where it is executing, we can use this with the help of variables
* Collecting information can be disabled as well
```yaml
---
- name: do something
  hosts: all
  gather_facts: no
  ...
  ...
```
* In the playbook the facts will be collected and will be available in a special variables ``ansible_facts``
* Consider the below playbook
```yml
---
- name: exploring facts
  become: no
  hosts: all
  tasks:
    - name: print os details
      ansible.builtin.debug:
        msg: "family: {{ ansible_facts['os_family'] }} distribution: {{ ansible_facts['distribution'] }}"
```
* The statement ``ansible_facts['os_family']`` represents accessing os family from the facts collected
* From facts the variables can be accessed with full names ``ansible_default_ipv4`` or ``ansible_facts['default_ipv4']``
```yml
---
- name: exploring facts
  become: no
  hosts: all
  tasks:
    - name: print os details
      ansible.builtin.debug:
        var: ansible_default_ipv4
    - name: same info
      ansible.builtin.debug:
        var: ansible_facts['default_ipv4']
```        
* Lets apply conditionals to ansible playbook [ReferHere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html)
* Refer for the changeset in my ansible=>yamlfiles=>handlers=>combined.yaml
* Explore
* Explore the verbosity levels of execution i.e -v, -vv , -vvv ..
* Write an ansible adhoc command to install git on node1
```
sudo apt install git
sudo yum install git
```
# Ansible Variables
* Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line. You can also create variables during a playbook run by registering the return value or values of a task as a new variable.
* [ReferHere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html) for the official docs on variables
* Refer in my ansible=>yamlfiles=>handlers=>hosts changeset containing variables
* Lets use a generic package manager package [ReferHere](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html)
* Refer for the changeset where we used package instead of apt/yum for install apache in my ansible=>yamlfiles=>handlers=>combined_red_ubuntu.yaml
* variables at the inventory level need not be in inventory file means we are create variables in inventory file but specielly no need to inventory file for variables.
* Refer for the debug messages in my ansible=>yamlfiles=>handlers=>combined_red_ubuntu.yaml
* [Refer Here](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html) for loops in ansible
* Refer the changeset with loops and using loops from variables in my ansible=>yamlfiles=>handlers=>combined_red_ubuntu.yaml

# tomcat installation
* Lets try to install tomcat without using package manager
* [Refer Here](https://linuxize.com/post/how-to-install-tomcat-10-on-ubuntu-22-04/) for manual steps for installing tomcat 10 on ubuntu 22.04
* In tomcat installation in the useradd command -d stands for home directory
* group variables are added in group vars folder for example tomcat is my group name so in the ``host : tomcat`` and group vars added in tomcat.yml
```yml
---
java_package: openjdk-11-jdk
```
* And in the hosts file also changed group name tomcat
```
[tomcat]
172.31.27.136 
```
* for explanation of this command``sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat``
# use this command ``useradd --help`` in that all arguments are there
* -d (--home)(/home)
* -G (--groups)
* -s (--shell)
* -g (--gid)(groupid)
* -u (--uid)(userid)
* -m (--create-home)user’s home directory(/home/username)
* -D (--defaults)
* -e (--expiredate)
* -c (--comment) #Creating a User with Custom Comment
* -p password
* -U (--user-group)
![preview](../ansibleimages/ans28.png)
* Based on manual commands we can search in google ansible modules(example:user module in ansible, group module in ansible)after that based on parameters we will write the playbook.
* And if needed we can create variables for that in tomcat.yml(it's for variables)
* 