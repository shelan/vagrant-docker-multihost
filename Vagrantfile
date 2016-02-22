# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/wily64"

  # Machine-specific provider-specific config (names)
  config.vm.define "vm1" do |machineconfig|
    machineconfig.vm.provider "virtualbox" do |vb|
      vb.name = "vm1"
    end
    machineconfig.vm.network "private_network", ip: "172.28.128.5"
  end

  config.vm.define "vm2" do |machineconfig|
    machineconfig.vm.provider "virtualbox" do |vb|
      vb.name = "vm2"
    end
    machineconfig.vm.network "private_network", ip: "172.28.128.6"
  end


  # Common provider-specific config
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
    vb.cpus = 1
    vb.customize ["modifyvm", :id, "--vram", "8"]
  end

  config.ssh.insert_key = false

  config.vm.provision :shell, path: "bootstrap.sh"

  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    sudo sh -c 'echo "deb https://apt.dockerproject.org/repo ubuntu-wily main" > /etc/apt/sources.list.d/docker.list'
    sudo apt-get update
    sudo apt-get -y install linux-image-extra-$(uname -r)
    sudo apt-get -y install docker-engine
    usermod -a -G docker vagrant   ## add vagrant user to docker group
    sudo mkdir -p /etc/systemd/system/docker.service.d/ 
    sudo sh -c 'cat /vagrant/docker.conf >> /etc/systemd/system/docker.service.d/docker.conf'
    sudo systemctl daemon-reload
    sudo systemctl restart docker
    sudo systemctl status docker
  SHELL
end