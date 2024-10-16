# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  # Define how many masters and workers you want
  MASTER_COUNT = 2
  WORKER_COUNT = 3
  MASTER_IP_PREFIX = "192.168.50.1"
  WORKER_IP_PREFIX = "192.168.50.2"
  NETWORK_PREFIX = "192.168.50."

  # All VMs will be configured to use a private network
  config.vm.network "private_network", type: "dhcp"

  # Common settings for all VMs
  config.vm.box = "ubuntu/bionic64"
  # Loop to create master nodes
  MASTER_COUNT.times do |i|
    config.vm.define "master#{i+1}" do |master|
      master.vm.hostname = "master#{i+1}"
      master.vm.network "private_network", ip: "#{NETWORK_PREFIX}#{10 + i}"

      master.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 2
      end

      master.vm.provision "shell", inline: <<-SHELL
        echo "Setting up Docker Swarm master node #{i+1}"
      SHELL
    end
  end

  # Loop to create worker nodes
  WORKER_COUNT.times do |i|
    config.vm.define "worker#{i+1}" do |worker|
      worker.vm.hostname = "worker#{i+1}"
      worker.vm.network "private_network", ip: "#{NETWORK_PREFIX}#{20 + i}"

      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 2
      end

      worker.vm.provision "shell", inline: <<-SHELL
        echo "Setting up Docker Swarm worker node #{i+1}"
      SHELL
    end
  end

  # Provision with Ansible to run the playbook
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook/site.yml"
    ansible.inventory_path = "playbook/inventory.conf"
    ansible.limit = "all"
  end

end
