# Vérification et tests

Cocher chaque test une fois validé et ajouter la capture dans `captures/`.

## 1. VLAN et trunk (SW1)

| Commande | Résultat attendu |
|---|---|
| `show vlan brief` | VLAN 10/20/30/99/999 avec les bons ports |
| `show interfaces trunk` | G0/1 en trunk, VLAN natif 99, VLAN autorisés 10,20,30,99 |
| `show port-security interface fa0/1` | Port security activé, 1 MAC sticky |

- [ ] Test validé

## 2. DHCP et inter-VLAN

| Test | Résultat attendu |
|---|---|
| PC VLAN 10 en DHCP | Adresse en 192.168.10.11+ |
| `show ip dhcp binding` (R1) | Un bail par PC |
| Ping PC VLAN 10 → PC VLAN 20 | Succès (routage inter-VLAN) |

- [ ] Test validé

## 3. OSPF

| Commande | Résultat attendu |
|---|---|
| `show ip ospf neighbor` (R1) | R2 (2.2.2.2) en état FULL |
| `show ip route ospf` (R2) | Routes O vers 192.168.10/20/30/99 et O*E2 0.0.0.0/0 |
| Ping PC site distant → PC VLAN 20 | Succès |

- [ ] Test validé

## 4. NAT / PAT

| Test | Résultat attendu |
|---|---|
| Ping PC VLAN 10 → 8.8.8.8 | Succès |
| `show ip nat translations` (R1) | Traductions vers 203.0.113.2 |
| Ping PC site distant → 8.8.8.8 | Succès (traduit par R1) |

- [ ] Test validé

## 5. ACL Invités

| Test | Résultat attendu |
|---|---|
| PC VLAN 30 obtient une IP en DHCP | Succès |
| Ping PC VLAN 30 → 8.8.8.8 | Succès |
| Ping PC VLAN 30 → PC VLAN 10 | Échec (bloqué) |
| `show access-lists ACL_INVITES` | Compteurs de correspondances sur les lignes deny |

- [ ] Test validé

## 6. Administration SSH

| Test | Résultat attendu |
|---|---|
| `ssh -l admin 192.168.99.2` depuis un PC VLAN 10 | Connexion à SW1 |
| `ssh -l admin 192.168.10.1` depuis un PC VLAN 20 | Refusé par l'ACL ADMIN_SSH |
| Telnet vers R1 | Refusé (`transport input ssh`) |

- [ ] Test validé

## Dépannage courant

- Pas d'IP DHCP : vérifier `show interfaces trunk` et que les sous-interfaces sont actives (`show ip interface brief`).
- OSPF pas FULL : vérifier les masques génériques et que G0/1 n'est pas passive.
- Pas d'accès Internet : vérifier `ip nat inside/outside` sur les bonnes interfaces.
