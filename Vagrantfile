# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "fedora/41-cloud-base"
  
#  config.vm.synced_folder "data", "/vagrant_data"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "libvirt" do |lvirt|
    lvirt.memory = "4096"
    lvirt.cpus = "8"
  end
  
  config.vm.provision "shell", inline: "dnf install -y python3-libdnf5"

  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "playbook.yaml"
  end
end
