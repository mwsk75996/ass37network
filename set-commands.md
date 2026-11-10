# Set-kommandoer – Assignment 37

Indlæses på SRX-C1 med `load set terminal`, efterfulgt af Ctrl-D, `commit check` og `commit`.

```
set system host-name SRX-C1
set system services ssh

set interfaces ge-0/0/3 description "OPERATIONS - PC2"
set interfaces ge-0/0/3 unit 0 family ethernet-switching vlan members OPERATIONS
set interfaces ge-0/0/4 description "OPERATIONS - unused"
set interfaces ge-0/0/4 unit 0 family ethernet-switching vlan members OPERATIONS
set interfaces ge-0/0/5 description "OPERATIONS - unused"
set interfaces ge-0/0/5 unit 0 family ethernet-switching vlan members OPERATIONS
set interfaces ge-0/0/6 description "SALES - PC1"
set interfaces ge-0/0/6 unit 0 family ethernet-switching vlan members SALES
set interfaces ge-0/0/7 description "SALES - unused"
set interfaces ge-0/0/7 unit 0 family ethernet-switching vlan members SALES

set interfaces vlan unit 10 description "SALES gateway"
set interfaces vlan unit 10 family inet address 192.168.10.1/24
set interfaces vlan unit 20 description "OPERATIONS gateway"
set interfaces vlan unit 20 family inet address 192.168.20.1/24

set vlans SALES vlan-id 10
set vlans SALES l3-interface vlan.10
set vlans OPERATIONS vlan-id 20
set vlans OPERATIONS l3-interface vlan.20

set security zones security-zone SALES interfaces vlan.10 host-inbound-traffic system-services ping
set security zones security-zone OPERATIONS interfaces vlan.20 host-inbound-traffic system-services ping

set security policies from-zone OPERATIONS to-zone SALES policy Allow_HTTP_and_ping match source-address any
set security policies from-zone OPERATIONS to-zone SALES policy Allow_HTTP_and_ping match destination-address any
set security policies from-zone OPERATIONS to-zone SALES policy Allow_HTTP_and_ping match application junos-http
set security policies from-zone OPERATIONS to-zone SALES policy Allow_HTTP_and_ping match application junos-icmp-ping
set security policies from-zone OPERATIONS to-zone SALES policy Allow_HTTP_and_ping then permit
set security policies from-zone SALES to-zone OPERATIONS policy deny_all_traffic match source-address any
set security policies from-zone SALES to-zone OPERATIONS policy deny_all_traffic match destination-address any
set security policies from-zone SALES to-zone OPERATIONS policy deny_all_traffic match application any
set security policies from-zone SALES to-zone OPERATIONS policy deny_all_traffic then deny
```
