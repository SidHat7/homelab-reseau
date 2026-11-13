# Homelab Réseau & Sécurité défensive

Série de projets réseau et cybersécurité défensive (*blue team*), construits autour d'une même infrastructure d'entreprise fictive qui s'enrichit d'un projet à l'autre. La partie réseau est réalisée dans **Cisco Packet Tracer** puis **GNS3**, la partie sécurité en **machines virtuelles**.

**Entreprise fictive :** *Kaloum Distribution*, une PME avec un siège et une agence reliés par un lien WAN.

## Projets

### Réseau

| # | Projet | Technologies | Statut |
|---|--------|-------------|--------|
| 01 | [Réseau d'entreprise sur 2 sites](01-reseau-entreprise/) | VLAN, 802.1Q, routage inter-VLAN, DHCP, OSPF, NAT/PAT | 🚧 En cours |
| 02 | Haute disponibilité | HSRP, EtherChannel (LACP), Rapid-PVST+ | ⏳ À venir |
| 03 | VPN site à site | IPsec, pfSense, GNS3 | ⏳ À venir |
| 04 | Durcissement des équipements | SSH, AAA, port-security, DHCP snooping, ACL, Syslog, NTP | ⏳ À venir |

### Sécurité défensive (blue team)

| # | Projet | Technologies | Statut |
|---|--------|-------------|--------|
| 05 | [SOC maison avec Wazuh](05-soc-wazuh/) | SIEM/XDR Wazuh, FIM, détection de vulnérabilités, SCA (CIS) | ⏳ À venir |
| 06 | [Détection d'attaques](06-detection-attaques/) | Kali, Nmap, Hydra, Atomic Red Team, MITRE ATT&CK | ⏳ À venir |
| 07 | [Sysmon et règles Sigma](07-sysmon-sigma/) | Active Directory, Sysmon, Sigma, règles Wazuh | ⏳ À venir |
| 08 | [IDS réseau avec Suricata](08-ids-suricata/) | pfSense, Suricata (IDS/IPS), ET Open, Wireshark | ⏳ À venir |
| 09 | [Durcissement et conformité](09-durcissement/) | CIS, ANSSI, Lynis, PingCastle, GPO, LAPS | ⏳ À venir |
| 10 | [Réponse à incident](10-reponse-incident/) | NIST SP 800-61, Volatility, chronologie, rapport, IOC | ⏳ À venir |

## Compétences mises en œuvre

- Conception d'un plan d'adressage IPv4 structuré et segmentation par VLAN
- Configuration d'équipements Cisco IOS (routeurs, switchs de niveau 2 et 3)
- Routage dynamique (OSPF) et accès Internet (NAT/PAT)
- Supervision de sécurité avec un SIEM et détection d'intrusion réseau
- Écriture de règles de détection et correspondance avec MITRE ATT&CK
- Durcissement de systèmes Windows, Linux et Active Directory
- Analyse et réponse à incident, rédaction de rapports
- Documentation technique et validation par des tests

## Organisation de chaque projet

```
XX-nom-du-projet/
├── README.md        # contexte, objectifs, schéma, adressage, tests, difficultés
├── topologie.pkt    # fichier Packet Tracer (projets réseau)
├── schema.png       # schéma réseau
├── configs/         # configurations (show running-config, règles, fichiers de config)
└── captures/        # preuves de fonctionnement
```

## Outils

Cisco Packet Tracer · GNS3 · VirtualBox / VMware · pfSense · Wazuh · Suricata · Sysmon · Kali Linux · Wireshark · draw.io · Git

> Toutes les attaques des projets de sécurité sont réalisées uniquement dans ce lab isolé, sur mes propres machines virtuelles.
