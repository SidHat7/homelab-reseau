# Projet 07 — Sysmon et règles de détection Sigma

## Contexte

Les journaux Windows par défaut ne suffisent pas pour détecter beaucoup d'attaques (ligne de commande complète, processus parent, connexions réseau…). **Sysmon** enrichit fortement ces journaux. Les règles **Sigma** permettent d'écrire des détections dans un format standard, indépendant du SIEM.

C'est le quotidien d'un analyste SOC : écrire, tester et affiner des règles de détection.

## Objectifs

- Installer **Sysmon** avec une configuration reconnue (SwiftOnSecurity ou Olaf Hartong)
- Remonter les journaux Sysmon dans Wazuh ([projet 05](../05-soc-wazuh/))
- Monter un petit **Active Directory** (domaine `kaloum.local`) sur SRV-WIN
- Écrire et tester mes propres **règles Sigma**, puis les convertir pour Wazuh
- Mesurer les faux positifs et affiner les règles

## Architecture du lab (VM)

| VM | Rôle | Adresse |
|----|------|---------|
| SRV-WIN | Contrôleur de domaine `kaloum.local` + Sysmon | 10.1.40.30 |
| PC-ADMIN | Poste joint au domaine + Sysmon | DHCP (10.1.10.0/24) |
| WAZUH | SIEM | 10.1.40.20 |
| KALI | Attaquant | 10.1.30.50 |

## Étapes de réalisation

1. Promouvoir SRV-WIN en contrôleur de domaine et joindre PC-ADMIN au domaine
2. Installer Sysmon avec la configuration choisie (versionnée dans [`configs/`](configs/))
3. Ajouter le canal `Microsoft-Windows-Sysmon/Operational` dans l'agent Wazuh
4. Vérifier la réception des événements Sysmon (ID 1, 3, 10, 11, 13…)
5. Écrire les règles Sigma, les convertir (sigma-cli / pySigma) et les intégrer dans Wazuh
6. Rejouer les attaques correspondantes et valider la détection

## Règles de détection

| Règle | Événements utilisés | Technique ATT&CK | Testée | Faux positifs |
|-------|-------------------|------------------|--------|---------------|
| Accès à la mémoire de LSASS | Sysmon 10 | T1003.001 — LSASS Memory | | |
| PowerShell avec commande encodée | Sysmon 1 | T1059.001 — PowerShell | | |
| Ajout d'un compte au groupe « Admins du domaine » | Windows 4728 / 4732 | T1098 — Account Manipulation | | |
| Création de service suspect | Windows 7045 | T1543.003 — Windows Service | | |
| Persistance via clé Run | Sysmon 13 | T1547.001 — Registry Run Keys | | |
| Office qui lance un shell | Sysmon 1 (processus parent) | T1204.002 — Malicious File | | |

Les règles Sigma sont dans [`regles-sigma/`](regles-sigma/), les versions converties pour Wazuh dans [`configs/`](configs/).

## Ce que j'ai appris

*À compléter pendant la réalisation.*

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*

## Améliorations prévues

- Audit de sécurité de l'Active Directory avec PingCastle → projet 09
- Utiliser ces détections dans un scénario d'incident complet → projet 10
