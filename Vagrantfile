# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	# use different plugins when behind a proxy
	if ENV['http_proxy'].nil? || ENV['http_proxy'] == ""
		required_plugins = %w( vagrant-vbguest vagrant-disksize )
	else
		required_plugins = %w( vagrant-vbguest vagrant-disksize vagrant-proxyconf )  
	end

	_retry = false
	required_plugins.each do |plugin|
		unless Vagrant.has_plugin? plugin
			system "vagrant plugin install #{plugin}"
			_retry=true
		end
	end	
	
	if Vagrant.has_plugin?("vagrant-vbguest")
		config.vbguest.auto_update = false
	end

	if Vagrant.has_plugin?("vagrant-disksize")
		config.disksize.size = '10GB'
	end

	if Vagrant.has_plugin?("vagrant-proxyconf")
		# let CNTLM listen on the vboxnet interface, set your localhost
		# as the proxy for VirtualBox machines, so APT can get through
		# note: the host is exposed to guests as 10.0.2.2; no need to bind
		# CNTLM to any other NIC, 127.0.0.1 is sufficient.
		config.proxy.http	 = "http://10.0.2.2:3128/"
		config.proxy.https	= "http://10.0.2.2:3128/"
		config.proxy.no_proxy = "localhost,127.0.0.1,10.,192.168.,.example.com"
	end

	# Use the same key for each machine
	config.ssh.insert_key = false
	config.vm.box_check_update = false

	config.vm.define "vm01" do |guest|
		guest.vm.box = "ubuntu/bionic64"
		guest.vm.hostname = "vm01"
		guest.vm.network "forwarded_port", guest: 80, host: 8080
		guest.vm.network "forwarded_port", guest: 443, host: 8443

		guest.vm.provider "virtualbox" do |vb|
			vb.name = "Virtual Machine 01"
			vb.memory = "3072"
			vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
		end

		guest.vm.provision "shell" do |sh|
			sh.path = "setup.sh"
			sh.privileged = false
		end	
	end
	
	config.vm.define "vm02" do |guest|
		guest.vm.box = "ubuntu/bionic64"
		guest.vm.hostname = "vm02"
		guest.vm.network "forwarded_port", guest: 80, host: 8081
		guest.vm.network "forwarded_port", guest: 443, host: 8444

		guest.vm.provider "virtualbox" do |vb|
			vb.name = "Virtual Machine 02"
			vb.memory = "3072"
			vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
		end

		guest.vm.provision "shell" do |sh|
			sh.path = "setup.sh"
			sh.privileged = false
		end	
	end

	config.vm.define "vm03" do |guest|
		guest.vm.box = "ubuntu/bionic64"
		guest.vm.hostname = "vm03"
		guest.vm.network "forwarded_port", guest: 80, host: 8082
		guest.vm.network "forwarded_port", guest: 443, host: 8445

		guest.vm.provider "virtualbox" do |vb|
			vb.name = "Virtual Machine 03"
			vb.memory = "3072"
			vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
		end

		guest.vm.provision "shell" do |sh|
			sh.path = "setup.sh"
			sh.privileged = false
		end	
	end
end
