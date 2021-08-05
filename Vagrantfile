# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Base VM OS configuration.
  config.vm.box = "bento/ubuntu-20.04"
  config.ssh.insert_key = false
  config.vm.synced_folder '.', '/vagrant', disabled: true

  # General VirtualBox VM configuration.
  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 1
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # MySQL.
  config.vm.define "db" do |db|
    db.vm.hostname = "db.test"
    db.vm.network :private_network, ip: "192.168.70.3"
    
    db.vm.provision "shell",
      inline: "sudo apt-get update -y && apt-get upgrade -y"
    
    db.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 1024]
    end
    
    db.vm.provision "ansible" do |ansible|
      ansible.playbook = "configuredb.yml"
      # ansible.inventory_path = "inventories/vagrant/inventory"
      ansible.limit = "db"
      ansible.extra_vars = {
        ansible_user: 'vagrant',
        # ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
      }
    end
  end

  config.vm.define "app" do |app|
    app.vm.hostname = "app.test"
    app.vm.network :private_network, ip: "192.168.70.4"

    app.vm.provision "shell",
      inline: "sudo apt-get update -y && apt-get upgrade -y"

    app.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 1024]
    end

    # Run Ansible provisioner once for all VMs at the end.
    app.vm.provision "ansible" do |ansible|
      ansible.playbook = "configure.yml"
      # ansible.inventory_path = "inventories/vagrant/inventory"
      ansible.limit = "app"
      ansible.extra_vars = {
        ansible_user: 'vagrant',
        # ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
      }
    end
  end
  
end
