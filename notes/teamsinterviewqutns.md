1. What is Ansible, and what are its key features?
* [referhere](https://www.devopsschool.com/blog/what-are-the-key-features-and-specific-roles-of-ansible/)

2. How do you configure Ansible and nodes in a network environment?
* Set up SSH connections so Ansible can connect to the managed nodes. Add your public SSH key to the authorized_keys file on each remote system. Test the SSH connections, for example: ssh username@192.0.2.50 If the username on the control node is different on the host, you need to pass the -u option with the ansible command. Ping the managed nodes.

3. Can you explain the concept of a playbook in Ansible?
* 
What is configuration management, and how does Ansible facilitate it?
What is an inventory in Ansible, and how is it used?
Please explain the difference between static and dynamic inventory in Ansible.

4. What is Ansible Vault, and how is it used to secure sensitive information?
Ansible vault is used to keep sensitive data, like passwords, rather than placing it as plain text in playbooks or roles. Any structured data file or single value inside a YAML file can be encrypted by Ansible.

To encrypt the data, the following command is given:

``Command: ansible-vault encrypt foo.yml bar.yml baz.yml``
To decrypt the data,  the following command is given:

``Command: ansible-vault decrypt foo.yml bar.yml baz.yml``

5. What is a Jinja template, and what is the difference between a template and a Jinja template in Ansible?
* read this documentation[referhere](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_templating.html)
* Ansible uses Jinja2 templating to enable dynamic expressions and access to variables and facts. You can use templating with the template module. For example, you can create a template for a configuration file, then deploy that configuration file to multiple environments and supply the correct data (IP address, hostname, version) for each environment. You can also use templating in playbooks directly, by templating task names and more. You can use all the standard filters and tests included in Jinja2. Ansible includes additional specialized filters for selecting and transforming data, tests for evaluating template expressions, and Lookup plugins for retrieving data from external sources such as files, APIs, and databases for use in templating.

All templating happens on the Ansible controller before the task is sent and executed on the target machine. This approach minimizes the package requirements on the target (jinja2 is only required on the controller). It also limits the amount of data Ansible passes to the target machine. Ansible parses templates on the controller and passes only the information needed for each task to the target machine, instead of passing all the data on the controller and parsing it on the target.

6. Which modules do you commonly use in your organization when working with Ansible?
* [referhere](https://opensource.com/article/19/9/must-know-ansible-modules)
* Module: Ansible modules are standalone scripts that can be used inside an Ansible playbook. A playbook consists of a play, and a play consists of tasks. 

7. Can you explain the concepts of roles, tasks, and handlers in Ansible?
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


8.  How do you configure group inventory in Ansible?
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

9. What are modules in Ansible, and how are they used?
[referhere](https://opensource.com/article/19/9/must-know-ansible-modules)
* stat: stat module is used for stop the repetation process,means skip the unnessasery extraction of application.

10.  Can you provide an overview of Ansible Collections and their significance?
* Collections are a distribution format for Ansible content that can include playbooks, roles, modules, and plugins. You can install and use collections through a distribution server, such as Ansible Galaxy, or a Pulp 3 Galaxy server.
[referhere](https://docs.ansible.com/ansible/latest/collections_guide/index.html)

11.  How do you pass values dynamically when running a playbook in Ansible?
* Defining variables at runtime:
You can define variables when you run your playbook by passing variables at the command line using the --extra-vars (or -e) argument. You can also request user input with a vars_prompt (see Interactive input: prompts). When you pass variables at the command line, use a single quoted string, that contains one or more variables, in one of the formats below.

key=value format
Values passed in using the key=value syntax are interpreted as strings. Use the JSON format if you need to pass non-string values such as Booleans, integers, floats, lists, and so on.
example command is ``ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"``
JSON string format``ansible-playbook release.yml --extra-vars '{"version":"1.23.45","other_variable":"foo"}'``
12. Ansible collections: Collections are a distribution format for Ansible content that can include playbooks, roles, modules, and plugins. You can install and use collections through a distribution server, such as Ansible Galaxy, or a Pulp 3 Galaxy server.
[referhere](https://docs.ansible.com/ansible/latest/collections_guide/collections_installing.html#installing-a-collection-from-a-git-repository)for collections documentation.command is ``ansible-galaxy collections init collections.skelton``
* In ansible reuseable assets are 
    * roles
    * modules
* In ansible we use custom modules
* Advantages of collections it is easy to find modules,in collections have roles and modules
* For create the role command is``ansible-galaxy role init <rolename>``example ``ansible-galaxy role init skelton``