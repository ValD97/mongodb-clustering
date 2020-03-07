# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "ansible-master" do |node|
    node.vm.network "private_network", ip: "10.0.0.100"
  end


  (1..8).each do |i|
    config.vm.define "mongo-#{i}" do |node|
      node.vm.network "private_network", ip: "10.0.0.1#{i}"
    end
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end

  # SHELL
  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("./id_rsa.pub").first.strip
    ssh_priv_key = File.read("./id_rsa")
    s.inline = <<-SHELL
      mkdir -p /root/.ssh
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
      echo "#{ssh_priv_key}" > /root/.ssh/id_rsa
      chmod 600 /root/.ssh/id_rsa
      echo "#{ssh_priv_key}" > /home/vagrant/.ssh/id_rsa
      chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
      chmod 600 /home/vagrant/.ssh/id_rsa
      apt install -y python
    SHELL
  end
end
