Role: wireguard
===============

This role supports configuring a host to run WireGuard.

To use this role, define a `wireguard` host variable as a list.
Each item in the list defines a different WireGuard interface.
Here is an example snippet of defining this variable in your YAML inventory:

```yaml
wireguard:
- name: wg-{{ inventory_hostname }}
  address: 192.168.1.1
  listen_port: 51820
  private_key: {{ wireguard_private_key }}
  peers:
  - public_key: DkUTqErr8rl7nGw25OCjh10806wp3d6claV1MehTumk=
    # public_key: '{{ hostvars["peer_host"].wireguard_public_key }}'
    persistent_keepalive: 15
    endpoint: 1.2.3.4:51820
    allowed_ips:
    - 192.168.1.2
    - 192.168.10.0/24
```

Only the private key is security-sensitive, so we refer to a value
(`wireguard_private_key`) which we assume comes from an
[Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html).

Also note how we can optionally choose to define a single host-level variable
for the public key of a host, so we can refer to it consistently when using it
as a peer in the WireGuard interfaces of other hosts.

Prerequisites
-------------

This role requires the `wg` utility on the node where Ansible itself runs.
You will probably need it anyway, to generate private/public keypairs when
configuring new hosts into the system, and setting host variables for them.


Role variables
--------------

This part of the documentation is pending.


Known issues
------------

Systemd's `wg-quick(8)` will not reconfigure the WireGuard interface when using
a dynamic DNS endpoint for a peer, and its IP changes, see
[open issue](https://github.com/systemd/systemd/issues/9911).
