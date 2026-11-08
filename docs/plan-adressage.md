# Plan d'adressage

## Réseaux

| Réseau | VLAN / Lien | Adresse | Passerelle | Plage DHCP |
|---|---|---|---|---|
| Admin | VLAN 10 | 192.168.10.0/24 | 192.168.10.1 | .11 – .254 |
| Technique | VLAN 20 | 192.168.20.0/24 | 192.168.20.1 | .11 – .254 |
| Invités | VLAN 30 | 192.168.30.0/24 | 192.168.30.1 | .11 – .254 |
| Management | VLAN 99 (natif) | 192.168.99.0/24 | 192.168.99.1 | – (statique) |
| Site distant | LAN R2 | 192.168.40.0/24 | 192.168.40.1 | .11 – .254 |
| Liaison R1–R2 | WAN privé | 10.0.12.0/30 | – | – |
| Liaison R1–ISP | Internet | 203.0.113.0/30 | – | – |
| « Internet » | Loopback ISP | 8.8.8.8/32 | – | – |

## Adresses des équipements

| Équipement | Interface | Adresse |
|---|---|---|
| R1 | G0/0.10 / .20 / .30 / .99 | 192.168.10.1 / 20.1 / 30.1 / 99.1 |
| R1 | G0/1 (vers R2) | 10.0.12.1/30 |
| R1 | G0/2 (vers ISP) | 203.0.113.2/30 |
| R2 | G0/0 (LAN) | 192.168.40.1/24 |
| R2 | G0/1 (vers R1) | 10.0.12.2/30 |
| ISP | G0/0 | 203.0.113.1/30 |
| SW1 | VLAN 99 | 192.168.99.2/24 |

## Câblage

| De | Vers | Rôle |
|---|---|---|
| R1 G0/0 | SW1 G0/1 | Trunk 802.1Q |
| R1 G0/1 | R2 G0/1 | Lien OSPF |
| R1 G0/2 | ISP G0/0 | Sortie Internet |
| R2 G0/0 | SW2 G0/1 | LAN distant |

## Ports de SW1

| Ports | VLAN |
|---|---|
| Fa0/1 – Fa0/5 | 10 Admin |
| Fa0/6 – Fa0/10 | 20 Technique |
| Fa0/11 – Fa0/15 | 30 Invités |
| Fa0/16 – Fa0/24, G0/2 | Désactivés (VLAN 999 « parking ») |
| G0/1 | Trunk vers R1 |
