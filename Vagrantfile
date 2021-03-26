$script_mysql = <<-SCRIPT
  apt-get update && \
  apt-get install -y mysql-server-5.7 && \
  mysql -e "create user 'phpuser'@'%' identified by 'pass';"
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.boot_timeout = 1200

    config.vm.define "mysql" do |mysql|
	    mysql.vm.network "public_network", ip: "192.168.31.241", netmask: "255.255.255.0"

	    mysql.vm.provision "shell", inline: "cat /config/id_vagrant.pub >> .ssh/authorized_keys"
	    mysql.vm.provision "shell", inline: $script_mysql
	    mysql.vm.provision "shell", inline: "cat /config/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf"
	    mysql.vm.provision "shell", inline: "service mysql restart"
	    
	    mysql.vm.synced_folder "./config", "/config"
	    mysql.vm.synced_folder ".", "/vagrant", disabled: true
	end

	config.vm.define "phpweb" do |phpweb|
	    phpweb.vm.network "forwarded_port", guest: 80, host: 8089
	    phpweb.vm.network "public_network", ip: "192.168.31.242", netmask: "255.255.255.0"
	end

  end