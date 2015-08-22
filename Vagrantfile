# -*- mode: ruby -*-
# vi: set ft=ruby :
ROOT = File.dirname(File.expand_path(__FILE__))

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

# Default env properties which can be overridden
# Example overrides:
#   echo "ENV['BASEIMAGE_PATH'] ||= '../../phusion/baseimage-docker'   " >> ~/.vagrant.d/Vagrantfile
#   echo "ENV['BASE_BOX_URL']   ||= 'd\:/dev/vm/vagrant/boxes/phusion/'" >> ~/.vagrant.d/Vagrantfile
BASE_BOX_URL          = ENV['BASE_BOX_URL']          || 'https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/'
VAGRANT_BOX_URL       = ENV['VAGRANT_BOX_URL']       || BASE_BOX_URL + 'ubuntu-14.04-amd64-vbox.box'

$script = <<SCRIPT
set -ex


apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.0.list

curl -sL https://deb.nodesource.com/setup | sudo bash -
sudo apt-get install build-essential

apt-get update -qq
sudo apt-get install -y --no-install-recommends nodejs

set -x \
&& apt-get install -y --no-install-recommends mongodb-org \
&& sudo rm -rf /var/lib/apt/lists/* \
&& sudo rm -rf /var/lib/mongodb \
&& sudo mv /etc/mongod.conf /etc/mongod.conf.orig

curl -sL https://install.meteor.com/ | sh

sudo npm install -g nodemon

service mongod status

SCRIPT

$command = "cp #{File.join('/etc/', '')} #{remote_file}"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'phusion-open-ubuntu-14.04-amd64'
  config.vm.box_url = VAGRANT_BOX_URL
  config.ssh.forward_agent = true
  config.vm.network "private_network", type: "dhcp"

  config.vm.provision :shell, :inline => $script
end

# plugin conflict
if Vagrant.has_plugin?("vagrant-vbguest") then
  config.vbguest.auto_update = false
end
