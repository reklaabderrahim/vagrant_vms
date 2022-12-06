NUM_MASTER_NODE = 1
NUM_WORKER_NODE = 2

IP_NW = "192.168.56."
MASTER_IP_START = 1
NODE_IP_START = 2

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false

  # Provision Master Nodes
  (1..NUM_MASTER_NODE).each do |i|
      config.vm.define "master-#{i}" do |node|
        # Name shown in the GUI
        node.vm.provider "virtualbox" do |vb|
            vb.name = "master-#{i}"
            vb.memory = 10240
            vb.cpus = 4
        end
        node.vm.hostname = "master-#{i}"
        node.vm.network :private_network, ip: IP_NW + "#{MASTER_IP_START + i}"
        node.vm.network "forwarded_port", guest: 22, host: "#{2710 + i}"

        node.vm.provision "setup-hosts", :type => "shell", :path => "setup-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision "setup-dns", type: "shell", :path => "update-dns.sh"

      end
  end


  # Provision Worker Nodes
  (1..NUM_WORKER_NODE).each do |i|
    config.vm.define "node0#{i}" do |node|
        node.vm.provider "virtualbox" do |vb|
            vb.name = "node0#{i}"
            vb.memory = 10240
            vb.cpus = 4
        end
        node.vm.hostname = "node0#{i}"
        node.vm.network :private_network, ip: IP_NW + "#{NODE_IP_START + i}"
                node.vm.network "forwarded_port", guest: 22, host: "#{2720 + i}"

        node.vm.provision "setup-hosts", :type => "shell", :path => "setup-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision "setup-dns", type: "shell", :path => "update-dns.sh"
    end
  end
end
