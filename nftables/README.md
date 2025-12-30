# Role: nftables

This role supports configuring a host to run nftables for firewalling.

This role follows the "logic-only" philosophy: the role contains the automation
code, while your inventory and templates contain the specific firewall rules.
This ensures the role is reusable across any project or environment.

## Features

* **Strictly Generic**: No environment-specific rules or ports are hardcoded inside the role.
* **Pre-deployment Validation**: Uses `nft -c -f` to validate configuration syntax before overwriting `/etc/nftables.conf`.
* **Atomic Changes**: Handlers only trigger a service restart if the configuration is valid and has changed.

## Role Variables

The role relies on a single per-host variable to determine which configuration
to apply:

| Variable | Default | Description |
| :--- | :--- | :--- |
| `nftables_config_file` | None | The relative path (from the playbook root) to the nftables template. |

## Recommended Project Layout

To maintain the generic nature of the role, store your host-specific or
group-specific templates in a central `templates/` directory at the root of
your repository:

```text
.
├── group_vars/
│   └── webservers.yml       # Define nftables_config_file here
├── host_vars/
│   └── db-01.yml            # Or override here for specific hosts
├── roles/
│   └── nftables/            # This role
└── templates/
    └── nftables/            # Your actual rule files
        ├── web_rules.conf.j2
        └── db_rules.conf.j2
```
