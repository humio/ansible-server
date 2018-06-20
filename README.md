ansible-humio
=========

Installer for Humio.
Current features
- Launch one Humio instance per CPU socket

Requirements
------------

A running Zookeeper and Kafka installation.

Role Variables
--------------

```yaml
humio_host_id: 0
humio_public_url: "http://{{ ansible_default_ipv4.address }}"
humio_socket_bind: "0.0.0.0"
humio_http_bind: "0.0.0.0"
zookeeper_hosts:
- ip: "{{ ansible_default_ipv4.address }}"
kafka_hosts:
zookeeper_hosts:
- ip: "{{ ansible_default_ipv4.address }}"
```

Dependencies
------------

Java must be installed. We recommend [humio.ansible-java galaxy module](https://github.com/humio/ansible-java).

Example Playbook
----------------

Ro run a singlenode Humio instance

```yaml
- hosts: humios
  become: true
  roles:
    - role: humio.ansible-java
    - role: AnsibleShipyard.ansible-zookeeper
    - role: humio.ansible-kafka
    - role: humio.ansible-humio
```

License
-------

Apache

Author Information
------------------

If you're having issues with this module please to not hesitate to reach out to us on the #ansible channel on our [Slack community team](https://community.humio.com/). 
