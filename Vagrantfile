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
        # From: https://github.com/dmotte/misc/blob/main/scripts/fetch-and-check.sh
        fetch_and_check() {
            local c s; c=$(curl -fsSL "$1"; echo x) && \
            s=$(echo -n "${c%x}" | sha256sum | cut -d' ' -f1) && \
            if [ "$s" = "$2" ]; then echo -n "${c%x}"
            else echo "Checksum verification failed for $1: got $s, expected $2" >&2
            return 1; fi
        }

        codename=$(grep VERSION_CODENAME /etc/os-release | cut -d= -f2)

        apt-get update; apt-get install -y curl gnupg linux-headers-generic

        fetch_and_check \
            'https://www.virtualbox.org/download/oracle_vbox_2016.asc' \
            '49e6801d45f6536232c11be6cdb43fa8e0198538d29d1075a7e10165e1fbafe2' | \
            gpg --dearmor -o /etc/apt/trusted.gpg.d/virtualbox.gpg
        echo 'deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/virtualbox.gpg]' \
            "https://download.virtualbox.org/virtualbox/debian $codename contrib" | \
            tee /etc/apt/sources.list.d/virtualbox.list
        apt-get update; apt-get install -y virtualbox-7.0
        # Note: a reboot is required before being able to use VirtualBox

        # Ref: https://developer.hashicorp.com/vagrant/install#Linux
        fetch_and_check \
            'https://apt.releases.hashicorp.com/gpg' \
            'cafb01beac341bf2a9ba89793e6dd2468110291adfbb6c62ed11a0cde6c09029' | \
            gpg --dearmor -o /etc/apt/trusted.gpg.d/hashicorp.gpg
        echo 'deb [signed-by=/etc/apt/trusted.gpg.d/hashicorp.gpg]' \
            "https://apt.releases.hashicorp.com $codename main" | \
            tee /etc/apt/sources.list.d/hashicorp.list
        apt-get update; apt-get install -y vagrant
    SHELL

    config.ssh.insert_key = false # This is usually recommended for base boxes
end
