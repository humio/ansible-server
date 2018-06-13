# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box = "bento/centos-7.5"
  config.vm.box_version = "201805.15.0"

  config.vm.provider "vmware_desktop" do |v|
    v.vmx["memsize"] = "4096"
    v.vmx["numvcpus"] = "1"
  end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "cluster.yml"
      ansible.groups = {
      	"zookeepers" => ["default"],
      	"kafkas" => ["default"],
      	"humios" => ["default"]
      }
      ansible.host_vars = {
      	"humio0" => { "zookeeper_id" => 1, "kafka_broker_id" => 0, "humio_host_id" => "0", "humio_instance_index" => 0 }
	  }
  end
end
