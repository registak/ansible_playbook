# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version '>= 1.5'

def require_plugins(plugins = {})
  needs_restart = false
  plugins.each do |plugin, version|
    next if Vagrant.has_plugin?(plugin)
    cmd = ['vagrant plugin install', plugin]
    cmd << "--plugin-version #{version}" if version
    system(cmd.join(' ')) || exit!
    needs_restart = true
  end
  exit system('vagrant', *ARGV) if needs_restart
end

require_plugins \
  'vagrant-bindfs' => '0.3.2',
  'vagrant-hostsupdater' => '1.0.2'

VAGRANTFILE_API_VERSION = '2'
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', 1024, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.define 'myapp' do |machine|
    machine.vm.hostname = 'localhost'

    machine.vm.box = 'CentOS_6.5_x86_64'
    machine.vm.box_url ='https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box'

    machine.vm.network 'forwarded_port', :guest => 80, :host => 8080, :auto_correct => true
    machine.vm.network 'forwarded_port', :guest => 443, :host => 8081, :auto_correct => true
    machine.vm.network :private_network, ip: '192.168.33.10'

    # machine.vm.synced_folder '../../', '/myapp', :type => 'nfs'
    # machine.vm.synced_folder '../ansible', '/ansible', :type => 'nfs'
    # machine.bindfs.bind_folder '//myapp', '//myapp'
    # machine.bindfs.bind_folder '/ansible', '/ansible'
  end

  config.ssh.forward_agent = true

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/site.yml"
    ansible.sudo = true
    ansible.groups = {
      'application' => %w(myapp),
      'vm' => %w(myapp),
      'mysql' => %w(myapp),
      'development:children' => %w(application vm mysql),
    }
    ansible.limit = "all"
  end

end
