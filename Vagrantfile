# -*- mode: ruby -*-
# vi: set ft=ruby :
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

$gateway = <<-SCRIPT
ifconfig eth1 up
ifconfig eth1 192.168.10.1
sudo su -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
SCRIPT

# This isolates hostA from the outside, except for ssh from the host
# Exercise consists in configure "hostA" to use "gateway" as a gateway
$hostA = <<-SCRIPT
ifconfig eth1 up
ifconfig eth1 192.168.10.2
route del default dev eth0
route add default gw 192.168.10.1 dev eth1
SCRIPT

Vagrant.configure("2") do |config|
	config.vm.define "gateway" do |host|
		host.vm.hostname = "gateway"
		host.vm.box = "ubuntu/trusty64"
		host.ssh.username = "vagrant"

		host.vm.provision "shell", inline: $gateway

		host.vm.provider :virtualbox do |vb|
			vb.customize ["modifyvm", :id, "--nic2", "intnet"]
		end
	end

	config.vm.define "hostA" do |host|
		host.vm.hostname = "hostA"
		host.vm.box = "ubuntu/trusty64"
		host.ssh.username = "vagrant"
		# host.ssh.host = "192.168.1.2"

		host.vm.provision "shell", inline: $hostA

		host.vm.provider :virtualbox do |vb|
			vb.customize ["modifyvm", :id, "--nic2", "intnet"]
		end
	end
end
