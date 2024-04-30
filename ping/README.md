Role: ping
==========

This role pings an Ansible host to ensure it is up and running, and can accept
SSH connections. It also confirms its inventory name matches its actual
hostname, as discovered by Ansible.

For more information, see the documentation on the
[ansible.builtin.ping](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html)
module.

Host variables
--------------

* `inventory_hostname`: The hostname of the Ansible host as configured in the inventory.
