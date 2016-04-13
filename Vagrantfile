# -*- mode: ruby -*-
# vi: set ft=ruby :

groups = {
  "app" => [ "app01", "app02" ],
  "web" => [ "lb01" ],
  "all_groups:children" => ["app", "web"]
}

Vagrant.configure(2) do |config|

  config.vm.define "app01" do |app01|
    app01.vm.box = "centos/7"
    app01.vm.hostname = "app01"
    app01.vm.network "private_network", ip: "172.16.128.20"

    app01.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/app.yml"
      ansible.groups = groups
      ansible.sudo = true
    end
  end

  config.vm.define "app02" do |app02|
    app02.vm.box = "centos/7"
    app02.vm.hostname = "app02"
    app02.vm.network "private_network", ip: "172.16.128.21"

    app02.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/app.yml"
      ansible.groups = groups
      ansible.sudo = true
    end
  end

  config.vm.define "web01" do |web01|
    web01.vm.box = "centos/7"
    web01.vm.hostname = "web01"
    web01.vm.network "private_network", ip: "172.16.128.10"

    web01.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/web.yml"
      ansible.groups = groups
      ansible.sudo = true
    end

    web01.vm.provision "shell", inline: "for i in {1..4}; do curl -s localhost:80; done"
  end

end
