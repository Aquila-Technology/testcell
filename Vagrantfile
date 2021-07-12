# Name of AFS Cell (lower case)
cell_name = "test.example.com"
# Name of Kerberos realm (upper case)
realm_name = "TEST.EXAMPLE.COM"

# Number of server VMs to run
N = 3
# An array to keep the hostnames in
systems_array = []

# Configure the VMs
Vagrant.configure("2") do |config|
  # The OS the VMs will use
  config.vm.box = "generic/centos7"
  # A VM for an AFS client
  config.vm.define "afsclient"
  config.vm.hostname = "afsclient.#{cell_name}"

  (1..N).each do |system_id|
    config.vm.define "afsserver#{system_id}" do |afssystem|
      afssystem.vm.hostname = "afsserver#{system_id}.#{cell_name}"
      # Append to the list of Server VMs
      systems_array.append("afsserver#{system_id}")
      
      # Use Ansible to parallelize the management, only run on the
      # last configured VM (N), so you don't need to run ansible-playbook
      # multiple times
      if system_id == N
        afssystem.vm.provision "ansible" do |p|
          # Run against all hosts in the Ansible inventory
          p.limit = "all"
          # Run the Provisioning Ansible playbook
          p.playbook = "provision.yml"
          # Install the appropriate OpenAFS Ansible collection
          p.galaxy_role_file = "requirements.yml"
          # This sets up the Ansible inventory with the appropriate
          # variables for running Ansible
          p.groups = {
            "afs_kdcs" => systems_array,
            "afs_databases" => systems_array,
            "afs_fileservers" => systems_array,
            "afs_clients" => ("afsclient"),
            "afs_admin_client" => ("afsclient"),
            "afs_cell:children" => ["afs_databases",
                                    "afs_fileservers",
                                    "afs_clients",
                                    "afs_admin_client" ],
            "afs_cell:vars" => {
              "afs_cell" => "#{cell_name}",
              "afs_realm" => "#{realm_name}",
	      "afs_afsd_opts" => "-dynroot-sparse -fakestat -afsdb",
	      "ansible_user" => "vagrant"
            }
          }
        end
      end
    end
  end
end

