# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = "api.makunouchi-vagrant-gozen.com"
  config.vm.box = "boxcutter/centos66"

  # config.vm.box_check_update = false

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", ip: "172.16.17.11"

  config.vm.synced_folder "./", "/var/work"

  # config.vm.provider "virtualbox" do |vb|
  #   vb.gui = true
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end

  config.vm.provider "virtualbox" do |v|
    v.gui = false # Display the VirtualBox GUI when booting the machine
    v.name = "api-makunouchi-vagrant-gozen"
    v.memory = 1024
    v.cpus = 2
    # v.update_guest_tools = true
  end

  # Execute Ansible by guest OS to guest OS itself.
  config.vm.provision "shell", inline: <<-SHELL
      if ! [ `which ansible` ]; then
        sudo rpm -Uvh --replacepkgs http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
        sudo yum install -y ansible
      fi
      cp -rf /var/work/provision /home/vagrant && chown -R vagrant /home/vagrant && find /home/vagrant/provision -type f | xargs chmod 644
  SHELL
end
