
NODE_ROLES = [ "master1", "worker1" ]
NODE_BOXES = [ "ubuntu/xenial64", "ubuntu/xenial64" ]
NODE_CPUS = 1
NODE_MEMORY = 1024
NETWORK_PREFIX = "10.10.10"

def provision(vm, role, node_num)
  vm.box = NODE_BOXES[node_num]
  vm.hostname = role

  node_ip = "#{NETWORK_PREFIX}.#{100+node_num}"
  vm.network "private_network", type: "dhcp"
  vm.provision "ansible", run: 'once' do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "playbooks/site.yml"
    ansible.groups = {
      "master" => NODE_ROLES.grep(/^master/),
      "worker" => NODE_ROLES.grep(/^worker/),
    }
  end
end

Vagrant.configure("2") do |config|
  # config.vm.provider "libvirt" do |v|
  #   v.cpus = NODE_CPUS
  #   v.memory = NODE_MEMORY
  # end
  config.vm.provider "virtualbox" do |v|
    v.cpus = NODE_CPUS
    v.memory = NODE_MEMORY
    v.linked_clone = true
  end

  NODE_ROLES.each_with_index do |name, i|
    config.vm.define name do |node|
      provision(node.vm, name, i)
    end
  end
end