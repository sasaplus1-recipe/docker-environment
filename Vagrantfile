# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
  end

  config.vm.synced_folder './share', '/home/vagrant/share', create: true

  config.vm.provision 'shell', privileged: false, inline: <<-SHELL
    # change to use JAIST mirror server
    if grep '//ftp.jaist.ac.jp/pub/Linux' /etc/apt/sources.list
    then
      echo 'already used JAIST mirror server'
    else
      sudo sed -i.bak -e 's|//archive.ubuntu.com|//ftp.jaist.ac.jp/pub/Linux|' /etc/apt/sources.list
    fi

    # update
    sudo apt-get update
    sudo apt-get --yes upgrade

    # install dependencies for Docker
    sudo apt-get --yes install apt-transport-https ca-certificates

    # add GPG key
    sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

    # add apt source
    sudo sh -c "echo 'deb https://apt.dockerproject.org/repo ubuntu-trusty main' > /etc/apt/sources.list.d/docker.list"

    # update
    sudo apt-get update
    sudo apt-get --yes upgrade

    # install dependencies for Docker
    sudo apt-get install linux-image-extra-$(uname -r) apparmor

    # install Docker
    sudo apt-get update
    sudo apt-get --yes install docker-engine

    # start the Docker daemon
    sudo service docker start

    # verify
    sudo docker run hello-world

    # add docker group
    sudo groupadd docker

    # add to docker group
    sudo usermod -aG docker vagrant

    # verify without sudo
    docker run hello-world
  SHELL
end
