Role: multihomed-ssh
====================

This role supports declaring multiple SSH endpoints for the same host and tries
them out in order, until one of them succeeds; then, it updates the variables
for this host so every subsequent connection to it uses the settings that
worked.

This is useful when you run Ansible in different locations, in which case you
may have different ways of accessing the same host over SSH, e.g., using just
its IP, or an internal hostname, or via an SSH jumphost.

To use this role, assign it to one or more hosts, and set the `multihomed`
variable for each one of them, e.g.:

```yaml
multihomed:
# Use the internal DNS name of the host
- ansible_host: myhost.internal.domain
# Use an IP directly, e.g., because DNS is not yet operational
- ansible_host: 192.168.1.1
# Use port forwarding on localhost
- ansible_host: localhost
  ansible_port: 2223
# Use a number of jumphosts, in a chain
- ansible_host: myhost.internal.domain
  ansible_ssh_common_args: '-J user1@proxyjump1:2222,user2@proxyjump2'
```

It is recommended you assign this role as early as possible in your playbook,
to allow it to set host variables appropriately, so the rest of your playbook
can SSH into the host seamlessly.

Make sure to also prevent Ansible from attempting to gather facts when
assigning this role, since this role does not need them, and Ansible will
probably not be able to SSH into the host anyway, until this role has run:

```yaml
- name: Discover SSH endpoints for multihomed hosts
  hosts: all
  gather_facts: false
  roles:
  - multihomed
```

Role variables
--------------

* `multihomed`: A list of dictionaries, each being an alternative set of
  connection-related host variables which the role will try in order. For each
  item in the list, the role will combine it with the existing host variables,
  and try to connect to the host. If it succeeds, it will override the contents
  of the in-memory inventory to combine this specific dict with the host's
  variables for the remainder of this run, and move on.

More context
------------

This interesting [GitHub issue](https://github.com/ansible/proposals/issues/97)
is relevant.
