# Assignment 37 – SRX Routing Traffic Between VLANs

Konfiguration til SRX240 i JSON: `r1-config.json`. Bygger videre på [Assignment 36](https://github.com/mwsk75996/ass36network) med de samme porte og VLANs.

## Routeren

- **R1 / SRX-C1** – switcher og router på samme boks. De to VLANs fra Assignment 36, SALES og OPERATIONS, har nu hver sit virtuelle interface (`vlan.10` og `vlan.20`), som er gateway for PC'erne. SRX-C1 router mellem dem.
- **Zoner:** `vlan.10` ligger i zone SALES og `vlan.20` i zone OPERATIONS. Begge tillader ping til SRX'en.
- **Policies:** OPERATIONS → SALES tillader ping og HTTP. SALES → OPERATIONS afvises. Det er samme regler som i Pers base config.
- Testet med to PC'er: PC1 på SALES og PC2 på OPERATIONS.

## Ændringer fra Assignment 36

| | Assignment 36 | Assignment 37 |
|---|---|---|
| Routing mellem VLANs | Nej | Ja, via `vlan.10` og `vlan.20` |
| Gateway på PC'erne | Ingen | 192.168.10.1 og 192.168.20.1 |
| Zoner og policies | Ingen | SALES og OPERATIONS, se ovenfor |
| PC'er | Tre, PC5 som DHCP-server | To, begge statisk |
| DHCP | dnsmasq på PC5 | Ingen |

Assignmentet foreslår `vlan.5` og `vlan.6`. Vi har nummereret de virtuelle interfaces efter VLAN ID (10 og 20), så de er nemmere at koble sammen.

## Adresser (LLD)

| VLAN | ID | Netværk | Port | Enhed | Adresse |
|---|---|---|---|---|---|
| SALES | 10 | 192.168.10.0/24 | vlan.10 | SRX-C1 gateway | 192.168.10.1/24 |
| SALES | 10 | 192.168.10.0/24 | ge-0/0/6 | PC1 | 192.168.10.10/24 (statisk) |
| SALES | 10 | 192.168.10.0/24 | ge-0/0/7 | – | ubrugt |
| OPERATIONS | 20 | 192.168.20.0/24 | vlan.20 | SRX-C1 gateway | 192.168.20.1/24 |
| OPERATIONS | 20 | 192.168.20.0/24 | ge-0/0/3 | PC2 | 192.168.20.5/24 (statisk) |
| OPERATIONS | 20 | 192.168.20.0/24 | ge-0/0/4 | – | ubrugt |
| OPERATIONS | 20 | 192.168.20.0/24 | ge-0/0/5 | – | ubrugt |

Fuld LLD i [LLD.md](LLD.md).

## Filer

| Fil | Indhold |
|---|---|
| `r1-config.json` | SRX-konfigurationen (JSON) |
| `set-commands.md` | Samme konfiguration som set-kommandoer |
| `HLD.pdf` / `topology.jpg` | HLD med zoner, VLANs og test-PC'er |
| `LLD.md` | Interfaces, adresser, zoner og policies |
| `testplan.md` / `testplan.png` | Testplan, resultater og forklaring af show-kommandoer |
| `srx-output.txt` | Råt output fra SRX-C1 |

## Test (kort)

11/11 tests bestået: porte og MAC-adresser i rigtige VLANs, gateways og routingtabel, PC2 (OPERATIONS) kan pinge og hente webside fra PC1 (SALES), port 8080 afvises, og PC1 kan ikke nå OPERATIONS (`deny_all_traffic` hits). Se `testplan.md` for detaljer og forklaring af `show route terse` og `show ethernet-switching`-kommandoerne.
