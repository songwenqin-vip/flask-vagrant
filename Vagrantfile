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
  # boxes at https://atlas.hashicorp.com/search.
  #config.vm.box = "ubuntu/trusty32"
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "flask-dev"

  config.vm.provider :virtualbox do |vb|
        vb.name = "flask-dev"
        vb.memory = "8192"
        vb.cpus = "4"
    end
  config.vbguest.auto_update = false

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
  config.vm.network "public_network", use_dhcp_assigned_default_route: true
  config.vm.network "private_network",  type:"dhcp"

  config.vm.synced_folder "/flask-dev", "/share",
    owner: "vagrant",
    group: "vagrant",
    mount_options: ["dmode=775,fmode=755"]

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
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

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  
  config.vm.provision "shell", inline: <<-SHELL
    echo "install basic softwares"
    apt-get -qq update -y
    apt-get -qq install -y git lftp tree unzip
    mkdir -p /home/vagrant/workspace/
    chown vagrant:vagrant /home/vagrant/workspace/
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    echo "prepare Python environment"
    apt-get -qq install -y gcc-multilib g++-multilib libffi-dev libffi6 libffi6-dbg python-crypto python-mox3 python-pil python-ply libssl-dev zlib1g-dev libbz2-dev libexpat1-dev libbluetooth-dev libgdbm-dev dpkg-dev quilt autotools-dev libreadline-dev libtinfo-dev libncursesw5-dev tk-dev blt-dev libssl-dev zlib1g-dev libbz2-dev libexpat1-dev libbluetooth-dev libsqlite3-dev libgpm2 mime-support netbase net-tools bzip2
    wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz
    tar xfz Python-2.7.14.tgz
    rm Python-2.7.14.tgz
    cd Python-2.7.14
    ./configure --prefix /usr/local/lib/python2.7.14 --enable-ipv6
    make
    make install
    cd ..
    rm -rf Python-2.7.14
    ln -sf /usr/local/lib/python2.7.14/bin/python /usr/bin/python
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    python get-pip.py
    rm get-pip.py
    ln -s /usr/local/lib/python2.7.14/bin/easy_install /usr/bin/easy_install
    ln -s /usr/local/lib/python2.7.14/bin/pip /usr/bin/pip
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    echo "prepare flask environment"
    pip install flask-restful
  SHELL
   
  config.vm.provision "shell", inline: <<-SHELL
    echo "install Postgres"
    wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
    sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
    apt-get update
    apt-get install -y postgresql postgresql-contrib
    sed -i "s/#listen_address.*/listen_addresses '*'/" /etc/postgresql/*/main/postgresql.conf
    apt-get -qq install -y libpq-dev
    pip install pygresql
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    echo "install Ubuntu Desktop"
    apt-get install -y --no-install-recommends ubuntu-desktop
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    echo "install Pycharm"
    wget -q https://download.jetbrains.com/python/pycharm-community-2017.2.3.tar.gz
    tar xvf pycharm-community-2017.2.3.tar.gz
    mv pycharm-community-2017.2.3 /opt
    rm pycharm-community-2017.2.3.tar.gz
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    echo "Rebooting...bye"
    reboot
  SHELL



   
   
end
