# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
$instance_name_prefix='deb'
$num_instances = 3
$vm_memory = 1024
$vm_cpus = 1

if ENV["NUM_INSTANCES"].to_i > 0 && ENV["NUM_INSTANCES"]
  $num_instances = ENV["NUM_INSTANCES"].to_i
end
def vm_gui
  $vb_gui.nil? ? $vm_gui : $vb_gui
end

def vm_memory
  $vb_memory.nil? ? $vm_memory : $vb_memory
end

def vm_cpus
  $vb_cpus.nil? ? $vm_cpus : $vb_cpus
end


  config.vm.box = "debian/jessie64"
  # always use Vagrants insecure key
  config.ssh.insert_key = false
  # forward ssh agent to easily ssh into the different machines
  config.ssh.forward_agent = true
  config.vm.provider :virtualbox do |v|
    # On VirtualBox, we don't have guest additions or a functional vboxsf
    # in CoreOS, so tell Vagrant that so it can be smarter.
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  # plugin conflict
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end
  (1..$num_instances).each do |i|
    config.vm.define vm_name = "%s-%02d" % [$instance_name_prefix, i] do |config|
    config.vm.hostname = vm_name
    config.vm.provider :virtualbox do |vb|
      vb.memory = vm_memory
      vb.cpus = vm_cpus
    end
      ip = "172.17.9.#{i+100}"
      config.vm.network :private_network, ip: ip
    end
  end
end
