Vagrant.configure("2") do |config|
    config.vm.box = "bertvv/centos72"
    config.ssh.forward_agent = true
    config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = "vm1"
    vm1.vm.network "private_network", ip: "10.10.10.1"
    vm1.vm.provision :shell, inline: <<-SHELL
        yum install git -y
        echo "10.10.10.2 vm2 vm2" >> /etc/hosts
        git clone https://github.com/IvanNikishov/GitVagrant.git
        cd GitVagrant
        git checkout -b task2 origin/task2
        cat README.md
    SHELL
end

config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = "vm2"
    vm2.vm.network "private_network", ip: "10.10.10.2"
    vm2.vm.provision :shell, inline: <<-SHELL
    echo "10.10.10.1 vm1 vm1" >> /etc/hosts
SHELL
end
end
