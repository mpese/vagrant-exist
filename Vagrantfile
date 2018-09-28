Vagrant.configure(2) do |config|
  # https://docs.vagrantup.com.

  config.vm.box = "centos/7"
  config.vm.hostname = "mpese-vagrant.rit.bris.ac.uk"

  config.vm.define "web", primary: true do |web|
    web.vm.network "private_network", ip: "192.168.56.8"
    config.vm.network "forwarded_port", guest: 8080, host: 9090
    config.vm.network "forwarded_port", guest: 80, host: 8000
    config.vm.network "forwarded_port", guest: 22, host: 2214

    config.vm.provider "virtualbox" do |vb|
      vb.cpus = "4"
      vb.memory = "4096"
    end

    web.vm.provision "shell", inline: <<-SHELL
	setenforce 0
        sed -i -e 's/^SELINUX=.*/SELINUX=permissive/' /etc/selinux/config

        yum -y update
	yum install -y epel-release
	yum install -y git

	yum install -y nginx
        cp /vagrant/nginx.conf /etc/nginx/nginx.conf
        chown root.root /etc/nginx/nginx.conf
        systemctl enable nginx
        systemctl start nginx

        rpm -Uvh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm
        yum install -y puppet-agent
        yum install -y rubygems
        yum install -y ruby-augeas
        gem install r10k:2.6.4

        cd /vagrant/control
        /usr/local/bin/r10k puppetfile install -v info

        /opt/puppetlabs/bin/puppet apply -e 'hiera_include("classes")' --hiera_config /vagrant/hiera.yaml --modulepath modules

	mkdir -p /srv/projects/mpese/images
	chown -R existdb. /srv/projects/mpese
    SHELL
  end
end
