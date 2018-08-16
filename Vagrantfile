# -*- mode: ruby -*-
# vi: set ft=ruby :

# elk Vagrant file to spin up multiple machines running ELK stack
# Maintainer Michael Vilain [201801.26]


Vagrant.configure(2) do | config |
	# For a complete reference, please see the online documentation at
	# https://docs.vagrantup.com.

	# config.vm.box = 'centos/7'
	config.vm.box = 'centos7'
	# config.vm.network 'forwarded_port', guest: 80, host: 8080
	if Vagrant.has_plugin?("vagrant-proxyconf")
	    config.proxy.http     = "http://www-proxy.us.oracle.com:80"
	    config.proxy.https    = "https://www-proxy.us.oracle.com:80"
	    config.proxy.no_proxy = "localhost,127.0.0.1,.local"
	end
	config.vm.synced_folder '.', '/vagrant', disabled: false
	config.vm.provider :virtualbox do |vb|
		#vb.gui = true
		vb.memory = '1024'
	end

	# provision on all machines -- set hosts and allow ssh w/o checking
	# ius community repo has nginx
	config.vm.provision "shell", inline: %q|
		cat /vagrant/etc-hosts >> /etc/hosts
		sed -i.orig -e "s/#   CheckHostIP yes/CheckHostIP no/" /etc/ssh/ssh_config
	    # yum install -y https://centos7.iuscommunity.org/ius-release.rpm
	    yum install -y python libselinux-python
|

	# run ansible playbook on all nodes...node-specific stuff is in the playbook
	# config.vm.provision "ansible" do |ansible|
		# ansible.compatibility_mode = "2.0"
		# ansible.playbook = "playbook-cent.yml"
		# ansible.inventory_path = "./inventory"
		# ansible.verbose = "v"
		# ansible.raw_arguments = [""]
	# end

	config.vm.define "kibana1" do | kibana1 | 
		kibana1.vm.network 'private_network', ip: '192.168.10.101'
		kibana1.vm.hostname = 'kibana1'
		kibana1.ssh.insert_key = false
		kibana1.vm.provision "ansible" do |ansible| 
	        ansible.compatibility_mode = "2.0"
	        ansible.inventory_path = "./inventory"
	        ansible.playbook = "playbook-kibana.yml"
	        # ansible.verbose = "v"
	        # ansible.raw_arguments = [""]
		end
	end

	config.vm.define "elastic1" do | elastic1 |
		elastic1.vm.network 'private_network', ip: '192.168.10.201'
		elastic1.vm.hostname = 'elastic1'
		# elastic1.ssh.insert_key = false
		# elastic1.vm.provision "shell", inline: <<-SHELL2
		# SHELL2
	end

	config.vm.define "elastic2" do | elastic2 |
		elastic2.vm.network 'private_network', ip: '192.168.10.202'
		elastic2.vm.hostname = 'elastic2'
		# elastic2.ssh.insert_key = false
		# elastic2.vm.provision "shell", inline: <<-SHELL3
		# SHELL3
	end

	config.vm.define "logstash1", primary: true do | logstash1 |
		logstash1.vm.network 'private_network', ip: '192.168.10.100'
		logstash1.vm.hostname = 'logstash1'
		# logstash1.ssh.insert_key = false
		# logstash1.vm.network 'forwarded_port', guest: 80, host: 8080
		# logstash1.vm.provision "shell", inline: <<-SERVER
		# SERVER
	end

end
