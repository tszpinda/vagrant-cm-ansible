# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  OS_IMAGE="bento/centos-7.2"

  # create mgmt node
  config.vm.define :mgmt, primary: true do |mgmt_config|
      mgmt_config.vm.box = OS_IMAGE
      mgmt_config.vm.hostname = "mgmt"
      mgmt_config.vm.network :private_network, ip: "10.0.15.10"
      mgmt_config.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
      mgmt_config.vm.provision "shell", path: "bootstrap-mgmt.sh"
      mgmt_config.ssh.shell = "/bin/sh"
      mgmt_config.ssh.username= "vagrant"
      mgmt_config.ssh.password = "vagrant"
  end

  # create load balancer
  config.vm.define :lb do |lb_config|
      lb_config.vm.box = OS_IMAGE
      lb_config.vm.hostname = "lb"
      lb_config.vm.network :private_network, ip: "10.0.15.11"
      lb_config.vm.network "forwarded_port", guest: 80, host: 8080
      lb_config.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
      lb_config.ssh.username= "vagrant"
      lb_config.ssh.password = "vagrant"
  end

  # https://docs.vagrantup.com/v2/vagrantfile/tips.html
  (1..2).each do |i|
    config.vm.define "web#{i}" do |node|
        node.vm.box = OS_IMAGE
        node.vm.hostname = "web#{i}"
        node.vm.network :private_network, ip: "10.0.15.2#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
        node.ssh.username= "vagrant"
        node.ssh.password = "vagrant"
    end
  end

end
