# -*- mode: ruby -*-
# vi: set ft=ruby :

host_project_dir = File.dirname(File.expand_path(__FILE__))

require 'yaml'
# Load default VM configurations.
vconfig = YAML.load_file("#{host_project_dir}/vars/common.yml")
# Use optional config.yml and local.config.yml for configuration overrides.

if File.exist?("#{host_project_dir}/vars/config.yml")
  vconfig.merge!(YAML.load_file("#{host_project_dir}/vars/config.yml"))
end

Vagrant.configure("2") do |config|

  config.vm.box = vconfig['vm_box']
  config.vm.hostname = vconfig['vm_hostname']

  config.vm.network :private_network, :auto_network => true

  config.vm.provider "virtualbox" do |v|
    v.name = vconfig['vm_hostname']
    v.memory = vconfig['vm_memory']
    v.cpus = vconfig['vm_cpus']
  end

  config.vm.synced_folder ".", "/vagrant"
  # Synced folders.
  vconfig['vagrant_synced_folders'].each do |synced_folder|
    config.vm.synced_folder synced_folder['local_path'], synced_folder['destination'], create: true
  end

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.provision "base", :type => "ansible_local" do |ansible|
    ansible.playbook = "playbook-base.yml"
  end

  config.vm.provision "amp", :type => "ansible_local" do |ansible|
    ansible.playbook = "playbook-amp.yml"
  end

  config.vm.provision "varnish", :type => "ansible_local" do |ansible|
    ansible.playbook = "playbook-varnish.yml"
  end

  config.vm.provision "nginx", :type => "ansible_local" do |ansible|
    ansible.playbook = "playbook-nginx.yml"
  end

  config.vm.provision "finish", :type => "ansible_local" do |ansible|
    ansible.playbook = "playbook-finish.yml"
  end

end
