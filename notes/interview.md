Ansible Interview Questions
----------------------------
# What is Ansible ?
* It is an open-source automation tool, or platform, used for IT tasks such as configuration management, application deployment, intraservice orchestration, and provisioning.
* Ansible is a tool written in Python, and it uses the declarative markup language YAML to describe the desired state of devices and configuration. In association with the idea of a "desired state," Ansible also uses the concept of idempotency.(Ansible it means after 1 run of a playbook to set things to the desired state, further runs of the same playbook should result in 0 changes.)[referhere](https://gauravtank2203.medium.com/idempotency-in-ansible-using-httpd-977239f99075)
* Function of Ansible roles allow you to develop reusable automation components by grouping and encapsulating related automation artifacts, like configuration files, templates, tasks, and handlers.
# What is a node in Ansible?
* Ansible control nodes are primarily used to run tasks on managed hosts. You can use any machine with Python installed as an Ansible control node. However, you cannot use Windows as an Ansible control node. Managed nodes: Hosts that you manage using Ansible.
Inventory: A list of managed nodes.
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
* An effective way is organization creates a service account for the admins to login and perform administration.
![preview](../ansibleimages/ans00.png)
* For the lab activities our service account’s name would be devops
* Having username and password is not a sensible option then how to solve this problem
# How to setup key pair based authentication in linux machines
* Key pair is combination of two keys public and private using alortihms, we will be using RSA
* Create a key pair ssh-keygen
* Copy the public key to linux machine ssh-copy-id username@ipaddress
* connect to the machine using private key ssh -i <path-to-private key> username@ipaddress
* Generally private keys created will have extension of .pem
* i.e we create a Service account public and private key. Copy the service account public key to all the server  disable password based authentication
* Setting up sudo permissions:
     * We need to add devops user to the sudoers group (Wheel)
     * Execute sudo visudo
* We need atleast two linux machines
     * one is Ansible control node
     * others is/are nodes
* We will be creating a service account called as devops in all machines
* We will be creating a key pair in Ansible control node
* Copy the public key into the nodes
* Optional: Disable password based authentication

# Insted of sh command we used below in previous ansible playbooks we used sh command directly but it is not safe so we use this cmd command with use of shell module
example:      
```
ansible.builtin.shell:
  cmd: "chmod +x {{ homedir }}/latest/bin/*.sh"
```

  