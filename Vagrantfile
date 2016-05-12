# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = '2'
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = 'CentOS_6.5_x86_64'
  config.vm.box_url ='https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box'
  config.vm.network :forwarded_port, guest:80, host:8080
  config.vm.network :forwarded_port, guest:443, host: 8443
  config.vm.network :forwarded_port, guest: 3000, host: 3000
  config.vm.network :private_network, ip: '192.168.33.10'

  config.vm.synced_folder './app', '/var/rails', mount_options: ['dmode=777', 'fmode=666']

  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', 1024]
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/site.yml"
    ansible.inventory_path = "hosts"
    ansible.limit = "all"
  end

end
