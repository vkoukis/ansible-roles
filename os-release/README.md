# Role: os-release

This role configures the package manager to follow a specific OS release.
It also supports upgrading OS packages to their latest versions automatically.
It currently supports APT-based systems, e.g., Debian/Ubuntu.


## Role variables

The names of all variables are case-insensitive.

* `os_family`: Must be `debian`, since this role only supports Debian-based
  distributions using APT, for now. In the future, you will be able to set
  this to `redhat`, for example.
* `os_distribution`: Must be `debian`, since this role only supports Debian
  for now. In the future, you will be able to set this to `ubuntu`, for
  example.
* `os_release`: For Debian-based systems, you can set it to a specific release
  codename (e.g., `bookworm`) or the generic name of Debian distribution
  (`stable`, `testing`, `unstable`).

There are also family- and distribution-specific variables.
All of these have sensible defaults.
Currently, they are:

* **Debian/Ubuntu family:**
  * `os_debian_remove_sources_list`: This role installs `*.sources` file under
    `/etc/apt/sources.list.d`, following the new DEB822 sources.list format.
    See [SourcesList](https://wiki.debian.org/SourcesList) for more context
    (default: `true`)
  * `os_debian_autoremove`: Whether to automatically remove dependencies
    that are no longer required.
    (default: `true`)
  * `os_debian_dist_upgrade`: Whether to automatically upgrade all OS packages.
    (default: `true`)
  * `os_debian_components:` A list of components to activate in APT.
    (default: `main`, `contrib`, `non-free`, `non-free-firmware`)
  * `os_debian_src:` Whether to configure repositories for
    [Debian source packages](https://wiki.debian.org/Packaging/SourcePackage)
    so `apt source` works. (default: `true`)
  * **Debian:**
    * `os_debian_security`: Whether to enable the [Debian
      security](https://wiki.debian.org/DebianSecurity) repository.
      (default: `true`)
    * `os_debian_updates`: Whether to enable the [Debian
      updates](https://wiki.debian.org/StableUpdates) repository.
      (default: `true`)
    * `os_debian_proposed_updates`: Whether to enable the [Debian proposed
      updates](https://wiki.debian.org/StableProposedUpdates) repository.
      (default: `true`)
    * `os_debian_backports`: Whether to enable the [Debian
      backports](https://wiki.debian.org/Backports) repository.
      (default: `true`)
    * `os_debian_debug`: Whether to enable the [Debian debug
      packages](https://wiki.debian.org/AutomaticDebugPackages) repositories.
      (default: `false`)
