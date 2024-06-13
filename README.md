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

Example variables
------------
```
ipset_files:
  - name: country_nl
    type: "hash:net"
    state: present
    file: "country_nl.netset"

ipset_rules:
  - name: my-ipspace
    type: "hash:net"
    state: present
    addresses:
      - 1.2.3.0/22
      - 2.3.4.5/32

firewall:
  input_policy: "deny"  # Deny incoming traffic & log. Not by policy! DROP = by policy
  output_policy: "ACCEPT"  # allow outgoing traffic by default
  forward_policy: "deny"  # Deny incoming traffic & log. Not by policy! DROP = by policy
  log: true  # Log policy dropped traffic
  rulesv6:
    - comment: "Allow ICMP"
      proto: ipv6-icmp
  rules:
    - comment: "Allow ICMP"
      proto: icmp
    - comment: "Allow ssh"
      src_ip: "1.2.3.4/32"
      dst_port: 22
    - comment: Allow port 9000-9005 from dutch ip space
      dst_port: 9000:9005
      proto: tcp
      src_set: country_nl
```

Example Playbook
----------------
```
- hosts: all
  roles:
    - firewall
```

License
-------

GPLv2

Author Information
------------------
Cees Moerkerken