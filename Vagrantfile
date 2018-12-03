# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|
  config.vm.define "rtr1" do |rtr1|
        rtr1.vm.box = "n9k"
        rtr1.ssh.insert_key = false
        rtr1.vm.boot_timeout = 240
        rtr1.vm.synced_folder '.', '/vagrant', disabled: true
        rtr1.vm.network :forwarded_port, guest: 80, host: 8881, id: 'http'
        rtr1.vm.network :forwarded_port, guest: 22, host: 3321, id: 'ssh'
        rtr1.vm.network "private_network", auto_config: false, virtualbox__intnet: "hostA_vtepA"
        rtr1.vm.network "private_network", auto_config: false, virtualbox__intnet: "leaf1_spine"
        rtr1.vm.provider :virtualbox do |vb|
                vb.name = "rtr1"
                vb.customize ['modifyvm',:id,'--memory','6144']
                vb.customize ['modifyvm',:id,'--macaddress1','0800276CEEAA']
                vb.customize ['modifyvm',:id,'--nicpromisc2','allow-all']
                vb.customize ['modifyvm',:id,'--nicpromisc3','allow-all']
                vb.customize ['modifyvm',:id,'--nicpromisc4','allow-all']
                vb.customize ['modifyvm',:id,'--uart1','0x3F8','4']
                vb.customize ['modifyvm',:id,'--uartmode1','server','/tmp/rtr1']
                vb.customize "pre-boot", [
                        "storageattach", :id,
                        "--storagectl", "SATA",
                        "--port", "1",
                        "--device", "0",
                        "--type", "dvddrive",
                        "--medium", "./rtr1_config.iso",
                ]
        end
  end
  config.vm.define "rtr2" do |rtr2|
        rtr2.vm.box = "n9k"
        rtr2.ssh.insert_key = false
        rtr2.vm.boot_timeout = 240
        rtr2.vm.synced_folder '.', '/vagrant', disabled: true
        rtr2.vm.network :forwarded_port, guest: 80, host: 8882, id: 'http'
        rtr2.vm.network :forwarded_port, guest: 22, host: 3322, id: 'ssh'
        rtr2.vm.network "private_network", auto_config: false, virtualbox__intnet: "hostB_vtepB"
        rtr2.vm.network "private_network", auto_config: false, virtualbox__intnet: "leaf2_spine"
        rtr2.vm.provider :virtualbox do |vb|
                vb.name = "rtr2"
                vb.customize ['modifyvm',:id,'--memory','6144']
                vb.customize ['modifyvm',:id,'--macaddress1','0800276CEEBB']
                vb.customize ['modifyvm',:id,'--nicpromisc2','allow-all']
                vb.customize ['modifyvm',:id,'--nicpromisc3','allow-all']
                vb.customize ['modifyvm',:id,'--nicpromisc4','allow-all']
                vb.customize ['modifyvm',:id,'--uart1','0x3F8','4']
                vb.customize ['modifyvm',:id,'--uartmode1','server','/tmp/rtr2']
                vb.customize "pre-boot", [
                        "storageattach", :id,
                        "--storagectl", "SATA",
                        "--port", "1",
                        "--device", "0",
                        "--type", "dvddrive",
                        "--medium", "./rtr2_config.iso",
                ]
        end
  end
  config.vm.define "rtr3" do |rtr3|
        rtr3.vm.box = "n9k"
        rtr3.ssh.insert_key = false
        rtr3.vm.boot_timeout = 240
        rtr3.vm.synced_folder '.', '/vagrant', disabled: true
        rtr3.vm.network :forwarded_port, guest: 80, host: 8883, id: 'http'
        rtr3.vm.network :forwarded_port, guest: 22, host: 3323, id: 'ssh'
        rtr3.vm.network "private_network", auto_config: false, virtualbox__intnet: "hostC_vtepA"
        rtr3.vm.network "private_network", auto_config: false, virtualbox__intnet: "leaf1_spine"
        rtr3.vm.network "private_network", auto_config: false, virtualbox__intnet: "leaf2_spine"
        rtr3.vm.provider :virtualbox do |vb|
                vb.name = "rtr3"
                vb.customize ['modifyvm',:id,'--memory','6144']
                vb.customize ['modifyvm',:id,'--macaddress1','0800276CEECC']
                vb.customize ['modifyvm',:id,'--nicpromisc2','allow-all']
                vb.customize ['modifyvm',:id,'--nicpromisc3','allow-all']
                vb.customize ['modifyvm',:id,'--nicpromisc4','allow-all']
                vb.customize ['modifyvm',:id,'--uart1','0x3F8','4']
                vb.customize ['modifyvm',:id,'--uartmode1','server','/tmp/rtr3']
                vb.customize "pre-boot", [
                        "storageattach", :id,
                        "--storagectl", "SATA",
                        "--port", "1",
                        "--device", "0",
                        "--type", "dvddrive",
                        "--medium", "./rtr3_config.iso",
                ]
        end
  end

  config.vm.define "bird1" do |bird1|
    config.vm.provider "virtualbox" do |bird1|
      bird1.name = "bird1"
    end
    bird1.vm.box = "ubuntu/bionic64"
        bird1.vm.network :forwarded_port, guest: 22, host: 12200, id: 'ssh'
        bird1.vm.network "private_network", virtualbox__intnet: "hostA_vtepA", auto_config: false
        bird1.vm.network "private_network", virtualbox__intnet: "hostB_vtepB", auto_config: false
        bird1.vm.network "private_network", virtualbox__intnet: "hostB_vtepB", auto_config: false
        bird1.vm.provision "shell", inline: <<-SHELL
          sudo ip addr add 192.168.11.2/31 dev enp0s8
          sudo ip addr add 192.168.22.2/31 dev enp0s9
          sudo ip addr add 100.100.100.100/32 dev lo:100
          sudo ip link set enp0s8 up
          sudo ip link set enp0s9 up
          sudo apt-get -y install software-properties-common
          sudo apt-get -y install python-software-properties
          sudo apt-get update
          sudo apt-get -y install bird
          sudo apt-get -y install traceroute
          sudo apt-get -y install tcpdump
          sudo apt-get -y install lldpd
          sudo service lldpd start
	  sudo cp /vagrant/bird.conf /etc/bird/bird.conf
	  sudo service bird restart
        SHELL
        bird1.vm.hostname = "bird1"
  end
  config.vm.define "bird2" do |bird2|
    config.vm.provider "virtualbox" do |bird2|
      bird2.name = "bird2"
    end
    bird2.vm.box = "ubuntu/bionic64"
        bird2.vm.network :forwarded_port, guest: 22, host: 13200, id: 'ssh'
        bird2.vm.network "private_network", virtualbox__intnet: "hostC_vtepA", auto_config: false
        bird2.vm.provision "shell", inline: <<-SHELL
          sudo ip addr add 172.16.99.200/24 dev enp0s8
          sudo ip addr add 192.168.0.0/16 via 172.16.99.1
          sudo ip addr add 172.16.0.0/16 via 172.16.99.1
          sudo ip addr add 100.100.100.100/32 via 172.16.99.1
          sudo ip link set enp0s8 up
          sudo apt-get update
          sudo apt-get -y install traceroute
          sudo apt-get -y install tcpdump
          sudo apt-get -y install mtr
        SHELL
        bird2.vm.hostname = "bird2"
  end

end
  
