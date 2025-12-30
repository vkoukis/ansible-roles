# Role: ipv6-disable

This role disables IPv6 on all interfaces of a host, by configuring `sysctl`,
and restarting the `systemd-sysctl` service.

To use this role, just assign it to a specific host.

To revert the effect of this role, set the `ipv6_disabled` host variable to
`false`, and re-run Ansible.
Alternatively, remove file `/etc/sysctl.d/99-ipv6.conf` on the host and restart
the `systemd-sysctl` service.

## Role variables

* `ipv6_disable`: Disable IPv6 on all interfaces if true, enable IPv6
  otherwise. Default: true.

## Alternative implementation

The most robust way to ensure no IPv6 traffic is ever possible to reach the
node or originate from the node is to disable IPv6 at the kernel level, by
passing the `ipv6.disable=1` command-line argument. This disables IPv6
completely, meaning there are no IPv6-related configuration parameters under
`/proc`, and applications cannot even create IPv6 sockets in userspace.

However, this has a few disadvantages:

* It requires a reboot.
* Applications may fail when IPv6 does not even exist in the kernel. For
  example, the default NGINX installation of Debian binds to the wildcard IPv6
  address, which means a simple `apt install nginx` fails.

For these reasons, this role prefers to use a sysctl-based approach to disable
IPv6 by default on all interfaces, including the loopback interface.

## Verify

Here is a one-liner to verify the IPv6-related state of all network interfaces on a system:

```bash
for i in /proc/sys/net/ipv6/conf/*/disable_ipv6; do echo $i $(cat $i); done
```

## NetworkManager

NetworkManager seems to interfere with sysctl settings when configuring specific
connections. If you are using NetworkManager, you may need to set `ipv6.method=disabled`
to connections it manages.
