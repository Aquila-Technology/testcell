# vagrant init hashicorp/bionic64
Vagrant.configure("2") do |config|
	config.vm.box = "generic/centos7"
	config.vm.define "afsserver"
	config.vm.hostname = "afsserver.test.example.com"
	config.vm.provision "ansible" do |p|
		p.playbook = "provision.yml"
		p.host_vars = {
			"afsserver" => {
				"afs_cell" => "test.example.com",
				"afs_realm" => "TEST.EXAMPLE.COM",
				"afs_afsd_opts" => "-dynroot-sparse -fakestat -afsdb",
				"ansible_user" => "vagrant"
			}
		}
		p.groups = {
			"afs_kdcs" => ["afsserver"],
			"afs_databases" => ["afsserver"],
			"afs_fileservers" => ["afsserver"],
			"afs_clients" => ["afsserver"],
			"afs_cell:children" => ["afs_databases","afs_fileservers","afs_clients"],
		}
	end
end

