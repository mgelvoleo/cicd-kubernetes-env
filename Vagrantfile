Vagrant.configure("2") do |config|
    # Base box
    config.vm.box = "ubuntu/jammy64"
  
    # Shared SSH settings
    config.ssh.insert_key = false
  
    # Control Node - CI/CD
    config.vm.define "controller" do |controller|
      controller.vm.hostname = "controller"
      controller.vm.network "private_network", ip: "192.168.56.10"
      controller.vm.provider "virtualbox" do |vb|
        vb.name = "controller"
        vb.memory = 2048
        vb.cpus = 2
      end
    end
  
    # Kubernetes Master
    config.vm.define "master" do |master|
      master.vm.hostname = "master"
      master.vm.network "private_network", ip: "192.168.56.11"
      master.vm.provider "virtualbox" do |vb|
        vb.name = "k8s-master"
        vb.memory = 3072
        vb.cpus = 2
      end
    end
  
    # Kubernetes Worker 1
    config.vm.define "worker1" do |worker|
      worker.vm.hostname = "worker1"
      worker.vm.network "private_network", ip: "192.168.56.12"
      worker.vm.provider "virtualbox" do |vb|
        vb.name = "k8s-worker1"
        vb.memory = 2048
        vb.cpus = 2
      end
    end
  
    # Kubernetes Worker 2
    config.vm.define "worker2" do |worker|
      worker.vm.hostname = "worker2"
      worker.vm.network "private_network", ip: "192.168.56.13"
      worker.vm.provider "virtualbox" do |vb|
        vb.name = "k8s-worker2"
        vb.memory = 2048
        vb.cpus = 2
      end
    end
  end
  