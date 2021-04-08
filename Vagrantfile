# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "debian/contrib-buster64"
    nginx.vm.hostname = "infrachallenge"
    nginx.vm.network "private_network", ip: "10.2.2.100"
    config.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
    end
  end

  N = 3
  (1..N).each do |node_id|
    config.vm.define "node#{node_id}" do |node|
      node.vm.box = "debian/contrib-buster64"
      node.vm.hostname = "node#{node_id}"
      node.vm.network "private_network", ip: "10.2.2.#{100+node_id}"
      config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
      end

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if node_id == N
        node.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "infra.yml"
          ansible.inventory_path = "inventory"
        end
      end
    end
  end
end
