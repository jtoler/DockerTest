# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?("vagrant-docker-compose")
  system("vagrant plugin install vagrant-docker-compose")
  puts "Dependencies installed, please try the command again."
  exit
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

hostname = 'local.dev'
memory = 4096
cpus = 2

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  # Networking
  config.vm.hostname = hostname
  config.vm.network :private_network,
    # https://github.com/oscar-stack/vagrant-auto_network
    :auto_network => true  
  config.vm.network(:forwarded_port, guest: 80, host: 8080)
  #config.vm.network(:forwarded_port, guest: 443, host: 4433)
  #config.vm.network(:forwarded_port, guest: 8080, host: 1234)
  #config.vm.network(:forwarded_port, guest: 3306, host: 12345)

  # Docker Provisions
  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", run: "always"

   # Synced folders.
  opts = {
    create: true,
    type: 'rsync',
    rsync__exclude: ['.idea/', '.git/'],
    rsync__args: ['--verbose', '--archive', '--delete', '-z']
  }

  # Disable default synced folder.
  #config.vm.synced_folder '.', '/vagrant', disabled: true
  # Project/site files.
  #config.vm.synced_folder './code/', '/opt/code', opts
  # Docker files.
  #config.vm.synced_folder './provision/', '/opt/provision', opts

  # VirtualBox provider configuration.
  config.vm.provider :virtualbox do |v|
    v.customize ['modifyvm', :id, '--memory', memory]
    v.cpus = cpus
    v.name = hostname
  end
  # VMware Fusion provider configuration.
  config.vm.provider :vmware_fusion do |v|
    v.vmx['memsize'] = memory
    v.vmx['numvcpus'] = cpus
    v.vmx['displayName'] = hostname
  end
  
  #config.vm.synced_folder "/Users/Justin/Documents/GitProjects/", "/"

  # plugin conflict
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end
  
end
