# Consul Automation with Ansible and Vagrant

## Preface

Just a fun project to build a consul cluster in vagrant with ansible.

Much thanks goes out to Brian Shumate's ansible consul role, which this
uses.

## Vagrant Development Environment Setup

In order to setup the development environment, the following tools are
required on your development workstation:
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](http://www.vagrantup.com/) with Ansible provisioner
* [Ansible](https://www.ansible.com/)
* [Python](https://www.python.org/) with python-pip / "pip3" installed

This was tested on the following versions of software:
* ansible 2.7.2
* vagrant 2.2.0
* virtualbox 5.2.22
* python 3.7.1
* python-pip 18.0
* consul 1.4.0

### Getting started
1. Install the required software from above, particularly Vagrant, VirtualBox,
   Python/Pip, and Ansible. Consul will be downloaded automatically.
2. Change directory into this solution repository
3. If needed, edit ansible.cfg and set the roles path to a writable path. It
   defaults to ./roles in this directory.
4. `ansible-galaxy install brianshumate.consul`
5. `vagrant up`
6. Validate the cluster web UI at: http://10.1.42.210:8500/ui/ or via:
   `vagrant ssh consul1 -c 'consul operator raft list-peers'`
