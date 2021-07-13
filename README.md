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
    * I've tested VMWare Fusion on macOS and libvirtd on Fedora.  
    * Vagrant will required a [provider](https://www.vagrantup.com/docs/providers/installation) for your hypervisor.
* Start up the cell
  * Run `vagrant up`
  * Connect to the AFS Client VM by running `vagrant ssh afsclient`
