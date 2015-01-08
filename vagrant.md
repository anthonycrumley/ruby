# Ruby Contributor Vagrant Guide

The following instructions will help you quickly get an MRI Ruby development environment ready for contributing in short order.

### Getting Started

1. Install Git: http://git-scm.com/downloads (or [GitHub for Windows](http://windows.github.com/) if you want a GUI)
2. Install VirtualBox: https://www.virtualbox.org/wiki/Downloads
3. Install Vagrant: http://www.vagrantup.com/
4. Open a terminal
5. Clone the project: `git clone https://github.com/ruby/ruby.git`
6. Enter the project directory: `cd ruby`

### Setting Up Vagrant

Build the Vagrant development environment by entering the following command:
```
vagrant up
```

The first time this command is ran will take a while because the VM image will need to download.

Follow the directions at the end of the installation to begin contributing.
