ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "bootstrap.sh"

  # Kubernetes Master Server
  config.vm.define "k8s-master" do |kmaster|
    kmaster.vm.box = "centos/7"
    kmaster.vm.hostname = "k8s-master.example.com"
    kmaster.vm.network "public_network", ip: "192.168.1.150"
    kmaster.vm.provider "virtualbox" do |v|
      v.name = "k8s-master"
      v.memory = 2048
      v.cpus = 2
      # Prevent VirtualBox from interfering with host audio stack
      v.customize ["modifyvm", :id, "--audio", "none"]
    end
    kmaster.vm.provision "shell", path: "bootstrap_kmaster_calico.sh"
  end

  NodeCount = 2

  # Kubernetes Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.define "k8s-worker#{i}" do |workernode|
      workernode.vm.box = "centos/7"
      workernode.vm.hostname = "k8s-worker#{i}.example.com"
      workernode.vm.network "public_network", ip: "192.168.1.15#{i}"
      workernode.vm.provider "virtualbox" do |v|
        v.name = "k8s-worker#{i}"
        v.memory = 1024
        v.cpus = 1
        # Prevent VirtualBox from interfering with host audio stack
        v.customize ["modifyvm", :id, "--audio", "none"]
      end
      workernode.vm.provision "shell", path: "bootstrap_kworker.sh"
    end
  end

end