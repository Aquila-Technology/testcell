# Set up an OpenAFS cell on your personal device

Ever wanted to run [OpenAFS](https://www.openafs.org) but didn't know how to set it up?  Maybe you've used it but wanted to learn more about how to be an administrator?  The aim of this project is to make setting up a small test cell really easy.  We just need a couple tools like Ansible and Vagrant, and a functional Hypervisor such as VMWare or VirtualBox, and you're all set!

## Getting Started
* Prerequisites 
  * Hardware Requirements:
    The default configuration runs 4 separate VMs, so you need some resources
    * At least 8G of RAM, 16G is reccomended
    * At least 60G of free disk space
    * A CPU that supports virtualization of some sort.
    * An x86_64/amd64 CPU. (It might run on arm64 but I've not tried)
  * If you don't have these kinds of resources, edit the [Vagrantfile](./Vagrantfile) and change the `N` value to 1.  This will limit the cell to only 1 server and 1 client.  You won't be able to do as much with it, and you miss out on some of the features OpenAFS has for running a cluster of hosts.
## Installing
* Download and install [Vagrant](https://www.vagrantup.com/):
  * On Linux and Windows, install from the [Vagrant download page](https://www.vagrantup.com/downloads).
  * On MacOS, install from the above downloads page or via [Homebrew](https://brew.sh/):
    `$ brew install vagrant`
* Download and install Ansible
  * Install from your [local package manager](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-specific-operating-systems) or [from `pip`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip)
* Install a hypervisor.
  * I've tested VMWare Fusion on macOS and libvirtd on Fedora.  VirtualBox is also a popular hypervisor to use with Vagrant and will work too.
  * Vagrant will required a [provider](https://www.vagrantup.com/docs/providers/installation) for your hypervisor.
## Start up the cell
* Clone this repository and `cd` into it.
* Run `vagrant up`
* Connect to the AFS Client VM by running `vagrant ssh afsclient`
* Here's a [brief demo](./docs/vos_release.svg) of using Vagrant to run some AFS comments
* I suggest following the [OpenAFS User Guide](https://docs.openafs.org/UserGuide/index.html) with the new cell to learn how to use AFS.
## How does it work?
* This test cell is made possible through the very powerful and useful [Ansible Collection for OpenAFS](https://github.com/openafs-contrib/ansible-openafs).
* The [`provision.yml`](./provision.yml) Ansible playbook is a slightly modified version of the [`realm.yaml`](https://github.com/openafs-contrib/ansible-openafs/blob/master/playbooks/realm.yaml) and [`cell.yaml`](https://github.com/openafs-contrib/ansible-openafs/blob/master/playbooks/cell.yaml) from the openafs_contrib.openafs Collection, with two minor tweaks:
  * I modify /etc/hosts so all the hostnames and IPs of the VMs are defined, which fixes some issues with Vagrant on VMware Fusion
  * I update all the packages before running any of the openafs roles.
## But I don't want to use Vagrant!
* It is possible to use this test cell without using Vagrant.  (You still need Ansible!)
* If you're familiar with Ansible, set up your own VMs, and set up an inventory that would look like this:
```
# Example Inventory

[afs_kdcs]
afsserver1

[afs_databases]
afsserver1
afsserver2
afsserver3

[afs_fileservers]
afsserver1
afsserver2
afsserver3

[afs_clients]
afsclient

[afs_admin_client]
afsclient

[afs_cell:children]
afs_databases
afs_fileservers
afs_clients
afs_admin_client

[afs_cell:vars]
afs_cell=test.example.com
afs_realm=TEST.EXAMPLE.COM
afs_afsd_opts=-dynroot-sparse -fakestat -afsdb
ansible_user=USERNAME
```
## Questions?  Comments?
Please file any issues you have in the [Github Issues](https://github.com/Aquila-Technology/testcell/issues) page for this repository.
