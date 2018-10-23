# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.ssh.forward_agent = true

  # may need to "vagrant plugin install vagrant-vbguest"
  config.vm.synced_folder "./", "/vagrant", type: "virtualbox", disabled: false

  config.vm.network "forwarded_port", guest: 27015, host: 27015
  config.vm.network "forwarded_port", guest: 27016, host: 27016
  config.vm.network "forwarded_port", guest: 27017, host: 27017
  config.vm.network "forwarded_port", guest: 27018, host: 27018
  config.vm.network "forwarded_port", guest: 27019, host: 27019
  config.vm.network "forwarded_port", guest: 27500, host: 27500

  config.vm.provider "virtualbox" do |vb|
   vb.gui = true
   vb.memory = "8192"
  end

  config.vm.provision "shell", inline: <<-SHELL
    setenforce 0
    sed -i 's/SELINUX=\(enforcing\|permissive\)/SELINUX=disabled/g' /etc/selinux/config
    yum install -y docker git nano nss curl libcurl unzip
    systemctl enable docker
    systemctl start docker
    docker run -d \
      --name stationeers \
      -p 27500:27500 \
      -p 27500:27500/udp \
      -p 27015:27015/udp \
      -v /vagrant/stationeers:/var/opt/stationeers \
      dtandersen/stationeers \
      -batchmode -nographics -autostart \
      -autosaveinterval=300 -clearallinterval=900 \
      -worldtype=Mars \
      -loadworld=mysave \
      -worldname=mysave
  SHELL
end
