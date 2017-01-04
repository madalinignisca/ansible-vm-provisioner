# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "small.vm"

  config.vm.network :private_network, :type => "dhcp"

  config.vm.provider "virtualbox" do |v|
    v.name = "small-vm"
    v.memory = 512
    v.cpus = 1
  end

  config.vm.synced_folder ".", "/vagrant"
  config.vm.synced_folder "./demo", "/var/www/small.vm", create: true

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
