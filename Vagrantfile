# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "cwoodcock/ubuntu-16.04"
  config.vm.network "private_network", type: "dhcp"

  config.vm.define "proxy" do |box|
    box.vm.hostname = "proxy.vagrant.test"
    box.landrush.enabled = true
    box.landrush.host_interface = 'enp0s8'
  end

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", 33]
    vb.customize ["modifyvm", :id, "--memory", 256]
    vb.linked_clone = true
  end

  # workaround for https://github.com/vagrant-landrush/landrush/issues/293
  config.vm.provision :shell,
    inline: "echo 'nameserver 8.8.8.8' > /etc/resolv.conf"

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
