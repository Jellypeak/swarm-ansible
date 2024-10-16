Vagrant.configure("2") do |config|

  # Define how many masters and workers you want
  MASTER_COUNT = 1
  WORKER_COUNT = 1

  # VM settings
  config.vm.box = "ubuntu/bionic64"

  # Loop to create master nodes
  MASTER_COUNT.times do |i|
    config.vm.define "master#{i+1}" do |master|
      master.vm.hostname = "master#{i+1}"
      master.vm.network "private_network", type: "dhcp"
      master.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
      end
      master.vm.provision "shell", inline: "echo 'Setting up Docker Swarm master node #{i+1}'"
    end
  end

  # Loop to create worker nodes
  WORKER_COUNT.times do |i|
    config.vm.define "worker#{i+1}" do |worker|
      worker.vm.hostname = "worker#{i+1}"
      worker.vm.network "private_network", type: "dhcp"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
      end
      worker.vm.provision "shell", inline: "echo 'Setting up Docker Swarm worker node #{i+1}'"
    end
  end

  # Task to generate dynamic inventory
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    echo "[master]" > inventory.ini
    for i in $(seq 1 #{MASTER_COUNT}); do
      vagrant ssh-config master$i | grep HostName | awk '{print "master"i " ansible_host="$2}' >> inventory.ini
    done
    echo "[worker]" >> inventory.ini
    for i in $(seq 1 #{WORKER_COUNT}); do
      vagrant ssh-config worker$i | grep HostName | awk '{print "worker"i " ansible_host="$2}' >> inventory.ini
    done
    echo "[all:vars]" >> inventory.ini
    echo "ansible_user=vagrant" >> inventory.ini
    echo "ansible_ssh_private_key_file=.vagrant/machines/master1/virtualbox/private_key" >> inventory.ini
  SHELL
end
