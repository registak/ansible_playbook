# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VESION = "2"
Vagrant.configure(VAGRANTFILE_API_VESION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "forwarded_port", guest:80, host:8080
  config.vm.network "forwarded_port", guest:443, host: 8443
end
