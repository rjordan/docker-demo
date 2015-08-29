# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/vivid64"

  config.vm.define "balancer" do |node|
    node.vm.hostname = "balancer"
    node.vm.network :private_network, :ip => '10.20.1.2'
    # For the default website
    node.vm.network "forwarded_port", guest: 80, host: 8080
    # For the non-routed WS port
    node.vm.network "forwarded_port", guest: 3001, host: 3001
    # For RabbitMQ Management
    node.vm.network "forwarded_port", guest: 15672, host: 8081
  end

  config.vm.define "docker1" do |node|
    node.vm.hostname = "docker1"
    node.vm.network :private_network, :ip => '10.20.1.3'
  end

  config.vm.provision 'ansible', run: :always do |ansible|
    ansible.groups = {
      "docker" => ["docker1"]
    }
    ansible.playbook = 'playbook.yml'
  end
end
