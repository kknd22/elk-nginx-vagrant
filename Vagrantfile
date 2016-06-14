# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'rbconfig'
is_windows = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/ubuntu1404"
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # ELK server.
  config.vm.define "elk" do |elk|
     elk.vm.hostname = "elkvm"
     elk.vm.network :private_network, ip: "192.168.9.90"

     if is_windows
        # Provisioning configuration for shell script.
        elk.vm.provision "shell" do |sh|
           sh.path = "provisioning/JJG-Ansible-Windows/windows.sh"
           #sh.args = ["/vagrant/provisioning/elk/playbook.yml", " --inventory", "/vagrant/provisioning/elk/inventory"]
           sh.args = "/vagrant/provisioning/elk/playbook.yml"
        end
     else
        # Provisioning configuration for Ansible (for Mac/Linux hosts).
        elk.vm.provision :ansible do |ansible|
           ansible.playbook = "provisioning/elk/playbook.yml"
           ansible.inventory_path = "provisioning/elk/inventory"
           ansible.sudo = true
           ansible.limit = "all"  ### <<-- adding this line
        end
     end
  end
end
