# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
sudo yum install -y git
sudo dnf config-manager --set-enabled crb
sudo systemctl stop firewalld
sudo systemctl disable firewalld
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky9"

  # if on virtualbox, create a host network vboxnet0
  #config.vm.network "private_network", type: "dhcp", name: "vboxnet0", auto_config: false

  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.memory = "4096"
    libvirt.cpus = 2 
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.synced_folder ".", "/vagrant", type: "rsync"

  # Run basic configuration script
  config.vm.provision "shell", inline: $script
end

