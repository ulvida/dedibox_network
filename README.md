Network configuration for online.net proxmox distrib 
=========

dedibox_network configures the network, particularly IPv6, for a dedibox, server in the cloud provider Scaleway/online.net, installed with proxmox distribution.

The role automates the tasks described in [dedibox documentation for DHCPv6 client](https://www.scaleway.com/en/docs/dedibox-network/ipv6/how-to/configure-dhcpv6/) and those descibed in [IPv6 on dedibox running Linux](https://www.scaleway.com/en/docs/dedibox-network/ipv6/how-to/configure-ipv6-linux/).

Requirements
------------

To have a server at online.net and install it with proxmox. Thhe role may workor be easily adaptable for debian or any debian derivated GNU/Linux distribution.


Role Variables
--------------
The role expects the following variable , describing the network interfaces configuration: 

```yaml
dedibox_net_interfaces:
- id: net0
  name: vmbr0
  # Do define at least the IPv4!
  ip4: 192.168.33.10
  netmask4: 255.255.255.0
  gw4: 192.168.33.1
  ip6: 2001:db8::10
  gw6: 2001:db8:6a::1
  netmask6: 64
  # If we define a netif_bridge_port, the interface is defined as a bridge
  bridge_port: eth0
  This is the DUID parameter given in online.net web console
  DHCPv6_DUID: XX:YY:ZZ:TT:UU:WW:QQ:KK:EE:DD
- id: net1 
  dhcp: yes  # interface with DCHP, previous parameters aren't take into account
  mtu: 9000  # sets the MTU of the interface, for DHCP or static
```

By default, the role will [configure IPv6 using systemd](https://www.scaleway.com/en/docs/dedibox-network/ipv6/how-to/configure-ipv6-linux/#how-to-configure-ipv6-on-debian), but it didn't work for me. If you set: 

```yaml
dedibox_use_systemd: false
```
it will [configure IPv6 withouyt using systemd](https://www.scaleway.com/en/docs/dedibox-network/ipv6/how-to/configure-ipv6-linux/#how-to-configure-ipv6-on-debian-without-systemd)


Dependencies
------------

no dependencies

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml

- hosts: servers
  roles:
      - { role: username.rolename, x: 42 }
  vars:
    dedibox_net_interfaces:
    - id: net0
      name: vmbr0
      # Do define at least the IPv4!
      ip4: 192.168.33.10
      netmask4: 255.255.255.0
      gw4: 192.168.33.1

License
-------

(c) Daniel Viña Ulriksen (@ulvida)
licensed under GPLv3

Author Information
------------------

@ulvida
