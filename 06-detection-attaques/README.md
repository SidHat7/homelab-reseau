# Projet 06 — Détection d'attaques (attaquer pour mieux défendre)

## Contexte

Un SIEM n'a de valeur que s'il détecte réellement les attaques. Dans ce projet, je joue les deux rôles : **attaquant** depuis une VM Kali, puis **analyste SOC** qui vérifie que chaque action est détectée, l'analyse et propose une contre-mesure.

Toutes les attaques sont menées **uniquement dans le lab isolé**, sur mes propres machines virtuelles.

## Objectifs

- Simuler des techniques d'attaque courantes et les relier au référentiel **MITRE ATT&CK**
- Vérifier pour chacune si une alerte est générée dans Wazuh ([projet 05](../05-soc-wazuh/))
- Créer des **règles personnalisées** quand une attaque n'est pas détectée
- Proposer une **remédiation** pour chaque scénario

## Architecture du lab (VM)

| VM | Rôle | Adresse |
|----|------|---------|
| KALI | Attaquant | 10.1.30.50 (VLAN INVITES) |
| WAZUH | SIEM | 10.1.40.20 |
| SRV-LIN | Cible Linux | 10.1.40.40 |
| SRV-WIN | Cible Windows | 10.1.40.30 |

Outil complémentaire : **Atomic Red Team** pour exécuter des techniques ATT&CK de façon reproductible sur Windows.

## Scénarios

| # | Attaque | Outil | Technique ATT&CK | Détectée ? | Remédiation |
|---|---------|-------|------------------|-----------|-------------|
| 1 | Scan de ports | Nmap | T1046 — Network Service Discovery | | |
| 2 | Brute force SSH | Hydra | T1110.001 — Password Guessing | | |
| 3 | Brute force RDP / SMB | Hydra / CrackMapExec | T1110 — Brute Force | | |
| 4 | Création d'un compte local | `net user` | T1136.001 — Create Local Account | | |
| 5 | Ajout au groupe Administrateurs | `net localgroup` | T1098 — Account Manipulation | | |
| 6 | PowerShell encodé | Atomic Red Team | T1059.001 — PowerShell | | |
| 7 | Persistance par tâche planifiée | `schtasks` | T1053.005 — Scheduled Task | | |
| 8 | Effacement des journaux | `wevtutil cl` | T1070.001 — Clear Windows Event Logs | | |

## Méthode pour chaque scénario

1. **Attaque** — commande lancée et résultat obtenu
2. **Détection** — alerte Wazuh (ID de règle, niveau, capture)
3. **Analyse** — ce que l'alerte montre et comment je confirme que c'est malveillant
4. **Correction** — règle personnalisée si rien n'a été détecté
5. **Remédiation** — mesure qui empêche ou limite l'attaque (fail2ban, verrouillage de compte, GPO…)

Le détail de chaque scénario est dans [`captures/`](captures/) et les règles personnalisées dans [`configs/`](configs/) (`local_rules.xml`).

## Bilan de couverture

*À compléter : nombre de scénarios détectés nativement, après règles personnalisées, et non détectés.*

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*

## Améliorations prévues

- Améliorer la visibilité Windows avec Sysmon et des règles Sigma → projet 07
- Détecter le scan et le brute force côté réseau avec Suricata → projet 08
