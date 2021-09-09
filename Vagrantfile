$script = <<-SCRIPT
apt-get update
cat /tmp/pub_key.pub >> /home/vagrant/.ssh/authorized_keys
chown -R vagrant:vagrant /home/vagrant/.ssh/authorized_keys
chmod 600 /home/vagrant/.ssh/authorized_keys
apt-get install python3.6
echo "Client done!"
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "node1" do |node|
    node.vm.box = "bento/ubuntu-18.04"
    node.vm.define "project"
    node.vm.network "private_network", ip: "192.168.20.20"
    node.vm.provider "virtualbox" do |vb|
       vb.gui = false
       vb.memory = "512"
       vb.cpus = "4"
    end
    node.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/tmp/pub_key.pub"
    node.vm.provision "shell",
        inline: $script
  end
end

