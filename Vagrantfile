# -*- mode: ruby -*-
# vi: set ft=ruby :

# プロビジョニングスクリプト
$configureBox = <<-SHELL

  # パッケージ更新
  apt-get update
  apt-get upgrade -y

  # Dockerの前提パッケージ
  apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
  # Dockerのレポジトリ追加
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
  add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  # Dockerのインストール
  apt-get update
  apt-get install -y docker-ce=$(apt-cache madison docker-ce | grep 18.03.1 | head -1 | awk '{print $3}')
  apt-mark hold docker-ce

  # vagrantユーザーをdockerグループに追加
  usermod -aG docker vagrant

  # 有効化と再起動
  systemctl enable docker
  systemctl restart docker

SHELL

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.network "private_network", ip: "192.168.33.11"

  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end

  config.vm.synced_folder ".", "/vagrant", type:"virtualbox"

  # プロビジョニング実行
  config.vm.provision "shell", inline: $configureBox

end
