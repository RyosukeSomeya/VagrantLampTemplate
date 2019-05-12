# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.hostname = "web_app.localhost"
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/var/www/app",owner: 'apache', group: 'apache', type:"virtualbox"


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

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
      sudo yum update
      #PHP 7.2インストール
      sudo yum -y remove php* #5系の削除
      sudo yum -y install epel-release
      sudo yum -y install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
      sudo yum install -y --enablerepo=remi,remi-php72,epel php php-devel php-mbstring php-mysqlnd php-mcrypt php-pear php-gd php-fpm php-pecl-xdebug php-opcache php-pecl-memcached
      #mysql5.7インストール
      sudo yum -y remove mariadb-libs #mariadb削除
      sudo rm -rf /var/lib/mysql/ #mariadb削除
      sudo yum -y localinstall http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
      sudo yum -y install mysql-community-server
      sudo systemctl start mysqld.service
      sudo systemctl enable mysqld.service #自動起動設定
      #apacheインストール
      sudo yum -y install httpd
      sudo systemctl start httpd
      sudo systemctl enable httpd

      #アプリケーション用ディレクトリ作成
      sudo mkdir /var/www/app
      sudo chown apache:apache /var/www/app

      #テストページ作成
      touch test.php
      echo '<?php echo "test page"; ?>' > test.php
      sudo mv test.php /var/www/app/

      #ルートディレクトりの変更を促す
      echo 'ルートディレクトリを/var/www/appに変更する必要があります。'
      echo 'command1: vagrant ssh'
      echo 'command2: cd /etc/httpd/conf/'
      echo 'command3: sudo vi httpd.conf'
      echo '以下のようにhttpd.confを編集'
      echo 'DocumentRoot /var/www/app'
      echo '<Directory "/var/www/app">'
      echo 'DirectoryIndex test.php index.html'
      echo 'command4:httpd -t でsyntax確認'
      echo 'command5:sudo systemctl restart httpd'
      echo '最後にselinuxを確認し、無効化などを行う'
      echo 'command6: getenforce で確認'
      echo 'command7: setenforce 0 で一時無効化'
      echo 'seLinuxの設定変更は /etc/selinux/config'
      echo 'SELINUX=disabledで無効化'
   SHELL
end
