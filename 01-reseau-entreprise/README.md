# Projet 01 — Réseau d'entreprise sur 2 sites

## Contexte

*Kaloum Distribution* dispose d'un **siège** (direction, comptabilité, serveurs) et d'une **agence**. Les deux sites doivent être reliés, segmentés par service, et accéder à Internet par le siège.

## Objectifs

- Segmenter chaque site en VLAN par service
- Assurer le routage inter-VLAN avec **deux méthodes** : switch de niveau 3 (siège) et *router-on-a-stick* (agence)
- Distribuer les adresses par DHCP avec **deux méthodes** : serveur centralisé + `ip helper-address` (siège) et pool sur routeur (agence)
- Relier les sites avec **OSPF** (zone 0)
- Donner l'accès à Internet via **NAT/PAT** sur le routeur du siège

## Schéma

![Schéma réseau](schema.png)

## Matériel (Packet Tracer)

| Équipement | Modèle | Rôle |
|-----------|--------|------|
| R1-SIEGE | ISR4331 | Routeur du siège, OSPF, NAT, sortie Internet |
| R2-AGENCE | ISR4331 | Routeur de l'agence, router-on-a-stick, DHCP |
| R-FAI | ISR4331 | Simule le fournisseur d'accès Internet |
| MLS-SIEGE | 3560-24PS | Switch cœur de niveau 3 (SVI, inter-VLAN, OSPF) |
| SW-SIEGE-1 | 2960-24TT | Switch d'accès du siège |
| SW-AGENCE-1 | 2960-24TT | Switch d'accès de l'agence |
| SRV-DHCP-DNS | Server-PT | DHCP + DNS du siège (VLAN 40) |
| SRV-WEB-EXT | Server-PT | Site web « sur Internet » |
| PC / Laptop | PC-PT | Postes clients |

## VLAN

| ID | Nom | Usage |
|----|-----|-------|
| 10 | ADMIN | Direction et administration |
| 20 | COMPTA | Comptabilité |
| 30 | INVITES | Wi-Fi / visiteurs |
| 40 | SERVEURS | Serveurs (siège uniquement) |
| 99 | GESTION | Administration des équipements |
| 999 | NATIF | VLAN natif des trunks (inutilisé) |

## Plan d'adressage

Convention : `10.<site>.<vlan>.0/24` — site 1 = siège, site 2 = agence. La passerelle est toujours en `.1`.

### Siège (site 1)

| VLAN | Réseau | Passerelle | Plage DHCP |
|------|--------|-----------|-----------|
| 10 ADMIN | 10.1.10.0/24 | 10.1.10.1 | .100 – .200 |
| 20 COMPTA | 10.1.20.0/24 | 10.1.20.1 | .100 – .200 |
| 30 INVITES | 10.1.30.0/24 | 10.1.30.1 | .100 – .200 |
| 40 SERVEURS | 10.1.40.0/24 | 10.1.40.1 | statique |
| 99 GESTION | 10.1.99.0/24 | 10.1.99.1 | statique |

### Agence (site 2)

| VLAN | Réseau | Passerelle | Plage DHCP |
|------|--------|-----------|-----------|
| 10 ADMIN | 10.2.10.0/24 | 10.2.10.1 | .100 – .200 |
| 20 COMPTA | 10.2.20.0/24 | 10.2.20.1 | .100 – .200 |
| 30 INVITES | 10.2.30.0/24 | 10.2.30.1 | .100 – .200 |
| 99 GESTION | 10.2.99.0/24 | 10.2.99.1 | statique |

### Liaisons point à point

| Liaison | Réseau | Adresses |
|---------|--------|----------|
| MLS-SIEGE ↔ R1-SIEGE | 10.0.0.0/30 | .1 (MLS) / .2 (R1) |
| R1-SIEGE ↔ R2-AGENCE (WAN) | 10.0.0.4/30 | .5 (R1) / .6 (R2) |
| R1-SIEGE ↔ R-FAI | 203.0.113.0/30 | .2 (R1) / .1 (FAI) |
| R-FAI ↔ SRV-WEB-EXT | 198.51.100.0/24 | .1 (FAI) / .10 (serveur) |

### Adresses fixes

| Équipement | Adresse |
|-----------|---------|
| SRV-DHCP-DNS | 10.1.40.10 |
| MLS-SIEGE (gestion) | 10.1.99.1 |
| SW-SIEGE-1 (gestion) | 10.1.99.11 |
| SW-AGENCE-1 (gestion) | 10.2.99.11 |

## Étapes de réalisation

1. Placer et câbler les équipements, nommer chaque appareil
2. Créer les VLAN et configurer les ports d'accès et les trunks 802.1Q
3. Configurer les SVI sur MLS-SIEGE et activer `ip routing`
4. Configurer les sous-interfaces sur R2-AGENCE (router-on-a-stick)
5. Configurer le DHCP (serveur + `ip helper-address` au siège, pools sur R2 à l'agence)
6. Configurer OSPF zone 0 sur MLS-SIEGE, R1-SIEGE et R2-AGENCE
7. Configurer la route par défaut vers le FAI, sa diffusion dans OSPF et le NAT/PAT sur R1
8. Tester et documenter

## Tests de validation

- [ ] Chaque PC reçoit une adresse DHCP dans le bon VLAN — `ipconfig`
- [ ] Ping inter-VLAN sur le même site
- [ ] Ping entre le siège et l'agence
- [ ] Voisins OSPF établis — `show ip ospf neighbor`
- [ ] Routes OSPF apprises — `show ip route ospf`
- [ ] Accès au site web externe depuis un PC des deux sites
- [ ] Traductions NAT visibles — `show ip nat translations`
- [ ] Trunks corrects — `show interfaces trunk`

Les captures sont dans [`captures/`](captures/) et les configurations complètes dans [`configs/`](configs/).

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*

## Améliorations prévues

- Haute disponibilité de la passerelle et des liens → projet 02
- Isolation du VLAN INVITES par ACL et durcissement des équipements → projet 04
