# Description
The repo contains a script to set up elastic cluster of 3 nodes on vagrant. The playbook in written so that it can be reused on any machine.
# Usage
- Install the required roles.
```
ansible-galaxy install -r requirements.yml
```
- Power up the vagrant VMs
```
vagrant up --no-provision
```
- Access the nodes over ssh to get them into known hosts or configure ssh not to check hosts for 192.168.77.*
```
ssh vagrant@192.168.77.21  
ssh vagrant@192.168.77.22
ssh vagrant@192.168.77.23
```
- Run ansible provisioning
```
ansible-playbook --private-key=~/.vagrant.d/insecure_private_key -u vagrant -i inventory/vagrant_ansible_inventory  playbook.yml
```
- Access *kopf* on ```http://localhost:19201/_plugin/kopf``` You should have 3 nodes running. Everything is green.
