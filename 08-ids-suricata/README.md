# Projet 08 — Détection d'intrusion réseau avec Suricata

## Contexte

Les agents Wazuh voient ce qui se passe **sur** les machines. Il faut aussi voir ce qui passe **sur le réseau**. Ce projet ajoute un **IDS réseau (Suricata)** entre les zones du réseau de *Kaloum Distribution*, et fait le lien avec les projets réseau 01 à 04.

## Objectifs

- Mettre en place un pare-feu **pfSense** (ou OPNsense) qui sépare LAN, serveurs et DMZ
- Activer **Suricata** sur les interfaces internes avec les règles **ET Open**
- Envoyer les alertes Suricata vers Wazuh ([projet 05](../05-soc-wazuh/))
- Analyser le trafic suspect avec **Wireshark** (captures PCAP)
- Passer d'un mode **détection (IDS)** à un mode **blocage (IPS)** sur une règle choisie

## Architecture du lab (VM)

| Zone | Réseau | Contenu |
|------|--------|---------|
| WAN | NAT de l'hyperviseur | Accès Internet |
| LAN | 10.1.10.0/24 | PC-ADMIN |
| SERVEURS | 10.1.40.0/24 | WAZUH, SRV-WIN, SRV-LIN |
| INVITES | 10.1.30.0/24 | KALI (attaquant) |
| DMZ | 172.16.0.0/24 | Serveur web exposé (future appli) |

pfSense : 2 vCPU, 2 Go RAM minimum avec Suricata activé.

## Schéma

![Schéma du lab](schema.png)

## Étapes de réalisation

1. Installer pfSense et créer les interfaces des zones
2. Écrire les règles de pare-feu entre les zones (INVITES isolé, DMZ joignable seulement sur 80/443)
3. Installer le paquet Suricata et activer les règles ET Open
4. Envoyer les journaux Suricata (EVE JSON) vers Wazuh via syslog
5. Lancer les scénarios de test et analyser les captures dans Wireshark
6. Activer le mode blocage sur une règle et vérifier son effet

## Scénarios de test

| # | Trafic généré | Depuis → vers | Alerte attendue | Résultat |
|---|--------------|---------------|-----------------|----------|
| 1 | Scan Nmap SYN | KALI → SERVEURS | ET SCAN | |
| 2 | Brute force SSH | KALI → SRV-LIN | ET SCAN / brute force SSH | |
| 3 | Requête vers un domaine malveillant connu | PC-ADMIN → Internet | ET DNS / ET MALWARE | |
| 4 | Téléchargement du fichier de test EICAR | PC-ADMIN → Internet | Détection EICAR | |
| 5 | Tentative d'injection SQL sur le serveur web | KALI → DMZ | ET WEB_SERVER | |
| 6 | Accès d'INVITES vers SERVEURS | KALI → 10.1.40.0/24 | Bloqué par le pare-feu | |

## Tests de validation

- [ ] Alertes Suricata visibles dans pfSense
- [ ] Alertes Suricata visibles dans Wazuh
- [ ] Un PCAP par scénario analysé dans Wireshark (dans [`captures/`](captures/))
- [ ] Mode IPS : le trafic de test est bloqué

Les règles de pare-feu exportées et la configuration de Suricata sont dans [`configs/`](configs/).

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*

## Améliorations prévues

- Comparer avec un VPN IPsec entre sites → projet 03
- Déployer une application web réelle dans la DMZ et la surveiller
