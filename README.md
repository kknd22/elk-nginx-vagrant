# elk-nginx-vagrant
vagrant provision ubuntu, elk, nginx based on https://github.com/geerlingguy/ansible-vagrant-examples/tree/master/elk    
and https://github.com/geerlingguy/JJG-Ansible-Windows

the following is copyu from the orignal README.md

# Ansible Vagrant profile for ELK (Elasticsearch, Logstash, Kibana)

## Background

Vagrant and VirtualBox (or some other VM provider) can be used to quickly build or rebuild virtual servers.

This Vagrant profile configures two servers:

  1. A server with the ELK stack ([Elasticsearch](http://www.elasticsearch.org/), [Logstash](http://logstash.net/), and [Kibana](http://www.elasticsearch.org/overview/kibana/)) using the [Ansible](http://www.ansible.com/) provisioner.
  2. A server with the Nginx and [Logstash Forwarder](https://github.com/elasticsearch/logstash-forwarder) using the [Ansible](http://www.ansible.com/) provisioner, which routes Nginx access logs to the first server.

## Getting Started

This README file is inside a folder that contains a `Vagrantfile` (hereafter this folder shall be called the [vagrant_root]), which tells Vagrant how to set up your virtual machine in VirtualBox.

To use the vagrant file, you will need to have done the following:

  1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
  3. Install [Ansible](http://ansibleworks.com/) ([guide for installing Ansible](http://docs.ansible.com/intro_installation.html))
  4. Open a shell prompt (Terminal app on a Mac) and cd into the folder containing the `Vagrantfile`
  5. 5. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`

Once all of that is done, you can simply type in `vagrant up`, and Vagrant will create both new VMs and configure them.

Once the VMs are up and running (after `vagrant up` is complete and you're back at the command prompt), you can log into either one via SSH if you'd like by typing in `vagrant ssh [name]` (either `logs` for the ELK server, or `webs` for the Nginx server). Otherwise, the next steps are below.

### Setting up your hosts file

You need to modify your host machine's hosts file (Mac/Linux: `/etc/hosts`; Windows: `%systemroot%\system32\drivers\etc\hosts`), adding the lines below:

    192.168.9.90  logs
    192.168.9.91  webs

(Where `logs`/`webs` is the hostname you have configured in the `Vagrantfile`).

After that is configured, you could visit http://logs/ in a browser, and you'll see the Kibana dashboard, and you can visit http://webs/, and you'll see Nginx's default index page.

If you'd like additional assistance editing your hosts file, please read [How do I modify my hosts file?](http://www.rackspace.com/knowledge_center/article/how-do-i-modify-my-hosts-file) from Rackspace.

## Author Information

Created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
