# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  #config.vm.box = "ubuntu/xenial64"
  config.vm.box = "boonen/docker_debian-jessie64"
  # config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |v|
    v.memory = 256
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y python python-pip
    pip install docker-py
  SHELL

  (1..6).each do |i|
    if [1,2,3].include?(i)
      config.vm.define "manager-#{i}" do |node|
        node.vm.hostname = "manager-#{i}"
        node.vm.network "private_network", ip: "192.168.33.1#{i}"
        # node.vm.network "forwarded_port", guest: 80, host: 8080
      end
    else
      config.vm.define "worker-#{i}" do |node|
        node.vm.hostname = "worker-#{i}"
        node.vm.network "private_network", ip: "192.168.33.1#{i}"
        # node.vm.network "forwarded_port", guest: 80, host: 8080
        # hack to only run once at the end
        if i == 6
          node.vm.provision "ansible" do |ansible|
            ansible.playbook = "swarm.yml"
            #ansible.playbook = "swarm-facts.yml"
            ansible.limit = "all"
            ansible.extra_vars = {
              swarm_iface: "eth1"
            }
            ansible.groups = {
              "manager" => ["manager-[1:3]"],
              "worker"  => ["worker-[4:6]"],
            }
            ansible.raw_arguments = [
              "-M ./library"
            ]
          end
        end
      end
    end
  end
end
