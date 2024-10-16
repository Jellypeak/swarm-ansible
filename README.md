# Docker Swarm Solution with Vagrant and Ansible

This project sets up a Docker Swarm cluster using Vagrant for provisioning virtual machines and Ansible for configuring the Docker Swarm environment. The setup includes both master and worker nodes, which are defined in the inventory file. This is ideal for local testing and development of Docker Swarm environments.

## Project Structure

```
.
├── README.md
├── Vagrantfile
├── collections
│   └── requirements.yml
├── playbooks
│   ├── reboot.yml
│   ├── reset.yml
│   ├── site.yml
│   └── upgrade.yml
└── sample-inventory.conf
```

### Prerequisites

Before you start, ensure that you have the following installed on your local machine:

> **NOTE:** If you have macOS with an ARM processor (e.g., Apple M1/M2 chips) or you're using Windows, this solution might not work properly due to compatibility issues with Vagrant and VirtualBox. In this case, it is recommended to either use **Ubuntu** to run the Vagrant local cluster or deploy the cluster on Ubuntu servers directly.


1. **Vagrant**: Download and install from [here](https://www.vagrantup.com/downloads).
2. **VirtualBox**: Download and install from [here](https://www.virtualbox.org/wiki/Downloads).
3. **Ansible**: Install Ansible by running the following command:
    ```bash
    sudo apt-get install ansible
    ```

### Getting Started

Follow these steps to set up and run the Docker Swarm cluster.

#### 1. Clone the Repository

```bash
git clone https://github.com/Jellypeak/swarm-ansible.git
cd swarm-ansible
```

#### 2. Install Required Ansible Collections

This project requires the community.docker Ansible collection. Install it by running:

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

#### 3. Start the Vagrant Environment

To start the virtual machines (VMs) and configure the Docker Swarm cluster, run the following command:

> **NOTE** before that copy `sample-inventory.conf` to `inventory.conf` and configure it for your purposes

```bash
vagrant up
```

This will:

* Create virtual machines as defined in the `Vagrantfile`.
* Install Docker on each machine.
* Set up a Docker Swarm cluster using the Ansible playbooks.
* The master and worker nodes will be set up according to the `inventory.conf` inventory file.


#### 4. SSH into the Virtual Machines

You can SSH into any virtual machine by running:

```bash
vagrant ssh <vm-name>
```

#### 5. Verify Docker Swarm Setup

Once logged into a master node, you can verify the Swarm setup with the following command:


```bash
docker node ls
```

This will show the nodes that are part of the Docker Swarm cluster.


## Available Playbooks
Here are the main Ansible playbooks provided in the playbooks/ directory:

* `site.yml`: Initializes the Docker Swarm cluster by setting up master and worker nodes.
* `reboot.yml`: Reboots all services in the Docker Swarm cluster.
* `reset.yml`: Resets the entire Docker Swarm cluster by removing all services and leaving the Swarm.
* `upgrade.yml`: Upgrades Docker services by pulling the latest images and recreating the services.

### Running a Playbook

You can run any of the playbooks with the following command:

```bash
ansible-playbook -i inventory.conf playbooks/<playbook-name>
```

For example, to set up the Docker Swarm cluster, run:

```bash
ansible-playbook -i sample-inventory.conf playbooks/site.yml
```

### Cleaning Up

To destroy the virtual machines when you're done, simply run:

```bash
vagrant destroy -f
```

This will remove all the created virtual machines and free up the resources.

