# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box_check_update = false

  config.vm.define "devops" do |devops|
    devops.vm.hostname = "devops.dexter.com.br"
    devops.vm.box = "debian/stretch64"
    devops.vm.network "private_network", ip: "192.168.10.10"
    devops.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = "2"
    end
  devops.vm.provision "shell", inline: <<-SHELL
    echo 'deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main' > /etc/apt/sources.list.d/ansible.list
    apt-get install -y dirmngr
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
    wget https://apt.puppetlabs.com/puppet6-release-xenial.deb
    dpkg -i puppet6-release-xenial.deb
    apt-get update
    apt-get install -y puppet-agent
    apt-get install -y ansible vim
    HOSTS=$(head -n7 /etc/hosts)
    echo -e "$HOSTS" > /etc/hosts
    echo '192.168.10.10 devops.dexter.com.br' >> /etc/hosts
    echo '192.168.10.20 docker.dexter.com.br' >> /etc/hosts
    echo '192.168.10.30 automation.dexter.com.br' >> /etc/hosts
    echo '192.168.10.40 minion.dexter.com.br' >> /etc/hosts
    cp /vagrant/files/inventory /etc/ansible/hosts
    cp /vagrant/files/ansible.cfg /etc/ansible/ansible.cfg
   SHELL
  end

 config.vm.define "docker" do |docker|
    docker.vm.hostname = "docker.dexter.com.br"
    docker.vm.box = "opensuse/openSUSE-15.0-x86_64"
    docker.vm.network "private_network", ip: "192.168.10.20"
    docker.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = "2"
    end
 docker.vm.provision "shell", inline:<<-SHELL
     zypper install -y vim docker docker-compose
     systemctl enable docker
     systemctl start docker
     systemctl stop firewalld
     systemctl disable firewalld
    SHELL
  end

 config.vm.define "automation" do |automation|
    automation.vm.hostname = "automation.dexter.com.br"
    automation.vm.box = "centos/7"
    automation.vm.network "private_network", ip: "192.168.10.30"
    automation.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = "2"
    end
    automation.vm.provision "shell", inline:<<-SHELL
      yum install -y vim
      localectl set-locale LANG=en_US.utf8
      yum reinstall -y glibc-common
    SHELL
  end

config.vm.define "minion" do |minion|
    minion.vm.hostname = "minion.dexter.com.br"
    minion.vm.box = "debian/stretch64"
    minion.vm.network "private_network", ip: "192.168.10.40"
    minion.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
    end
  end

config.vm.provision "shell", inline:<<-SHELL
     cat /vagrant/files/key.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL

end

