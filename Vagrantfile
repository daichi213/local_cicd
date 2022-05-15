# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("4") do |config|

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "base"
  
  config.vm.define :node1 do |node|
    node.vm.box = "generic/ubuntu2204"
    node.vm.network :forwarded_port, guest: 22, host: 2201, id: "ssh"
    node.vm.network :private_network, ip: "192.168.33.11"

    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ansible
    SHELL
  end

  config.vm.define :node2 do |node|
    node.vm.box = "generic/ubuntu2204"
    node.vm.network :forwarded_port, guest: 22, host: 2202, id: "ssh"
    node.vm.network :forwarded_port, guest: 8080, host: 8002, id: "http"
    node.vm.network :private_network, ip: "192.168.33.12"

    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y default-jdk
      apt-get install openjdk-7-jre
      wget http://ftp.meisei-u.ac.jp/mirror/apache/dist/tomcat/tomcat-10/v10.0.20/bin/apache-tomcat-10.0.20.tar.gz
      tar zxvf apache-tomcat-10.0.20.tar.gz
      wget https://github.com/gitbucket/gitbucket/releases/download/4.37.2/gitbucket.war
      mv gitbucket.war apache-tomcat-10.0.20/webapps/
    SHELL
  end

  config.vm.define :node3 do |node|
    node.vm.box = "generic/ubuntu2204"
    node.vm.network :forwarded_port, guest: 22, host: 2203, id: "ssh"
    node.vm.network :forwarded_port, guest: 8080, host: 8003, id: "http"
    node.vm.network :private_network, ip: "192.168.33.13"

    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y default-jdk
      apt-get install -y jenkins
    SHELL
    node.vm.synced_folder "./jenkins_home", "/var/jenkins_home"
  end

  config.vm.define :node4 do |node|
    node.vm.box = "generic/ubuntu2204"
    node.vm.network :forwarded_port, guest: 22, host: 2204, id: "ssh"
    node.vm.network :forwarded_port, guest: 80, host: 8004, id: "http"
    node.vm.network :private_network, ip: "192.168.33.14"

    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y jenkins
    SHELL
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
