# Projet 05 — SOC maison avec Wazuh

## Contexte

*Kaloum Distribution* veut une visibilité centralisée sur la sécurité de ses serveurs et de ses postes. Le but est de mettre en place un **SIEM/XDR open source (Wazuh)** qui collecte les journaux, détecte les comportements suspects et vérifie la conformité des machines.

Ce projet pose la base de la partie **sécurité défensive** du homelab : les projets 06 à 10 s'appuient sur lui.

## Objectifs

- Déployer un serveur **Wazuh** (indexer + manager + dashboard) en machine virtuelle
- Enrôler des agents sur une VM **Windows** et une VM **Linux**
- Activer la **surveillance d'intégrité des fichiers** (FIM)
- Activer la **détection de vulnérabilités** et l'**audit de configuration** (SCA, référentiel CIS)
- Comprendre une alerte : règle, niveau, décodeur, champs extraits

## Architecture du lab (VM)

| VM | Système | Rôle | Adresse | Ressources mini |
|----|---------|------|---------|-----------------|
| WAZUH | Ubuntu Server 22.04/24.04 | Wazuh all-in-one | 10.1.40.20 | 2 vCPU, 4 Go RAM, 50 Go |
| SRV-WIN | Windows Server 2022 (évaluation) | Serveur / futur contrôleur AD | 10.1.40.30 | 2 vCPU, 4 Go RAM |
| SRV-LIN | Ubuntu Server | Serveur web / SSH | 10.1.40.40 | 1 vCPU, 1 Go RAM |
| PC-ADMIN | Windows 10/11 (évaluation) | Poste utilisateur | DHCP (10.1.10.0/24) | 2 vCPU, 3 Go RAM |

Le plan d'adressage reprend celui du [projet 01](../01-reseau-entreprise/) : les serveurs sont dans le VLAN 40 (10.1.40.0/24). Les VM tournent sur VirtualBox ou VMware en réseau interne, et peuvent être allumées une par une si la RAM est limitée.

## Schéma

![Schéma du lab](schema.png)

## Étapes de réalisation

1. Créer la VM WAZUH et installer Wazuh avec le script d'installation officiel (mode *all-in-one*)
2. Accéder au dashboard et changer les mots de passe par défaut
3. Installer et enrôler l'agent Wazuh sur SRV-WIN, SRV-LIN et PC-ADMIN
4. Configurer la FIM (`<syscheck>`) sur des dossiers sensibles (`/etc`, `C:\Windows\System32\drivers\etc`, un dossier « partage compta »)
5. Activer le module de détection de vulnérabilités
6. Lancer un scan SCA et relever le score CIS de chaque machine
7. Générer des événements de test et observer les alertes

## Tests de validation

- [ ] Les 3 agents apparaissent « Active » dans le dashboard
- [ ] Modification d'un fichier surveillé → alerte FIM
- [ ] 5 échecs de connexion SSH sur SRV-LIN → alerte d'authentification
- [ ] Échecs d'ouverture de session Windows (événement 4625) → alerte
- [ ] Liste des vulnérabilités (CVE) remontée pour chaque agent
- [ ] Score SCA (CIS) relevé pour chaque machine → sert de point de départ au [projet 09](../09-durcissement/)

Les captures sont dans [`captures/`](captures/) et les fichiers de configuration (`ossec.conf`, règles locales) dans [`configs/`](configs/).

## Ce que j'ai appris

*À compléter pendant la réalisation.*

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*

## Améliorations prévues

- Simuler des attaques et vérifier leur détection → projet 06
- Enrichir les journaux Windows avec Sysmon → projet 07
- Ajouter la détection réseau avec Suricata → projet 08
