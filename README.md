firewall
=========

Ansible role to configure firewalling.

Comments
------------
If you need POSTROUTEING, PREROUTING (NAT) or FORWARD rules, don't use UFW. UFW only defines input rules. forward is allowed always and NAT is defined in "before.rules" wich is effectively plain IPTables.

UFW also depends on you deleting rules you no longer need. It does not define a definitive state of your IP Tables without flushing it. So of you want to fully define your firewall with this role, use IP Tables (the default).

Ipset is also supported but not documented.

Role Variables
--------------
Firewall configuration is a merged result of the following dicts:
- firewall
- firewall1
- firewall2
- firewall3
- firewall4
You can use this to configure defaults on multiple levels like all, different (sub)groups and the host. The higher the number, te higher the precedence. Lists (like rules) wil be merged.

Dependencies
------------


Example Playbook
----------------

- hosts: all
  gather_facts: false
  roles:
    - config_parser

License
-------

GPLv2

Author Information
------------------
Cees Moerkerken