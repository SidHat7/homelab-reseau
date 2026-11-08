# Homelab Réseau

Série de projets réseau réalisés dans **Cisco Packet Tracer** puis **GNS3**, construits autour d'une même infrastructure d'entreprise fictive qui s'enrichit d'un projet à l'autre.

**Entreprise fictive :** *Kaloum Distribution*, une PME avec un siège et une agence reliés par un lien WAN.

## Projets

| # | Projet | Technologies | Statut |
|---|--------|-------------|--------|
| 01 | [Réseau d'entreprise sur 2 sites](01-reseau-entreprise/) | VLAN, 802.1Q, routage inter-VLAN, DHCP, OSPF, NAT/PAT | 🚧 En cours |
| 02 | Haute disponibilité | HSRP, EtherChannel (LACP), Rapid-PVST+ | ⏳ À venir |
| 03 | VPN site à site | IPsec, pfSense, GNS3 | ⏳ À venir |
| 04 | Durcissement des équipements | SSH, AAA, port-security, DHCP snooping, ACL, Syslog, NTP | ⏳ À venir |

## Compétences mises en œuvre

- Conception d'un plan d'adressage IPv4 structuré et segmentation par VLAN
- Configuration d'équipements Cisco IOS (routeurs, switchs de niveau 2 et 3)
- Routage dynamique (OSPF) et accès Internet (NAT/PAT)
- Documentation technique et validation par des tests

## Organisation de chaque projet

```
XX-nom-du-projet/
├── README.md        # contexte, objectifs, schéma, adressage, tests, difficultés
├── topologie.pkt    # fichier Packet Tracer
├── schema.png       # schéma réseau
├── configs/         # configurations des équipements (show running-config)
└── captures/        # preuves de fonctionnement
```

## Outils

Cisco Packet Tracer · GNS3 · draw.io · Git
