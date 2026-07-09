# Set up VMs for a Kubernetes cluster

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

# base image
BOX="cloud-image/ubuntu-26.04"
BOX_VERSION="20260627.0.0"

# node counts
NUM_CONTROLS=1
NUM_COMPUTES=2

# hardware
CPUS=4
MEMORY=4096
DISK="20GB"

Vagrant.configure("2") do |config|
  # https://docs.vagrantup.com.

  control_nodes = [
    { name: "control-1", ip: "192.168.56.10", playbook: "playbooks/configure-control.yml" },
  ]

  compute_nodes = [
    { name: "compute-1", ip: "192.168.56.20", playbook: "playbooks/configure-compute.yml" },
    { name: "compute-2", ip: "192.168.56.21", playbook: "playbooks/configure-compute.yml" }
  ]

  control_nodes.each do |node|
    config.vm.define node[:name] do |n|
      n.vm.box = BOX
      n.vm.box_version = BOX_VERSION
      n.vm.hostname = node[:name]
      n.vm.disk :disk, size: DISK, primary: true

      n.vm.network "private_network", ip: node[:ip]

      # virutalbox provider configuration
      n.vm.provider "virtualbox" do |vb|
      #  vb.gui = true
        vb.name = node[:name]
        vb.cpus = CPUS
        vb.memory = MEMORY

        # allow multi-core support
        vb.customize ["modifyvm", :id, "--ioapic", "on"]

        # fix for slow network speed issue
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      end

      # provisioning
      n.vm.provision "ansible" do |ansible|
        ansible.inventory_path = "inventory/hosts.ini"
        ansible.playbook = node[:playbook]
      end
    end
  end

  compute_nodes.each do |node|
    config.vm.define node[:name] do |n|
      n.vm.box = BOX
      n.vm.box_version = BOX_VERSION
      n.vm.hostname = node[:name]
      n.vm.disk :disk, size: "20GB", primary: true

      n.vm.network "private_network", ip: node[:ip]

      # virutalbox provider configuration
      n.vm.provider "virtualbox" do |vb|
      #  vb.gui = true
        vb.name = node[:name]
        vb.cpus = CPUS
        vb.memory = MEMORY

        # allow multi-core support
        vb.customize ["modifyvm", :id, "--ioapic", "on"]

        # fix for slow network speed issue
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      end

      # provisioning
      n.vm.provision "ansible" do |ansible|
        ansible.inventory_path = "inventory/hosts.ini"
        ansible.playbook = node[:playbook]
      end
    end
  end

end

