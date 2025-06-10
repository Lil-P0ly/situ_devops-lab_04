# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.ssh.insert_key = false
  config.vm.box = "ubuntu_bento-24.04"

  # Общий провижининг для всех машин
  config.vm.provision "shell", env: {
    "RUNNER_PUBKEY" => File.read(File.expand_path("~/.ssh/runner_vagrant_key.pub")).strip
  }, inline: <<-SHELL
  set -eux

  # Создание пользователя runner
  groupadd -f runner
  useradd -m -g runner -G sudo -s /bin/bash runner
  echo 'runner:superpassword' | chpasswd

  # SSH-доступ по ключу
  mkdir -p /home/runner/.ssh
  echo "$RUNNER_PUBKEY" > /home/runner/.ssh/authorized_keys
  chmod 700 /home/runner/.ssh
  chmod 600 /home/runner/.ssh/authorized_keys
  chown -R runner:runner /home/runner/.ssh

  echo 'runner ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/runner
  chmod 440 /etc/sudoers.d/runner

  SHELL



  config.vm.define "ubuntu_slave1" do |ubuntu_slave1|
    ubuntu_slave1.vm.hostname =  "ubuntu-slave1"
    ubuntu_slave1.vm.network "forwarded_port", guest: 80, host: 8081
  end

  config.vm.define "ubuntu_slave2" do |ubuntu_slave2|
    ubuntu_slave2.vm.hostname = "ubuntu-slave2"
    ubuntu_slave2.vm.network "forwarded_port", guest: 80, host: 8082

  end

end