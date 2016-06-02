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
  config.vm.define "logs" do |logs|
     logs.vm.hostname = "logs"
     logs.vm.network :private_network, ip: "192.168.9.90"

     if is_windows
        # Provisioning configuration for shell script.
        logs.vm.provision "shell" do |sh|
           sh.path = "provisioning/JJG-Ansible-Windows/windows.sh"
           #sh.args = ["/vagrant/provisioning/elk/playbook.yml", " --inventory", "/vagrant/provisioning/elk/inventory"]
           sh.args = "/vagrant/provisioning/elk/playbook.yml"
        end
     else
        # Provisioning configuration for Ansible (for Mac/Linux hosts).
        logs.vm.provision :ansible do |ansible|
           ansible.playbook = "provisioning/elk/playbook.yml"
           ansible.inventory_path = "provisioning/elk/inventory"
           ansible.sudo = true
        end
     end
  end

  # Web server.
  config.vm.define "webs" do |webs|
     webs.vm.hostname = "webs"
     webs.vm.network :private_network, ip: "192.168.9.91"

     if is_windows
        # Provisioning configuration for shell script.
        webs.vm.provision "shell" do |sh|
           sh.path = "provisioning/JJG-Ansible-Windows/windows.sh"
           #sh.args = ["/vagrant/provisioning/web/playbook.yml", "--inventory", "/vagrant/provisioning/web/inventory"]
           sh.args = "/vagrant/provisioning/web/playbook.yml"
        end
     else
        # Provisioning configuration for Ansible (for Mac/Linux hosts).
        webs.vm.provision :ansible do |ansible|
           ansible.playbook = "provisioning/web/playbook.yml"
           ansible.inventory_path = "provisioning/web/inventory"
           ansible.sudo = true
        end
     end
   end
end
