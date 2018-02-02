Vagrant.configure("2") do |config|
config.vm.box = "bertvv/centos72"
config.ssh.forward_agent = true
config.vm.define "vm1" do |vm1|
vm1.vm.hostname = "vm1"
vm1.vm.network "private_network", ip: "10.10.10.1"
vm1.vm.provision :shell, inline: <<-SHELL
yum install git -y
echo "10.10.10.2 vm2 vm2" >> /etc/hosts
mkdir -p ~/.ssh
chmod 700 ~/.ssh
ssh-keyscan -H github.com >> ~/.ssh/known_hosts
ssh -T git@github.com
git clone https://github.com/IvanNikishov/GitVagrant.git
cd Git_vagrant
git checkout -b task2 origin/task2
cat Git_vagrant
echo "Hello Git_vagrant" >> test
git add Git_vagrant
git config --global user.name "IvanNikishov"
git config --global user.email nikishov.ivan.93@gmail.com
git commit -m 'testVagrant'
git remote set-url origin ggit@github.com:IvanNikishov/GitVagrant.git
git push -u origin task2
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

