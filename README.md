# vagrant-vagrantbox

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/dmotte/vagrant-vagrantbox/release.yml?branch=main&logo=github&style=flat-square)](https://github.com/dmotte/vagrant-vagrantbox/actions)
[![Vagrant Cloud](https://img.shields.io/badge/vagrant-dmotte/vagrantbox-blue?logo=vagrant&style=flat-square)](https://app.vagrantup.com/dmotte/boxes/vagrantbox)

:package: **Debian Vagrant box** with **Vagrant** and **VirtualBox** for nested virtualization.

This project uses:

- [dmotte/vagrant-ansiblebox](https://github.com/dmotte/vagrant-ansiblebox) as base box.
- [Oefenweb/ansible-virtualbox](https://github.com/Oefenweb/ansible-virtualbox) to install _VirtualBox_ inside the VM.
- [dmotte/ansible-role-vagrant](https://github.com/dmotte/ansible-role-vagrant) to install _Vagrant_ inside the VM.

> :package: This box is also on **Vagrant Cloud** as [`dmotte/vagrantbox`](https://app.vagrantup.com/dmotte/boxes/vagrantbox).

## Usage

To use this Vagrant box you can take inspiration from [the `test` folder of dmotte/vagrant-dockerbox](https://github.com/dmotte/vagrant-dockerbox/tree/main/test).

Remember to give your VM a decent amount of **RAM** and **CPU**, because it has to run other VMs inside.

```ruby
config.vm.provider "virtualbox" do |vb|
    vb.memory = 8192
    vb.cpus = 8
end
```

If you want to be able to access the **VirtualBox GUI**, you must set the Vagrant **X11 forwarding** flag:

```ruby
config.ssh.forward_x11 = true
```

and also install the `xauth` package inside the VM:

```ruby
config.vm.provision "shell", inline: <<-SHELL
    apt-get update && apt-get install -y xauth
SHELL
```
