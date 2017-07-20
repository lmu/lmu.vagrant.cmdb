# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Special Settings for this Vagrant Environment
USE_PUBLIC_NETWORK = false
#USE_PUBLIC_NETWORK = true
PRIVATE_NETWORK_BASE = "192.168.1"
#PRIVATE_NETWORK_BASE = PRIVATE_NETWORK_BASE + ""
PUBLIC_NETWORK_BASE = "137.193.211"
#PUBLIC_NETWORK_BASE = "192.168.2"
AUTOSTART_SECONDARY = false
#AUTOSTART_SECONDARY = true

$UBUNTU_XENIAL_BOOTSTRAP_SCRIPT = <<SCRIPT
echo "Bootstrap Ubuntu 16.04 Machine"
apt install -y python aptitude
SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Every Vagrant virtual environment requires a box to build off of.
  #config.vm.box = "ubuntu/xenial64"
  config.vm.box = "centos/7"

  config.ssh.forward_agent = true
  #config.ssh.private_key_path = "~/.ssh/id_rsa"

  # Disable SharedFolder by default.
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "cmdbtest0.verwaltung.uni-muenchen.de", primary: true do |node|
    node.vm.box = "centos/7"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "CMDB Test 0"
      vb.memory = 8192
      vb.cpus = 8
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/CMDB"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".107"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".107"
    end
  end

  config.vm.define "cmdbtest1.verwaltung.uni-muenchen.de", primary: false, autostart: AUTOSTART_SECONDARY do |node|
    node.vm.box = "centos/7"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "CMDB Test 1"
      vb.memory = 8192
      vb.cpus = 8
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/CMDB"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".107"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".107"
    end
  end

  config.vm.define "cmdb1.verwaltung.uni-muenchen.de", primary: false, autostart: AUTOSTART_SECONDARY do |node|
    node.vm.box = "centos/7"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "CMDB 1"
      vb.memory = 8192
      vb.cpus = 8
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/CMDB"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".107"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".107"
    end
  end

  config.vm.define "cmdbtest2.verwaltung.uni-muenchen.de", primary: false, autostart: AUTOSTART_SECONDARY do |node|
    node.vm.box = "ubuntu/xenial64"
    node.vm.provider "virtualbox" do |vb|
      vb.name = "CMDB Test 2"
      vb.memory = 8192
      vb.cpus = 8
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/CMDB"
                   ]
    end
    config.vm.provision "bootstrap", type: "shell" do |s|
      s.inline = $UBUNTU_XENIAL_BOOTSTRAP_SCRIPT
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".84"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".84"
    end
  end

  config.vm.provision "bootstrap", type: "shell" do |s|
    s.inline = "echo Bootstrap Machine"
  end
  config.vm.provision "ssh-key", type: "shell", privileged: false do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> ~/.ssh/authorized_keys
    SHELL
  end
  config.vm.provision "base-preseed", type: "ansible" do |ansible|
    ansible.playbook = "lmu.ansible.playbooks/base-preseed.yml"
    #ansible.verbose = "vvv"
  end
  config.vm.provision "application", type: "ansible" do |ansible| # run: "never"
    ansible.playbook = "lmu.ansible.playbooks/cmdb.yml"
    ansible.groups = {
      "cmdb" => ["cmdbtest0.verwaltung.uni-muenchen.de",
                 "cmdbtest1.verwaltung.uni-muenchen.de",
                 "cmdbtest3.verwaltung.uni-muenchen.de",
                 "cmdb1.verwaltung.uni-muenchen.de"],
      }
      #ansible.verbose = "vvvv"
      ansible.verbose = "vvv"
      #ansible.verbose = "vv"
      #ansible.verbose = "v"
      #ansible.verbose = ""
      #ansible.limit = "all"
      #ansible.tags = ["setup", "configuration", "update"]
      #ansible.skip_tags = ["update"]
      ansible.extra_vars = {
        ansible_connection: 'ssh',
        ansible_ssh_args: '-o ForwardAgent=yes',
        ansible_ssh_private_key_file: ['~/.ssh/id_rsa']
      }
  end

end
