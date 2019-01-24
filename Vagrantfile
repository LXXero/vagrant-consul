# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# `Vagrantfile` for bootstrapping a development Consul cluster with
# VirtualBox provider and Ansible provisioner
#
# This was modified from the original, available at:
# https://github.com/brianshumate/ansible-consul/tree/master/examples
#

ANSIBLE_PLAYBOOK = ENV['ANSIBLE_PLAYBOOK'] || "site.yml"
BOX_MEM = ENV['BOX_MEM'] || "1024"
BOX_NAME =  ENV['BOX_NAME'] || "ubuntu/trusty64"
CLUSTER_HOSTS = ENV['CLUSTER_HOSTS'] || "vagrant_hosts"
CONSUL_ACL_ENABLE = ENV['CONSUL_ACL_ENABLE'] || "false"
CONSUL_ATLAS_ENABLE = ENV['CONSUL_ATLAS_ENABLE'] || "false"
CONSUL_DNSMASQ_ENABLE = ENV['CONSUL_DNSMASQ_ENABLE'] || "false"
CONSUL_LOGLEVEL = ENV['CONSUL_LOGLEVEL'] || "INFO"
CONSUL_LOG_PATH = ENV['CONSUL_LOG_PATH'] || "/var/log/consul"
CONSUL_LOG_FILE = ENV['CONSUL_LOG_FILE'] || "consul.log"
CONSUL_SYSLOG_FACILITY = ENV['CONSUL_SYSLOG_FACILITY'] || "local0"
SYSLOG_USER = ENV['SYSLOG_USER'] || "syslog"
SYSLOG_GROUP = ENV['SYSLOG_GROUP'] || "adm"
CONSUL_NODE_OS = ENV['CONSUL_NODE_OS'] || "Linux"
VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.9.0"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if BOX_NAME.include? "freebsd"
    CONSUL_IFACE = "em1"
  end
  # Configure 3 Consul nodes
  config.vm.define :consul1 do |consul1_config|
    consul1_config.vm.box = BOX_NAME
    # FreeBSD needs a MAC, disabled synced folder, and explicit shell
    consul1_config.vm.base_mac = "080027D17374"
    consul1_config.vm.synced_folder ".", "/vagrant", disabled: true
    consul1_config.ssh.shell = "/bin/sh"
    consul1_config.vm.network :private_network, ip: "10.1.42.210"
    consul1_config.vm.hostname = "consul1.consul"
    consul1_config.ssh.forward_agent = true
    consul1_config.vm.provider "virtualbox" do |v|
      v.name = "consul-node1"
      v.customize ["modifyvm", :id, "--memory", BOX_MEM]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      #v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      #v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    consul1_config.vm.post_up_message = "Consul server 1 spun up!"
    end
  end
  config.vm.define :consul2 do |consul2_config|
    consul2_config.vm.box = BOX_NAME
    # FreeBSD needs a MAC, disabled synced folder, and explicit shell
    consul2_config.vm.base_mac = "080027D27374"
    consul2_config.vm.synced_folder ".", "/vagrant", disabled: true
    consul2_config.ssh.shell = "/bin/sh"
    consul2_config.vm.network :private_network, ip: "10.1.42.220"
    consul2_config.vm.hostname = "consul2.consul"
    consul2_config.ssh.forward_agent = true
    consul2_config.vm.provider "virtualbox" do |v|
      v.name = "consul-node2"
      v.customize ["modifyvm", :id, "--memory", BOX_MEM]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      #v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      #v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    consul2_config.vm.post_up_message = "Consul server 2 spun up!"
    end
  end
  config.vm.define :consul3 do |consul3_config|
    consul3_config.vm.box = BOX_NAME
    # FreeBSD needs a MAC, disabled synced folder, and explicit shell
    consul3_config.vm.base_mac = "080027D37374"
    consul3_config.vm.synced_folder ".", "/vagrant", disabled: true
    consul3_config.ssh.shell = "/bin/sh"
    consul3_config.vm.network :private_network, ip: "10.1.42.230"
    consul3_config.vm.hostname = "consul3.consul"
    consul3_config.ssh.forward_agent = true
    consul3_config.vm.provider "virtualbox" do |v|
      v.name = "consul-node3"
      v.customize ["modifyvm", :id, "--memory", BOX_MEM]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      #v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      #v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    consul3_config.vm.post_up_message = "Consul server 3 spun up!\n\nAccess http://consul1.consul:8500/ui/ in a browser for Consul UI."
    end
    consul3_config.vm.provision :ansible do |ansible|
      ansible.inventory_path = CLUSTER_HOSTS
      # As if variable related things in Ansible couldn't be more exciting,
      # extra Ansible variables can be defined here as well. Wheeee!
      #
      #ansible.extra_vars = {
      #  consul_log_level: "DEBUG",
      #  consul_client_address: "10.1.42.230"
      #}
      ansible.playbook = ANSIBLE_PLAYBOOK
      ansible.limit = "all"
      compatibility_mode = "2.0"
    end
  end
end
