<img align="right" src="logo.svg" style="width: 160px" />

# Deploy Humio with Ansible

## Running locally with Vagrant

Make sure you have [Vagrant](https://vagrantup.com) installed.

Then run the following command

```bash
$ vagrant up
```

Humio is now running on the primary network adapter, i.e. http://192.168.150.130:8080

## Deploying a single node instance

Make sure you have [Ansible](https://www.ansible.com) installed.

Create a new inventory file, i.e.

```yaml
all:
  hosts:
    humio.example.com:
      zookeeper_id: 0
      kafka_broker_id: 1
      humio_host_id: 0
  children:
    zookeepers:
      hosts:
        humio.example.com:
    kafkas:
      hosts:
        humio.example.com:
    humios:
      hosts:
        humio.example.com:
```

Save it as `humio.yml` and don't forget to replace all `humio.example.com` with the actual name of your Humio hostname or IP.

Now deploy the Humio with

```bash
$ ansible-playbook --inventory-file=humio.yml cluster.yml
```

After the playbook has finished, give Humio a few moments to start up, and access it at port `8080` on the host.

## Deploy a Humio cluster

A real high availability has 3 or 5 Zookeeper and Kafka nodes, so make sure you have an odd number of hosts in the `zookeepers` and `kafkas` groups. Humio can run on those machines as well.

The [Example Inventory](example_inventory.yml) file contains an example inventory with 8 nodes in all. Grap a copy of the file and tweak it to your own setup. Finally deploy a cluster with the following command:

```bash
$ ansible-playbook --inventory-file=humio.yml cluster.yml
```

With a cluster successfully installed, we highly recommend putting a sticky http load balancer in front of Humio.
