FreeBSD-PF
==========

PF firewall basic configuration.


**DISCLAIMER**: This role has been created for my personal use and come with
no warranty. As it manipulates the firewall, you have a high chance of being
locked out of your server if you don't know what you are doing!



Requirements
------------

FreeBSD with PF firewall

Role Variables
--------------


- `pf_ext_iface`, the external interface, 
  default to `{{ ansible_default_ipv4.device }}`
- `pf_rules`, a set of rule to apply in addition to the built in
  "proection" rules, default to an empty array
- `pf_ssh_whitelist`, a whitelist of network in CIDR notation for SSH
  connections, default to an empty array. It is important to whitelist
  your ansible machine as otherwise it will be blocked by the "ssh_abuse"
  table.

Example Playbook
----------------

An example configuration that allow connection on port 80 and whitelist
the local network for ssh.

    - hosts: servers
      roles:
      - role: kuon.freebsd-pf
        pf_rules:
        - pass in on $ext_if proto tcp from any to any port www flags S/SA synproxy state
        pf_ssh_whitelist:
        - 172.16.216.0/24
         
**NOTE**: The above example opens port 80 and activate SYNPROXY, depending on
what you are doing (HAProxy...), it may break things. Be sure to understand
the rules you add.

License
-------

MIT

Author Information
------------------

Ansible role by Nicolas Goy @kuon.

Rule file based on ["Click Death Squad"](https://sites.google.com/site/clickdeathsquad/Home/cds-bsdfirewall) rule set.
