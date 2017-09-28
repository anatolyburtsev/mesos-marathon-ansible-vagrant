# -*- mode: ruby -*-
# vi: set ft=ruby :


ANSIBLE_GROUPS = {
              "master" => ["master1", "master2", "master3"],
              "nodes" => ["node1","node2","node3"],
              "all_groups:children" => ["master", "nodes"]
            }

cluster = {
        "master1" => { :ip => "192.168.33.10", :cpus => 1, :mem => 1024 },
        "master2" => { :ip => "192.168.33.11", :cpus => 1, :mem => 1024 },
        "master3" => { :ip => "192.168.33.12", :cpus => 1, :mem => 1024 },
        "node1" => { :ip => "192.168.33.20", :cpus => 1, :mem => 1024 },
        "node2" => { :ip => "192.168.33.21", :cpus => 1, :mem => 1024 },
        "node3" => { :ip => "192.168.33.22", :cpus => 1, :mem => 1024 }
}

Vagrant.configure(2) do |config|
    config.vm.box = "bento/centos-7.1"

    cluster.each_with_index do |(hostname, info), index|
        config.vm.define hostname do |cfg|
            cfg.vm.network "private_network", ip: "#{info[:ip]}"
            cfg.vm.hostname = hostname
            cfg.hostsupdater.aliases = [hostname] 
            cfg.vm.provision "ansible" do |ansible|
                ansible.extra_vars = {
                    zoo1: "192.168.33.10",
                    zoo2: "192.168.33.11",
                    zoo3: "192.168.33.12",
                    zoo_myid: index + 1
                }
                ansible.playbook = "playbook.yml"
                ansible.groups = ANSIBLE_GROUPS
            end
        end
    end
end
