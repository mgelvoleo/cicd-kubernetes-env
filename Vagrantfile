Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/jammy64"
    config.ssh.insert_key = false
  
    # Controller node
    config.vm.define "cicd" do |cicd|
      cicd.vm.hostname = "controller"
      cicd.vm.network "private_network", ip: "192.168.56.10"
      cicd.vm.provider "virtualbox" do |vb|
        vb.name = "controller"
        vb.memory = 2048
        vb.cpus = 2
      end
  
      cicd.vm.provision "shell", inline: <<-SHELL
        # Generate SSH key for controller if not exists
        if [ ! -f /home/vagrant/.ssh/id_rsa ]; then
          ssh-keygen -t rsa -b 4096 -N "" -f /home/vagrant/.ssh/id_rsa
          chown vagrant:vagrant /home/vagrant/.ssh/id_rsa*
        fi
  
        # Add host entries
        echo "192.168.56.11 master"  | sudo tee -a /etc/hosts
        echo "192.168.56.12 worker1" | sudo tee -a /etc/hosts
        echo "192.168.56.13 worker2" | sudo tee -a /etc/hosts
      SHELL
    end
  
    # Function to add controller's pubkey to other nodes
    def add_pubkey(node, hostname, ip)
      node.vm.hostname = hostname
      node.vm.network "private_network", ip: ip
      node.vm.provider "virtualbox" do |vb|
        vb.name = "k8s-#{hostname}"
        vb.memory = 2048
        vb.cpus = 2
      end
  
      node.vm.provision "shell", inline: <<-SHELL
        mkdir -p /home/vagrant/.ssh
        chmod 700 /home/vagrant/.ssh
        # Copy controller's public key (shared via /vagrant folder)
        if [ -f /vagrant/id_rsa.pub ]; then
          cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
          chmod 600 /home/vagrant/.ssh/authorized_keys
          chown -R vagrant:vagrant /home/vagrant/.ssh
        fi
      SHELL
    end
  
    # Master node
    config.vm.define "master" do |master|
      add_pubkey(master, "master", "192.168.56.11")
      master.vm.provider "virtualbox" do |vb|
        vb.memory = 3072
      end
    end
  
    # Worker1
    config.vm.define "worker1" do |worker|
      add_pubkey(worker, "worker1", "192.168.56.12")
    end
  
    # Worker2
    config.vm.define "worker2" do |worker|
      add_pubkey(worker, "worker2", "192.168.56.13")
    end
  end
  