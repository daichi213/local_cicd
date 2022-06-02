# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "base"

  config.vm.define :git do |node|
    node.vm.box = "generic/ubuntu2110"
    node.vm.network :forwarded_port, guest: 22, host: 2202, id: "ssh"
    node.vm.network :forwarded_port, guest: 8080, host: 8002, id: "http"
    node.vm.network :private_network, ip: "192.168.33.12"
    node.vm.synced_folder "./gitbucket_home/service", "/etc/systemd/system"

    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y default-jdk
      apt-get install openjdk-7-jre
      cd /usr/lib
      wget -P /usr/lib https://github.com/gitbucket/gitbucket/releases/download/4.37.2/gitbucket.war
      systemctl start gitbucket_daemonized.service
    SHELL
  end

  config.vm.define :jenkins do |node|
    node.vm.box = "generic/ubuntu2110"
    node.vm.network :forwarded_port, guest: 22, host: 2203, id: "ssh"
    node.vm.network :forwarded_port, guest: 8080, host: 8003, id: "http"
    node.vm.network :private_network, ip: "192.168.33.13"

    node.vm.provision "shell", inline: <<-SHELL
      wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add - 
      sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
      apt-get update
      apt-get install -y default-jdk
      apt-get install -y jenkins
    SHELL
    node.vm.synced_folder "./jenkins_home", "/var/lib/jenkins"
  end

  config.vm.define :local do |node|
    node.vm.box = "generic/ubuntu2110"
    node.vm.network :forwarded_port, guest: 22, host: 2204, id: "ssh"
    node.vm.network :forwarded_port, guest: 80, host: 8004, id: "http"
    node.vm.network :private_network, ip: "192.168.33.14"
  end

  config.vm.define :development do |node|
    node.vm.box = "generic/ubuntu2110"
    node.vm.network :forwarded_port, guest: 22, host: 2205, id: "ssh"
    node.vm.network :forwarded_port, guest: 80, host: 8005, id: "http"
    node.vm.network :private_network, ip: "192.168.33.15"
  end

  config.vm.define :production do |node|
    node.vm.box = "generic/ubuntu2110"
    node.vm.network :forwarded_port, guest: 22, host: 2206, id: "ssh"
    node.vm.network :forwarded_port, guest: 80, host: 8006, id: "http"
    node.vm.network :private_network, ip: "192.168.33.16"
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
end
