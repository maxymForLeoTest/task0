Vagrant.require_version ">= 1.5"

require 'getoptlong'
opts = GetoptLong.new(
        [ '--nodesAmount', GetoptLong::OPTIONAL_ARGUMENT ]
)

nodesAmount = 2
opts.each do |opt, arg|
 case opt
   when '--nodesAmount'
    nodesAmount = arg
 end
end
nodesAmount = nodesAmount.to_i
puts "Number of nodes #{nodesAmount}"

Vagrant.configure("2") do |config|  
    config.vm.define :loadbalancer do |loadbalancer|
        loadbalancer.vm.provider :virtualbox do |v|
            v.name = "loadbalancer"
            v.customize [
                "modifyvm", :id,
                "--name", "loadbalancer",
                "--memory", 512,
                "--natdnshostresolver1", "on",
                "--cpus", 1,
            ]
        end

        loadbalancer.vm.box = "ubuntu/trusty64"
	loadbalancer.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
        loadbalancer.vm.network :private_network, ip: "192.168.30.10"
        loadbalancer.ssh.forward_agent = true
        loadbalancer.vm.synced_folder "./", "/vagrant", :nfs => true
    end


(1..nodesAmount).each do |i|
    config.vm.define "application_#{i}" do |application|
        application.vm.provider :virtualbox do |v|
            v.name = "application_#{i}"
            v.customize [
                "modifyvm", :id,
                "--name", "application_#{i}",
                "--memory", 512,
                "--natdnshostresolver1", "on",
                "--cpus", 1,
            ]
        end

        application.vm.box = "ubuntu/trusty64"
	application.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
        application.vm.network :private_network, ip: "192.168.30.2#{i}"
        application.ssh.forward_agent = true
        application.vm.synced_folder "./", "/vagrant", :nfs => true
    end
end 
end 
