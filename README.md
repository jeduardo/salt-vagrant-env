# Salt Vagrant Development Setup

This is a setup for developing and testing local states with Salt, heavily inspired by https://github.com/UtahDave/salt-vagrant-demo.

## Environment details

The environment setup includes one *master* and 2 *minions*, communicating using the *TCP* protocol. 
All keys are generated dynamically when the master is first provisioned. 
Salt states should be added from files/salt and will be mounted dynamically into the master. 
A similar process happens for salt pillars from files/pillar. 
A highstate will be triggered for all boxes during the Salt provisioning process through Vagrant.

## Usage

Usage is pretty simple:

```ShellSession
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-host-shell
vagrant up
```

Then a simple `vagrant ssh master` command followed by `sudo su` should give full access to the Salt master.
