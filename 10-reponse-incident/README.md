# Projet 10 — Réponse à incident

## Contexte

Ce projet rassemble tout le homelab dans un **scénario d'incident complet** chez *Kaloum Distribution* : une compromission est simulée, puis je la traite comme le ferait une équipe SOC / CSIRT, de la première alerte jusqu'au rapport final.

## Objectifs

- Dérouler les phases de la réponse à incident (modèle NIST SP 800-61) : préparation, détection et analyse, confinement, éradication, rétablissement, retour d'expérience
- Reconstituer une **chronologie** de l'attaque à partir des journaux (Wazuh, Sysmon, Suricata)
- Réaliser une analyse **forensic** de base (mémoire et journaux)
- Rédiger un **rapport d'incident** clair, pour un public technique et non technique

## Scénario (simulé dans le lab)

1. Un poste de la comptabilité ouvre une pièce jointe piégée (simulation, fichier de test)
2. Une commande PowerShell encodée télécharge un outil sur le poste
3. L'attaquant récupère des identifiants et se connecte au serveur
4. Il crée un compte administrateur et met en place une persistance
5. Il tente d'effacer les journaux

## Outils

| Outil | Usage |
|-------|-------|
| Wazuh ([projet 05](../05-soc-wazuh/)) | Alertes et recherche dans les journaux |
| Sysmon + règles Sigma ([projet 07](../07-sysmon-sigma/)) | Détail des processus |
| Suricata ([projet 08](../08-ids-suricata/)) | Trafic réseau |
| FTK Imager / DumpIt | Capture de la mémoire |
| Volatility 3 | Analyse de la mémoire |
| Wireshark | Analyse des PCAP |

## Déroulé

1. **Détection** — première alerte reçue et qualification (vrai ou faux positif ?)
2. **Analyse** — périmètre : machines, comptes et techniques ATT&CK concernés
3. **Confinement** — isolement du poste (règle pfSense / agent Wazuh), désactivation du compte
4. **Éradication** — suppression de la persistance et du compte créé
5. **Rétablissement** — remise en service et surveillance renforcée
6. **Retour d'expérience** — ce qui a manqué et les améliorations à apporter

## Chronologie de l'incident

| Heure | Machine | Événement | Source | Technique ATT&CK |
|-------|---------|-----------|--------|------------------|
| | | | | |

## Livrables

- [`rapport/`](rapport/) : rapport d'incident (résumé pour la direction + partie technique)
- [`captures/`](captures/) : alertes, résultats Volatility, PCAP
- [`configs/`](configs/) : règles ajoutées après le retour d'expérience
- Liste des **indicateurs de compromission** (IOC) : adresses IP, hachages, noms de fichiers

## Ce que j'ai appris

*À compléter pendant la réalisation.*

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*
