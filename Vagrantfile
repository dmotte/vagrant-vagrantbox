# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "dmotte/ansiblebox"

    config.vm.hostname = "vagrantbox"

    config.vm.provider "virtualbox" do |vb|
        # Enable nested virtualization (Nested VT-x/AMD-V)
        vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end

    config.vm.provision "shell", inline: <<-SHELL
        fetch_and_check() { # Src: https://github.com/dmotte/misc
            local c s; c=$(curl -fsSL "$1"; echo x) &&
            s=$(echo -n "${c%x}" | sha256sum | cut -d' ' -f1) &&
            if [ "$s" = "$2" ]; then echo -n "${c%x}"
            else echo "Checksum verification failed for $1: got $s, expected $2" >&2
            return 1; fi
        }

        apt-get update; apt-get install -y curl

        script_apt_virtualbox=$(fetch_and_check \
            https://raw.githubusercontent.com/dmotte/misc/main/scripts/provisioning/apt-virtualbox.sh \
            992bf88ef4904005d083e67af36ddda4b6a853571510ec415fb3c98d8171cc22)
        script_apt_vagrant=$(fetch_and_check \
            https://raw.githubusercontent.com/dmotte/misc/main/scripts/provisioning/apt-vagrant.sh \
            8d0d416a77bfe764c15cbce9d39ca53b9c14054cfc3a3083f31782750c491051)

        # Note that a reboot is required before being able to use VirtualBox
        echo "$script_apt_virtualbox" | bash -s -- 7.0
        echo "$script_apt_vagrant" | bash -s
    SHELL

    config.ssh.insert_key = false # This is usually recommended for base boxes
end
