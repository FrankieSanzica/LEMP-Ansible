# -*- mode: ruby -*-
# vi: set ft=ruby ts=2:

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Set the default Image
  # NOTE: If you want to default to Windows, switch base box images
  config.vm.box = "puppetlabs/centos-7.2-64-nocm"
  config.ssh.insert_key = false

  # Set default virtualbox configurations
  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  #############################################################################
  #                                                                           #
  #                             Linux Section                                 #
  #                                                                           #
  #############################################################################
  # NOTE: If you need a linux host, uncomment the section below.

   config.vm.define :lemptestbox do |host|
     host.vm.hostname = "lemptestbox"
     # NOTE: You can override the boxtype here
     #host.vm.box = "puppetlabs/centos-7.2-64-nocm"
     host.vm.network :private_network, ip: "192.168.31.22"
     host.vm.network "forwarded_port", guest: 80, host: 8080
     host.vm.network "forwarded_port", guest: 443, host: 8443
     host.vm.provider :virtualbox do |v|
       v.customize ["modifyvm", :id, "--name", host.vm.hostname]
     end
   end

  # Ansible provisioner
  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
        "LEMP" => ["lemptestbox"]
    }
    ansible.playbook = "playbook.yml"
    ansible.galaxy_role_file = "roles/requirements.yml"
    ansible.galaxy_command = "ansible-galaxy install --role-file=%{role_file} --roles-path=roles/ --ignore-errors"
    ansible.extra_vars = { ansible_winrm_scheme: "http" }
  end

end
