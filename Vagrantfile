# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "dmotte/ansiblebox"

    config.vm.hostname = "vagrantbox"

    config.vm.provider "virtualbox" do |vb|
        # Enable nested virtualization (Nested VT-x/AMD-V)
        vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.galaxy_role_file = "requirements.yml"
        ansible.playbook = "playbook.yml"
        ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    end

    config.ssh.insert_key = false # This is usually recommended for base boxes
end
