
# -*-mode:ruby-*-
# vi:setft=ruby:

VAGRANTFILE_API_VERSION="2"

Vagrant.configure(VAGRANTFILE_API_VERSION)do|config|
  # General Vagrant VM configuration.
  config.vm.box = "bento/ubuntu-20.04"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :virtualbox do |v|
  v.memory = 512
  v.linked_clone = true
end

# Application server
config.vm.define "app" do |app|
  app.vm.hostname = "orc-app1.test"
  app.vm.network :private_network, ip: "192.168.70.1"
end

# Database server
config.vm.define "db" do |db|
  db.vm.hostname = "orc-db.test"
  db.vm.network :private_network, ip: "192.168.70.2"
end end