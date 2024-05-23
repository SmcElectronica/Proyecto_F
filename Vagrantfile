# -- mode: ruby --
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

	if Vagrant.has_plugin? "vagrant-vbguest"
		config.vbguest.no_install = true
		config.vbguest.auto_update = false
		config.vbguest.no_remote = true
	end

	config.vm.define :clientnagios1 do |clientenagios|
		clientenagios.vm.box = "centos/7"
		clientenagios.vm.network :private_network, ip: "192.168.50.2"
		clientenagios.vm.hostname = "clientenagios"
	end

	config.vm.define :clientnagios2 do |clientenagios2|
		clientenagios2.vm.box = "centos/7"
		clientenagios2.vm.network :private_network, ip: "192.168.50.4"
		clientenagios2.vm.hostname = "clientenagios2"
	end

	config.vm.define :servernagios do |servidornagios|
		servidornagios.vm.box = "centos/7"
		servidornagios.vm.network :private_network, ip: "192.168.50.3"
		servidornagios.vm.hostname = "servidornagios"
		servidornagios.vm.network :forwarded_port, guest: 80, host: 8090
		servidornagios.vm.network :forwarded_port, guest: 443, host: 5568
	end
end
