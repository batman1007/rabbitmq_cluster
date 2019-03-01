# -*- mode: ruby -*-
# vi: set ft=ruby :

# If you edit this file, use two spaces for tabs, not TAB characters

# If your PC does not support VT extensions, paste this line into the config code below:
#   v.customize ["modifyvm", :id, "--hwvirtex", "off"]

# On your host system, you need to install vagrant plugins before using this vagrantfile:
#
# If you see "Unknown configuration section 'proxy'" then
#   vagrant plugin install vagrant-proxyconf
#
# If you see "The '' provisioner could not be found" 
#   vagrant plugin install vagrant-hosts

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!



VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # config.vm.box_download_insecure = true
  config.vm.box = "geerlingguy/centos7"
  
  # Configure Nodes 2 & 3 VMs
  (2..3).each do |vmnum|   
    config.vm.define "node%02d" % vmnum do |rabbitmq_config|
	  rabbitmq_config.vm.network :private_network, ip: "10.1.172.2#{vmnum}"
 	  rabbitmq_config.vm.network :forwarded_port,guest: 22,host: 2220+vmnum,id: "ssh",host_ip: "127.0.0.1",auto_correct: false
      rabbitmq_config.vm.hostname = "node%02d.virtualbox.net" % vmnum

      # modify server vm settings and attach disks
      rabbitmq_config.vm.provider "virtualbox" do |v, override|
        v.name = "node%02d" % vmnum
        v.gui = false
        v.customize ["modifyvm", :id, "--memory", 1024]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
        v.customize ["modifyvm", :id, "--cpus", "1"]
      end
	  
	  rabbitmq_config.vm.provision :hosts do |etchosts|
        (1..3).each do |vmnum|
          etchosts.add_host "10.1.172.2#{vmnum}", ["node%02d.virtualbox.net" % vmnum, "node%02d" % vmnum]
	    end
	  end
	end
  end
  
  config.vm.define "node01" do |rabbitmq_config|
	rabbitmq_config.vm.network :private_network, ip: "10.1.172.21"
 	rabbitmq_config.vm.network :forwarded_port,guest: 22,host: 2221,id: "ssh",host_ip: "127.0.0.1",auto_correct: false
    rabbitmq_config.vm.hostname = "node01.virtualbox.net"
    rabbitmq_config.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=755,fmode=664"]
	
    # modify server vm settings and attach disks
    rabbitmq_config.vm.provider "virtualbox" do |v, override|
      v.name = "node01"
      v.gui = false
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "1"]
    end
	
	rabbitmq_config.vm.provision :hosts do |etchosts|
      (1..3).each do |vmnum|
        etchosts.add_host "10.1.172.2#{vmnum}", ["node%02d.virtualbox.net" % vmnum, "node%02d" % vmnum]
	  end
	end
	
	rabbitmq_config.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "playbook.yml"
      ansible.install_mode   = :pip
	  # ansible.verbose        = true
	  ansible.limit          = "all"
      ansible.inventory_path = "inventory"
    end
  end
end

