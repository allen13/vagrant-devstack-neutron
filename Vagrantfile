# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  # if Vagrant.has_plugin?("vagrant-proxyconf")
  #   config.proxy.http     = "http://192.168.33.200:8123/"
  #   config.proxy.https    = "http://192.168.33.200:8123/"
  #   config.proxy.no_proxy = "localhost,127.0.0.1,192.168.1.2,192.168.2.2"
  # end
  # ubuntu precise because it's what OpenStack CI uses
  config.vm.box = "chef/ubuntu-14.04"
  config.vm.hostname = "devstack-01"

  # eth1 will be the primary interface for access to the vagrant box.
  # It's plugged into a virtualbox bridge, so its IP can be accessed
  # directly from the main host without nat port-forwarding hassle.
  # All OpenStack services will be available on this address.
  config.vm.network :private_network, ip: "192.168.1.2"

  # Add eth2 and eth3 for linuxbridge-agent interface_mappings.
  # It's necessary to specify IPs here - but they are not configured.
  config.vm.network :private_network, ip: "192.168.2.2", auto_config: false
  # config.vm.network :private_network, ip: "192.168.3.2", auto_config: false

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpus", "3"]
    v.customize ["modifyvm", :id, "--memory", "4096"]
    # enable serial port just in case vagrant image does not
    v.customize ["modifyvm", :id, "--uart1", "0x3F8", 4]
    v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook/site.yml"
  end
end
