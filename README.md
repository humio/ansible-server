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
humio_http_bind: "{{ ansible_eth0.ipv4.address }}"
humio_config:
  all: |
    AUTHENTICATION_METHOD=byproxy
    AUTH_BY_PROXY_HEADER_NAME=name-of-http-header
  "0": |
    HUMIO_HTTP_BIND={{ ansible_eth0_0.ipv4.address }}
  "1": |
    HUMIO_HTTP_BIND={{ ansible_eth0_1.ipv4.address }}
zookeeper_hosts:
  - ip: "{{ ansible_default_ipv4.address }}"
kafka_hosts:
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
