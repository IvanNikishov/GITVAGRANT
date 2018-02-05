Vagrant.configure("2") do |config|
    config.vm.box = "bertvv/centos72"
    config.ssh.forward_agent = true
config.vm.define "server1" do |server1|
        server1.vm.hostname = "server1"
        server1.vm.network "private_network", ip: "172.16.1.101"
        server1.vm.network "forwarded_port", guest: 80, host: 8080
        server1.vm.provision :shell, inline: <<-SHELL
        yum install httpd -y
        systemctl enable httpd 
        systemctl start httpd 
        firewall-cmd --zone=public --add-port=80/tcp --permanent
        firewall-cmd --reload
        cp /vagrant/mod_jk.so /etc/httpd/modules/
        chmod +x /etc/httpd/modules/mod_jk.so
        echo 'worker.list=lb' >> /etc/httpd/conf/workers.properties
        echo 'worker.lb.type=lb' >> /etc/httpd/conf/workers.properties
        echo 'worker.lb.balance_workers=server2, server3' >> /etc/httpd/conf/workers.properties
        echo 'worker.server2.host=172.16.1.102' >> /etc/httpd/conf/workers.properties
        echo 'worker.server2.port=8009' >> /etc/httpd/conf/workers.properties
        echo 'worker.server2.type=ajp13' >> /etc/httpd/conf/workers.properties
        echo 'worker.server3.host=172.16.1.103' >> /etc/httpd/conf/workers.properties
        echo 'worker.server3.port=8009' >> /etc/httpd/conf/workers.properties
        echo 'worker.server3.type=ajp13' >> /etc/httpd/conf/workers.properties
        echo 'LoadModule jk_module modules/mod_jk.so' >> /etc/httpd/conf/httpd.conf
        echo 'JkWorkersFile conf/workers.properties' >> /etc/httpd/conf/httpd.conf
        echo 'JkShmFile /tmp/shm' >> /etc/httpd/conf/httpd.conf
        echo 'JkLogFile logs/mod_jk.log ' >> /etc/httpd/conf/httpd.conf
        echo 'JkLogLevel info' >> /etc/httpd/conf/httpd.conf
        echo ' JkMount /test* lb' >> /etc/httpd/conf/httpd.conf
        service httpd restart 
SHELL
end
config.vm.define "server2" do |server2|
        server2.vm.hostname = "server2"
        server2.vm.network "private_network", ip: "172.16.1.102"
        server2.vm.provision :shell, inline: <<-SHELL
        yum install java-1.8.0-openjdk tomcat tomcat-webapps tomcat-admin-webapps -y
        systemctl enable tomcat
        systemctl start tomcat
        systemctl stop firewalld
        mkdir /usr/share/tomcat/webapps/testdir
        chown -R tomcat:tomcat /usr/share/tomcat/webapps/testdir/ 
        echo 'tomcat1' >> /usr/share/tomcat/webapps/testdir/index.html 
SHELL
end
config.vm.define "server3" do |server3|
        server3.vm.hostname = "server3"
        server3.vm.network "private_network", ip: "172.16.1.103"
        server3.vm.provision :shell, inline: <<-SHELL
        yum install java-1.8.0-openjdk tomcat tomcat-webapps tomcat-admin-webapps -y
        systemctl enable tomcat
        systemctl start tomcat
        systemctl stop firewalld
        mkdir /usr/share/tomcat/webapps/testdir
        chown -R tomcat:tomcat /usr/share/tomcat/webapps/testdir/ 
        echo 'tomcat2' >> /usr/share/tomcat/webapps/testdir/index.html 
SHELL

end
end
