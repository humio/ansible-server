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

  config.vm.define "humio0"
  config.vm.define "humio1"
  config.vm.define "humio2"
  config.vm.define "humio3"

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "cluster.yml"
      ansible.groups = {
      	"zookeepers" => ["humio[0:2]"],
      	"kafkas" => ["humio[0:2]"],
      	"humios" => ["humio[0:3]"]
      }
      ansible.host_vars = {
      	"humio0" => { "zookeeper_id" => 1, "kafka_broker_id" => 0, "humio_host_id" => "0", "humio_instance_index" => 0 },
      	"humio1" => { "zookeeper_id" => 2, "kafka_broker_id" => 1, "humio_host_id" => "1", "humio_instance_index" => 0 },
      	"humio2" => { "zookeeper_id" => 3, "kafka_broker_id" => 2, "humio_host_id" => "2", "humio_instance_index" => 0 },
      	"humio3" => { "humio_host_id" => "3", "humio_instance_index" => 0 },
	  }
  end
end
