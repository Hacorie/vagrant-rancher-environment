# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'ipaddr'
require_relative 'fix-network-issues.rb'

def getMachineMap(numAgents)
  map = Hash.new
  rancherServerIp = IPAddr.new "192.168.33.100"
  map["rancher-server"] = {hostname: "rancher-server", ip: rancherServerIp, memory: 1536, rancherServerIp: rancherServerIp, role: "server"}

  1.upto(numAgents).each do |i|
    map["rancher-agent#{i}"] = {hostname: "rancher-agent#{i}", ip: map.values.last[:ip].succ, memory: 1064, rancherServerIp: rancherServerIp, role: "agent"}
  end

  return map
end

#helper functions
def createRancherMachine (vagrant, config)
  hostname = config[:hostname]
  role = config[:role]
  isServerRole = (role == "server")
  puts isServerRole

  vagrant.vm.define hostname do |machine|
    machine.vm.hostname = hostname
    machine.vm.network "private_network", ip: config[:ip].to_s

    machine.vm.provider "virtualbox" do |vb|
      vb.memory = config[:memory]
    end

    machine.vm.provision :docker

    machine.vm.provision :rancher do |rancher|
      puts "here"
      rancher.version = "v1.1.1"
      rancher.hostname = config[:rancherServerIp].to_s
      rancher.port = 8080
      rancher.role = role
      rancher.project = "Default"
      rancher.project_type = "cattle"
      rancher.install_agent = isServerRole ? false : true
      rancher.deactivate = isServerRole ? true : false
      #TODO: for some reason these lines throw errors when they weren't before :/
      #rancher.labels["role=#{role}","env=local"]
      #rancher.server_args = "-- restart always"
    end
  end
end

Vagrant.configure("2") do |vagrant|
  vagrant.vm.box = "ubuntu/trusty64"
  machineMap = getMachineMap(3)
  machineMap.each do |key, value|
    createRancherMachine vagrant, value
  end
end

