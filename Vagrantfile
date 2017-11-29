# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    v.memory = 2048
    v.cpus = 2
  end

  help_host_vars = Hash.new

  N = 3
  (1..N).each do |machine_id|
    config.vm.define "node-#{machine_id}" do |machine|

      machine.vm.hostname = "node-#{machine_id}"
      machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}"

      help_host_vars["node-#{machine_id}"] = {
        "ansible_host" => "192.168.77.#{20+machine_id}",
        "ansible_port" => "22"
      }

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == N
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
          ansible.groups = {
           "elasticsearch" => ["node-[1:3]"]
          }
          ansible.host_vars = help_host_vars
        end
      end
    end
  end

  config.vm.define "node-1" do |first|
    first.vm.network "forwarded_port", guest: 9200, host: 19201
    first.vm.network "forwarded_port", guest: 9300, host: 19301
  end
end
