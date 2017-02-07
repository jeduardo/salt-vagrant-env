# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'vagrant-host-shell'

Vagrant.configure("2") do |config|
  # Configure salt master
  config.vm.define "master", autostart: true do |master|
    # Seed all files locally before bringing master and minions up
    master.vm.provision :host_shell do |host_shell|
      host_shell.inline = 'salt-key -u $USER --gen-keys-dir=files/keys/ --gen-keys=master &&
                        salt-key -u $USER --gen-keys-dir=files/keys/ --gen-keys=minion1 &&
                        salt-key -u $USER --gen-keys-dir=files/keys/ --gen-keys=minion2'
    end
  
    master.vm.box = "debian/jessie64"
    master.vm.host_name = "master"

    # Link local salt files into master
    master.vm.synced_folder "files/salt/", "/srv/salt"
    master.vm.synced_folder "files/pillar/", "/srv/pillar"
    master.vm.network "private_network", ip: "192.168.200.10"

    master.vm.provision :salt do |salt|
      salt.master_config = "files/etc/master"
      salt.minion_config = "files/etc/minion"
      salt.master_key = "files/keys/master.pem"
      salt.master_pub = "files/keys/master.pub"
      salt.minion_key = "files/keys/master.pem"
      salt.minion_pub = "files/keys/master.pub"
      salt.seed_master = {
        "master" => "files/keys/master.pub",
        "minion1" => "files/keys/minion1.pub",
        "minion2" => "files/keys/minion2.pub"
      }

      salt.install_master = true
      salt.install_type = "stable"
      salt.no_minion = false
      salt.verbose = true
      salt.colorize = true
      salt.run_highstate = true
    end
  end

  # Configure minion1
  config.vm.define "minion1", autostart: true do |minion1|
    minion1.vm.box = "debian/jessie64"
    minion1.vm.host_name = "minion1"
    minion1.vm.network "private_network", ip: "192.168.200.11"

    minion1.vm.provision :salt do |salt|
      salt.minion_config = "files/etc/minion"
      salt.minion_key = "files/keys/minion1.pem"
      salt.minion_pub = "files/keys/minion1.pub"

      salt.install_type = "stable"
      salt.verbose = true
      salt.colorize = true

      salt.run_highstate = true
    end
  end

  # Configure minion2
  config.vm.define "minion2", autostart: true do |minion2|
    minion2.vm.box = "debian/jessie64"
    minion2.vm.host_name = "minion2"
    minion2.vm.network "private_network", ip: "192.168.200.12"

    minion2.vm.provision :salt do |salt|
      salt.minion_config = "files/etc/minion"
      salt.minion_key = "files/keys/minion2.pem"
      salt.minion_pub = "files/keys/minion2.pub"

      salt.install_type = "stable"
      salt.verbose = true
      salt.colorize = true

      salt.run_highstate = true
    end
  end

end
