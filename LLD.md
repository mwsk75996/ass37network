# LLD – Assignment 37 (SRX Routing Traffic Between VLANs)

Enhederne svarer til [HLD'en](HLD.pdf).

## Interfaces og adresser

| Enhed | Interface | VLAN | Netværk | Adresse | Zone |
|---|---|---|---|---|---|
| R1 / SRX-C1 | vlan.10 | SALES (10) | 192.168.10.0/24 | 192.168.10.1/24 | SALES |
| R1 / SRX-C1 | vlan.20 | OPERATIONS (20) | 192.168.20.0/24 | 192.168.20.1/24 | OPERATIONS |
| R1 / SRX-C1 | ge-0/0/6 | SALES (10), untagged | – | – | – |
| R1 / SRX-C1 | ge-0/0/7 | SALES (10), untagged | – | ubrugt | – |
| R1 / SRX-C1 | ge-0/0/3 | OPERATIONS (20), untagged | – | – | – |
| R1 / SRX-C1 | ge-0/0/4 | OPERATIONS (20), untagged | – | ubrugt | – |
| R1 / SRX-C1 | ge-0/0/5 | OPERATIONS (20), untagged | – | ubrugt | – |
| PC1 | NIC → ge-0/0/6 | SALES | 192.168.10.0/24 | 192.168.10.10/24, GW 192.168.10.1 | – |
| PC2 | NIC → ge-0/0/3 | OPERATIONS | 192.168.20.0/24 | 192.168.20.5/24, GW 192.168.20.1 | – |

## VLANs

| VLAN | ID | l3-interface | Porte |
|---|---|---|---|
| SALES | 10 | vlan.10 | ge-0/0/6, ge-0/0/7 |
| OPERATIONS | 20 | vlan.20 | ge-0/0/3, ge-0/0/4, ge-0/0/5 |

## Routing

SRX-C1 har kun direkte forbundne netværk. Der er ingen statiske ruter.

| Destination | Next-hop |
|---|---|
| 192.168.10.0/24 | vlan.10 (direkte) |
| 192.168.20.0/24 | vlan.20 (direkte) |

## Security policies

| Fra zone | Til zone | Policy | Applikationer | Handling |
|---|---|---|---|---|
| OPERATIONS | SALES | Allow_HTTP_and_ping | junos-http, junos-icmp-ping | permit |
| SALES | OPERATIONS | deny_all_traffic | any | deny |

Host-inbound-traffic: begge zoner tillader ping til SRX'en.
