IMAGE_NAME = "ubuntu/focal64"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.provider :virtualbox do |vm|
            vm.name = "k8s-master"
        end
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.56.10"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.verbose = "v"
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.56.10",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "k8s-worker-#{i}" do |node|
            node.vm.provider :virtualbox do |vm|
                vm.name = "k8s-worker-#{i}"
            end
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.56.#{i + 10}"
            node.vm.hostname = "k8s-worker-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.verbose = "v"
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.56.#{i + 10}",
                }
            end
        end
    end
end
