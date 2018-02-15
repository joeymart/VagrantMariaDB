# -*- mode: ruby -*-
# vi: set ft=ruby :

# Alleged macOS CPU issue resolved by this
$enable_serial_logging = false

Vagrant.configure(2) do |config|
# alternative centos7 box
#	config.vm.box = "geerlingguy/centos7"
#  config.ssh.username = "vagrant"
#  config.ssh.password = "vagrant"
#  config.ssh.private_key_path = "~/.ssh/id_rsa"
#  config.ssh.forward_agent = true
#  config.ssh.insert_key = false

	config.vm.box = "centos/6"
	config.vm.boot_timeout = 600

	# vagrant vb-guest needs kernel-devel, but centos whines about it, do a yum update and then reload
  config.vm.provision "shell", name: "yum update", inline: "sudo yum update -y"
  config.vm.provision "shell", name: "yum install kernel-devel (req by vbguest)", inline: "sudo yum install -y \"kernel-devel-uname-r == $(uname -r)\""

  #	config.vm.network "private_network", ip: "192.168.33.33"
	config.vm.network "forwarded_port", guest: 3306, host: 3333

  # Copy MariaDB repo for yum
  config.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777", "fmode=666"]
  config.vm.provision "shell", name: "Sync mariadb.repo", inline: "sudo cp /vagrant/mariadb.repo /etc/yum.repos.d/"
  config.vm.provision "shell", name: "install mariadb", inline: "sudo yum install -y MariaDB-server MariaDB-client"
  config.vm.provision "shell", name: "Start mariadb", inline: "sudo /etc/init.d/mysql start"
  config.vm.provision "shell", name: "Set mariadb to start on startup", inline: "sudo chkconfig mysql on"
  config.vm.provision "shell", name: "Ensuring my.cnf target dir", inline: "sudo mkdir -p /etc/my.cnf.d"
  config.vm.provision "shell", name: "Sync my.cnf", inline: "sudo cp /vagrant/my.cnf /etc/my.cnf.d/my.cnf"
  config.vm.provision "shell", name: "Restarting MariaDB on new my.cnf", inline: "sudo /etc/init.d/mysql restart"
  config.vm.provision "shell", name: "Granting root access from anywhere", 
    inline: "mysql -uroot -e \"GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'  WITH GRANT OPTION; FLUSH PRIVILEGES;\""
  
  config.vm.provision "shell", run: "always", name: "Show mysql startup status", inline: "sudo service mysql status"
  
	#config.vm.synced_folder "~/Downloads/glide-db-dumps/", "/glide-db-dumps", :mount_options => ["dmode=777", "fmode=666"]
	#config.vm.synced_folder "~/repositories/server/", "/mariadb-source/server", :mount_options => ["dmode=777", "fmode=666"]

  # Some boxes spin up with cable disconnected, just make sure our guest network is "plugged-in"
  config.vm.provider 'virtualbox' do |vb|
    vb.customize ['modifyvm', :id, '--cableconnected1', 'on']
  end
end
