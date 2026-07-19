Role: ping
==========

This role pings an Ansible host to ensure it is up and running, and can accept
SSH connections. It also confirms its actual hostname matches the expected
hostname, as discovered by Ansible.

If `expected_hostname` is set, the role checks that the discovered hostname
matches it. Otherwise, it falls back to checking against `inventory_hostname`.

This is useful when you intentionally use an inventory hostname that differs
from the actual hostname of the host.

For more information, see the documentation on the
[ansible.builtin.ping](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html)
module.

Role variables
--------------

* `expected_hostname`: Optional. The expected actual hostname of the host. If
  not set, the check falls back to `inventory_hostname`.
