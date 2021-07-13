# Set up an OpenAFS cell on your personal device
This project up a very simple test cell using Vagrant and Ansible.

## Getting Started
* Prerequeisites 
  * Download and install [Vagrant](https://www.vagrantup.com/):
    * On Linux and Windows, install from the [Vagrant download page](https://www.vagrantup.com/downloads).
    * On MacOS, install from the above downloads page or via [Homebrew](https://brew.sh/):
      `$ brew install vagrant`
  * Download and install Ansible
    * Install from your [local package manager](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-specific-operating-systems) or [from `pip`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip)
  * Install a hypervisor.
    * I've tested VMWare Fusion on macOS and libvirtd on Fedora.  VirtualBox is also a popular hypervisor to use with Vagrant and will work too.
    * Vagrant will required a [provider](https://www.vagrantup.com/docs/providers/installation) for your hypervisor.
* Start up the cell
  * Run `vagrant up`
  * Connect to the AFS Client VM by running `vagrant ssh afsclient`
  * Here is a vagrant shell running some basic AFS commands including `vos release`:
    ![Running vos reelase](./docs/vos_release.svg)
* How does it work?
  * This test cell is made possible through the very powerful and useful [Ansible Collection for OpenAFS](https://github.com/openafs-contrib/ansible-openafs).
  * The [`provision.yml`](./provision.yml) Ansible playbook is a slightly modified version of the [`realm.yaml`](https://github.com/openafs-contrib/ansible-openafs/blob/master/playbooks/realm.yaml) and [`cell.yaml`](https://github.com/openafs-contrib/ansible-openafs/blob/master/playbooks/cell.yaml) from the openafs_contrib.openafs Collection, with two minor tweaks:
    * I modify /etc/hosts so all the hostnames and IPs of the VMs are defined, which fixes some issues with Vagrant on VMware Fusion
    * I update all the packages before running any of the openafs roles.
  * It is possible to use this test cell without using Vagrant.
    * Set up an inventory that would look like this:

    [afs_kdcs]
    afsserver1
    afsserver2
    afsserver3

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

