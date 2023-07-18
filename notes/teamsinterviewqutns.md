1. What is Ansible, and what are its key features?
* [referhere](https://www.devopsschool.com/blog/what-are-the-key-features-and-specific-roles-of-ansible/)

2. How do you configure Ansible and nodes in a network environment?
* Set up SSH connections so Ansible can connect to the managed nodes. Add your public SSH key to the authorized_keys file on each remote system. Test the SSH connections, for example: ssh username@192.0.2.50 If the username on the control node is different on the host, you need to pass the -u option with the ansible command. Ping the managed nodes.

3. Can you explain the concept of a playbook in Ansible?
* 

4. What is configuration management, and how does Ansible facilitate it?

5. What is an inventory in Ansible, and how is it used?
* Inventory: A list of managed nodes.
* Inventory in Ansible represents the hosts which we need to connect to.
* Ansible inventory is broadly classified into two types
     * static inventory: where we mention the list of nodes to connect to in some file
     * dynamic inventory: where we mention some script/plugin which will dynamically find out the nodes to connect to

6. Please explain the difference between static and dynamic inventory in Ansible.
* Static inventory can be mentioned in two formats
   * ini
   * yaml
Ini format: An INI file is a configuration file for computer software that consists of a text-based content with a structure and syntax comprising key–value pairs for properties, and sections that organize the properties.
* dynamic inventory: where we mention some script/plugin which will dynamically find out the nodes to connect to
  
7. What is Ansible Vault, and how is it used to secure sensitive information?
Ansible vault is used to keep sensitive data, like passwords, rather than placing it as plain text in playbooks or roles. Any structured data file or single value inside a YAML file can be encrypted by Ansible.

To encrypt the data, the following command is given:

``Command: ansible-vault encrypt foo.yml bar.yml baz.yml``
To decrypt the data,  the following command is given:

``Command: ansible-vault decrypt foo.yml bar.yml baz.yml``

8. What is a Jinja template, and what is the difference between a template and a Jinja template in Ansible?
* read this documentation[referhere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_templating.html)
* Ansible uses Jinja2 templating to enable dynamic expressions and access to variables and facts. You can use templating with the template module. For example, you can create a template for a configuration file, then deploy that configuration file to multiple environments and supply the correct data (IP address, hostname, version) for each environment. You can also use templating in playbooks directly, by templating task names and more. You can use all the standard filters and tests included in Jinja2. Ansible includes additional specialized filters for selecting and transforming data, tests for evaluating template expressions, and Lookup plugins for retrieving data from external sources such as files, APIs, and databases for use in templating.

All templating happens on the Ansible controller before the task is sent and executed on the target machine. This approach minimizes the package requirements on the target (jinja2 is only required on the controller). It also limits the amount of data Ansible passes to the target machine. Ansible parses templates on the controller and passes only the information needed for each task to the target machine, instead of passing all the data on the controller and parsing it on the target.

9. Which modules do you commonly use in your organization when working with Ansible?
* [referhere](https://opensource.com/article/19/9/must-know-ansible-modules)
* Module: Ansible modules are standalone scripts that can be used inside an Ansible playbook. A playbook consists of a play, and a play consists of tasks. 
* debug module: This module prints statements during execution and can be useful for debugging variables or expressions without necessarily halting the playbook.

Useful for debugging together with the ‘when:’ directive.

This module is also supported for Windows targets.

10. Can you explain the concepts of roles, tasks, and handlers in Ansible?
Roles:
------
roles are reusable,roles are not run ownly if we call the roles then the roles are running for that we write a playbook for our own role
```yml
---
- name: calling my roles
  hosts: all
  become: yes
  roles: 
    - role: postgres
```
* In the above playbook my role name is postgres,and first i created my ownrole with postgres name in that role having handlers,tasks,vars, in that main.yml in that our playbook is there diveded the parts and paste in that main.yml
tasks:
------
The task is a unit action in Ansible. It breaks a configuration policy into smaller files or blocks of code. These blocks can be useful in automating a process, for example, to install a package or update software:

Command: Install <package_name>

Command: update <software_name>

handlers:
---------
 Handlers are used to trigger the status of a service such as restarting or stopping a service.

11.  How do you configure group inventory in Ansible?
* hosts is collection of all machines,but group is collection of individual machines and operating system.
* 172.31.10.81
localhost

[ubuntu]
172.31.10.81
localhost

[centOS]
172.31.45.202

[webservers]
web[01:10].qt.com
or
---
all:
  children:
    ubuntu:
      hosts:
        172.31.27.91:
    redhat:
      hosts:
        172.31.20.69:
    webserver:
      hosts:
        172.31.27.91:
        172.31.20.69:

12. What are modules in Ansible, and how are they used?
[referhere](https://opensource.com/article/19/9/must-know-ansible-modules)
* stat: stat module is used for stop the repetation process,means skip the unnessasery extraction of application.

13.  Can you provide an overview of Ansible Collections and their significance?
* Collections are a distribution format for Ansible content that can include playbooks, roles, modules, and plugins. You can install and use collections through a distribution server, such as Ansible Galaxy, or a Pulp 3 Galaxy server.
[referhere](https://docs.ansible.com/ansible/latest/collections_guide/index.html)
[referhere](https://docs.ansible.com/ansible/latest/collections_guide/collections_installing.html#installing-a-collection-from-a-git-repository)for collections documentation.command is ``ansible-galaxy collections init collections.skelton``

14.  How do you pass values dynamically when running a playbook in Ansible?
* Defining variables at runtime:
You can define variables when you run your playbook by passing variables at the command line using the --extra-vars (or -e) argument. You can also request user input with a vars_prompt (see Interactive input: prompts). When you pass variables at the command line, use a single quoted string, that contains one or more variables, in one of the formats below.

key=value format
Values passed in using the key=value syntax are interpreted as strings. Use the JSON format if you need to pass non-string values such as Booleans, integers, floats, lists, and so on.
example command is ``ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"``
JSON string format``ansible-playbook release.yml --extra-vars '{"version":"1.23.45","other_variable":"foo"}'``
15. roles:
* In ansible reuseable assets are 
    * roles
    * modules
* In ansible we use custom modules
* Advantages of collections it is easy to find modules,in collections have roles and modules
* For create the role command is``ansible-galaxy role init <rolename>``example ``ansible-galaxy role init skelton``

# Facts vs variables
Ansible facts are data collected about the (target) systems on which Ansible takes actions. They are variables, but set by Ansible (in a way like system defined variables). They are collected during Gathering Facts stage of a playbook run, and it is controlled by the gather_facts setting. Ansible calls this variables discovered from systems. That said, it is possible to set custom facts also.
Some examples are:

ansible_hostname - the FQDN of the target system
ansible_os_family - the Operating System family of target system (RedHat, Debian, etc.)

# variables
* The other variables are the ones we can set as per our requirement (in a way like user defined variables).

Some examples are:

my_fav_fruits: [ 'orange', 'apple', 'banana' ] - yours might differ.
config_dir: '/etc/my_app/conf.d' - for my application configuration files.

# Tags
* If you have a large playbook, it may be useful to run only specific parts of it instead of running the entire playbook. You can do this with Ansible tags. Using tags to execute or skip selected tasks is a two-step process:

* Add tags to your tasks, either individually or with tag inheritance from a block, play, role, or import.

* Select or skip tags when you run your playbook.

