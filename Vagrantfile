# -*- mode: ruby -*-
# vi: set ft=ruby :
#
#

# Script for creating kvm (Kernel native) vm's for kubernetes.
# need a bridge set up under br0

# Network prefix to which a single digit is appended to form the nodes'
# addresses. The master will be at the first address (e.g NETWORK_PREFIX + 0 as
# in 192.168.1.5 -> 192.168.1.50 and. Workers will take 192.168.1.51, 52, 53,
# ...)
NETWORK_PREFIX="192.168.5.6"

# Make sure libvirt, qemu, kvm plugins are installed.
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
IMAGE_NAME = "debian/bullseye64"

# Number of nodes. Since we are appending a number to a string to calculate the
# IP numbers, the number of nodes should be less than 10.
NUM_NODES = 3 

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.provider :libvirt do |libvirt|
    libvirt.cpus = 2       # Number of cores
    libvirt.memory = 4096  # Memory in MB
    libvirt.driver = "kvm" # KVM is the default hypervisor
  end

  config.vm.define "master-1" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.network :public_network,
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "#{NETWORK_PREFIX}0"
    master.vm.hostname = "master"
  end

  (1..NUM_NODES).each do |i|
    config.vm.define "worker-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.network :public_network,
        :dev => "br0",
        :mode => "bridge",
        :type => "bridge",
        :ip => "#{NETWORK_PREFIX}#{i}"
      node.vm.hostname = "worker-#{i}"
    end
  end
end
