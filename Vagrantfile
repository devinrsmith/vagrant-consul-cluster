# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = '2'
PAPER_PATH = 'logs2.papertrailapp.com:55555'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"

  config.vm.define "n1" do |n|
    n.vm.hostname = "n1"
    n.vm.network "private_network", ip: "192.168.50.11"

    n.vm.provision "docker" do |d|

      d.run "logspout",
        image: "gliderlabs/logspout",
        cmd: "syslog://#{PAPER_PATH}",
        args: "-h n1 --restart=always --volume=/var/run/docker.sock:/tmp/docker.sock"
      
      d.run "consul",
        image: "progrium/consul",
        cmd: "-server -advertise 192.168.50.11 -bootstrap-expect 3",
        args: "-h n1 -p 192.168.50.11:8300:8300 -p 192.168.50.11:8301:8301 -p 192.168.50.11:8301:8301/udp -p 192.168.50.11:8302:8302 -p 192.168.50.11:8302:8302/udp -p 192.168.50.11:8400:8400 -p 192.168.50.11:8500:8500 -p 172.17.42.1:53:53 -p 172.17.42.1:53:53/udp"

      d.run "registrator",
        image: "gliderlabs/registrator",
        cmd: "consul://consul:8500",
        args: "-h n1 --link consul:consul -v /var/run/docker.sock:/tmp/docker.sock"

    end
  end

  config.vm.define "n2" do |n|
    n.vm.hostname = "n2"
    n.vm.network "private_network", ip: "192.168.50.12"

    n.vm.provision "docker" do |d|

      d.run "logspout",
        image: "gliderlabs/logspout",
        cmd: "syslog://#{PAPER_PATH}",
        args: "-h n2 --restart=always --volume=/var/run/docker.sock:/tmp/docker.sock"

      d.run "consul",
        image: "progrium/consul",
        cmd: "-server -advertise 192.168.50.12 -join 192.168.50.11",
        args: "-h n2 -p 192.168.50.12:8300:8300 -p 192.168.50.12:8301:8301 -p 192.168.50.12:8301:8301/udp -p 192.168.50.12:8302:8302 -p 192.168.50.12:8302:8302/udp -p 192.168.50.12:8400:8400 -p 192.168.50.12:8500:8500 -p 172.17.42.1:53:53 -p 172.17.42.1:53:53/udp"

      d.run "registrator",
        image: "gliderlabs/registrator",    
        cmd: "consul://consul:8500",
        args: "-h n2 --link consul:consul -v /var/run/docker.sock:/tmp/docker.sock"

    end
  end

  config.vm.define "n3" do |n|
    n.vm.hostname = "n3"
    n.vm.network "private_network", ip: "192.168.50.13"

    n.vm.provision "docker" do |d|

      d.run "logspout",
        image: "gliderlabs/logspout",
        cmd: "syslog://#{PAPER_PATH}",
        args: "-h n3 --restart=always --volume=/var/run/docker.sock:/tmp/docker.sock"

      d.run "consul", 
        image: "progrium/consul",
        cmd: "-server -advertise 192.168.50.13 -join 192.168.50.11",
        args: "-h n3 -p 192.168.50.13:8300:8300 -p 192.168.50.13:8301:8301 -p 192.168.50.13:8301:8301/udp -p 192.168.50.13:8302:8302 -p 192.168.50.13:8302:8302/udp -p 192.168.50.13:8400:8400 -p 192.168.50.13:8500:8500 -p 172.17.42.1:53:53 -p 172.17.42.1:53:53/udp"

      d.run "registrator",
        image: "gliderlabs/registrator",
        cmd: "consul://consul:8500",
        args: "-h n3 --link consul:consul -v /var/run/docker.sock:/tmp/docker.sock"

    end
  end
end
