# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  if Vagrant.has_plugin?("vagrant-cachier")
    # Configure cached packages to be shared between instances of the same base box.
    # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
    config.cache.scope = :box
  end

  config.vm.provider "virtualbox" do |v|
    #v.gui = true
    v.memory = 2048
    v.cpus = 4
  end

  # Create a forwarded port mapping which allows access to a specific port
  config.vm.network "forwarded_port", guest: 80, host: 8080     # Web Server
  config.vm.network "forwarded_port", guest: 9000, host: 9000   # phpMyAdmin

  # Use a public network so other devices on the network can access it
  config.vm.network "public_network"
  #config.vm.network "private_network", ip: "192.168.50.100"

  # Enable provisioning with Puppet
  config.vm.provision "puppet" do |puppet|
    puppet.manifests_path = "vagrant/puppet/manifests"
    puppet.module_path = 'vagrant/puppet/modules'
    puppet.manifest_file  = "init.pp"
    puppet.options = ['--verbose']
  end

  # Shared folders

  ### Using the default share approach

  # This shares the folder and sets very liberal permissions
  config.vm.synced_folder ".", "/vagrant",
    :owner => 'www-data',
    :group => 'www-data',
    :mount_options => ['dmode=777,fmode=777']

  # This disables the runtime folder from being synced which makes vagrant 2-3 times faster
  config.vm.synced_folder "craft/storage/runtime", "/vagrant/craft/storage/runtime", disabled: true

  ### Using rsync (which is normally faster but may require some additional set-up)

  # You have to run "vagrant rsync-auto" to start it syncing
  #config.vm.synced_folder ".", "/vagrant",
  #  type: "rsync",
  #  rsync__auto: "true",
  #  rsync__exclude: [".git/", "craft/storage/runtime/", "_build/.sass-cache/"],
  #  id: "vagrant-root"

end