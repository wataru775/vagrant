# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "centos/6"

  config.vm.network "forwarded_port", guest: 80, host: 8888, auto_config: false
  config.vbguest.installer_hooks[:before_install] = [
  'sed -i -e "s/^mirrorlist=http:\/\/mirrorlist.centos.org/#mirrorlist=http:\/\/mirrorlist.centos.org/g" /etc/yum.repos.d/CentOS-Base.repo' ,
   'sed -i -e "s/^#baseurl=http:\/\/mirror.centos.org/baseurl=http:\/\/vault.centos.org/g" /etc/yum.repos.d/CentOS-Base.repo',
   'yum -y update'
   ]
   config.vm.synced_folder "./public", "/usr/local/apache2/htdocs/", type: "virtualbox"

   config.vm.provision "shell", inline: <<-SHELL
    yum -y -q install gcc-c++ expat-devel

    curl --silent -o pcre-8.45.tar.gz https://free.nchc.org.tw/osdn//sfnet/p/pc/pcre/pcre/8.45/pcre-8.45.tar.gz
    tar zvfx pcre-8.45.tar.gz
    sudo tar zfx pcre-8.45.tar.gz -C /usr/local/src/
    cd /usr/local/src/pcre-8.45
    ./configure --prefix=/usr/local/pcre --silent
    make
    make install

    curl --silent -o apr-1.7.0.tar.gz https://dlcdn.apache.org//apr/apr-1.7.0.tar.gz
    sudo tar zfx apr-1.7.0.tar.gz -C /usr/local/src/
    cd /usr/local/src/apr-1.7.0
    ./configure --silent
    make
    make install

    curl --silent -o apr-util-1.6.1.tar.gz https://dlcdn.apache.org//apr/apr-util-1.6.1.tar.gz
    sudo tar zfx apr-util-1.6.1.tar.gz -C /usr/local/src/
    cd /usr/local/src/apr-util-1.6.1
    ./configure --with-apr=/usr/local/apr --silent
    make
    make install

    curl --silent -o httpd-2.4.51.tar.gz https://dlcdn.apache.org//httpd/httpd-2.4.51.tar.gz
    sudo tar zfx httpd-2.4.51.tar.gz -C /usr/local/src/
    cd /usr/local/src/httpd-2.4.51
    ./configure --with-apr=/usr/local/apr --with-pcre=/usr/local/pcre --silent
    make
    make install

    sudo /usr/local/apache2/bin/apachectl start
    /usr/local/apache2/bin/apachectl -v

    cd
  SHELL

end
