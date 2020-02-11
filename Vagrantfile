# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
servers = YAML.load_file(File.join(File.dirname(__FILE__), 'servers.yml'))


Vagrant.configure(2) do |config|
    
    servers.each do |servers|
      # Define the nodes configuration doing a iteration 
        config.vm.define servers["name"] do |srv|

          # Config the VM
            srv.vm.box = servers["box"]
            srv.vm.hostname = servers["name"]
            srv.vm.network :private_network, :ip => servers["ip"], :libvirt__network_name => "default" 
            srv.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: true
            srv.vm.network "forwarded_port", guest: 22, host: servers["ssh_port"], auto_correct: true
            if servers['name'] == 'k8smaster'
              srv.vm.network "forwarded_port", guest: 8001, host: 8001, auto_correct: true
            end

            srv.vm.provider "libvirt" do |virt|
                virt.memory = servers["ram"]
                virt.cpus = servers["vcpu"]
            end

            srv.vm.provider "virtualbox" do |vb|
                vb.memory = servers["ram"]
                vb.cpus = servers["vcpu"]
            end

            srv.vm.provider "vmware_desktop" do |vmw|
                vmw.vmx["memsize"] = servers["ram"]
                vmw.vmx["numvcpus"] = servers["vcpu"]
            end

        end
    end

    
end
