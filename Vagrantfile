# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant configuration for an MRI Ruby contributor environment.
Vagrant.configure(2) do |config|
  config.vm.box = "hashicorp/precise64"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Maps the /vagrant VM folder to the Ruby source folder.  NFS is used for 
  # better performance and may require configuration on the host machine.
  config.vm.synced_folder '.', '/vagrant', nfs: true

  # Configure the virtual machine with enough access to processors and memory
  # that it will not be sluggish.
  config.vm.provider "virtualbox" do |v|
    host = RbConfig::CONFIG['host_os']

    # Give VM 1/4 system memory & access to all cpu cores on the host
    if host =~ /darwin/
      cpus = `sysctl -n hw.ncpu`.to_i
      # sysctl returns Bytes and we need to convert to MB
      mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
    elsif host =~ /linux/
      cpus = `nproc`.to_i
      # meminfo shows KB and we need to convert to MB
      mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
    else # sorry Windows folks, I can't help you
      cpus = 2
      mem = 1024
    end

    v.customize ["modifyvm", :id, "--memory", mem]
    v.customize ["modifyvm", :id, "--cpus", cpus]
  end

  # Provision the Ruby development environment with a shell script.
  config.vm.provision "shell", inline: <<-SHELL
    # The following allows openssl to be upgraded unattended.
    unset UCF_FORCE_CONFFOLD
    export UCF_FORCE_CONFFNEW=YES
    ucf --purge /boot/grub/menu.lst
    export DEBIAN_FRONTEND=noninteractive

    apt-get update
    # Upgrade openssl unattended.
    apt-get -o Dpkg::Options::="--force-confnew" --force-yes -fuy upgrade openssl
    apt-get install -y git
    apt-get install -y autoconf
    apt-get install -y make
    apt-get install -y bison
    aptitude build-dep -y ruby1.9.1
    su - vagrant -c "mkdir ~/build"
    su - vagrant -c "cd /vagrant && autoconf"
    su - vagrant -c "cd ~/build && /vagrant/configure"
  SHELL

  # Usage instructions that will be displayed whenever 'vagrant up' completes.
  config.vm.post_up_message = <<-USAGE_INSTRUCTIONS
  To access the MRI Ruby development virtual machine:

    $ vagrant ssh
    vagrant@precise64:~$

  To build ruby:

    vagrant@precise64:~$ cd ~/build
    vagrant@precise64:~$ make

  To run MRI tests:

    vagrant@precise64:~$ cd ~/build
    vagrant@precise64:~$ make test-all

  To update RubySpec tests:

    vagrant@precise64:~$ cd ~/build
    vagrant@precise64:~$ make update-rubyspec
     
  To run RubySpec tests:

    vagrant@precise64:~$ cd ~/build
    vagrant@precise64:~$ make test-rubyspec

  USAGE_INSTRUCTIONS
end
