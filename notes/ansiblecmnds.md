# Ansible install on ubuntu commands
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
ansible --version
```
# Installing and Configuring Ansible
* We will create two ubuntu vms
* Create a user called devops in two vms with sudo permissions
* Create a key-pair in ansible control node & copy the public key to other vm from ansible control node  <ssh-copy-id username@anothernode privateIPaddress> example``ssh-copy-id devops@172.168.123.11`` this command execute in ACN for connecting the node to ACN.
* Installing ansible
* Now lets add inventory. Create a file called as ``vi hosts`` with one entry <ipaddress>
* Check connectivity by executing ``ansible -i hosts -m ping all``
# Note:
* To enable password authentications edit config ``sudo vi /etc/ssh/sshd_config`` and set PasswordAuthentication to yes from no.
* restart sshd service ``sudo service sshd restart``



