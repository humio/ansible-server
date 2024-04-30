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

`humio_total_memory_mb` can be used to override the memory used by Humio. This defaults to
using all memory available on the machine. The JVM memory settings will be determined automatically
based on that number using our [recommended formula](https://docs.humio.com/operations-guide/configuration/basic-configuration/jvm-configuration/#java-memory-options).
If you want more fine-grained control, you can set the JVM memory options individually. See
the `defaults/main.yml` file for which variables to override.

Example Playbook
----------------

To run a singlenode Humio instance pointing to existing zookeeper and kafka instances.

```yaml
- hosts: humios
  become: true
  vars:
    kafka_hosts:
      - ip: "172.17.0.2"
    zookeeper_hosts:
      - ip: "172.17.0.2"
  roles:
    - role: humio.server
```

License
-------

Apache 2.0

Author Information
------------------

If you're having issues with this module please to not hesitate to reach out to us on the #ansible channel on our [Slack community team](https://community.humio.com/).
