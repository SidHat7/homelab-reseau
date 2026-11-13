# Projet 09 — Durcissement et conformité des systèmes

## Contexte

Détecter les attaques ne suffit pas : il faut aussi **réduire la surface d'attaque**. Ce projet applique les recommandations de référence (**CIS**, **ANSSI**) aux serveurs Windows et Linux et à l'Active Directory de *Kaloum Distribution*, et **mesure** le gain avant/après.

Il complète le projet 04 (durcissement des équipements réseau) côté systèmes.

## Objectifs

- Mesurer le niveau de sécurité initial de chaque système
- Appliquer des mesures de durcissement ciblées et justifiées
- Mesurer à nouveau et présenter un **comparatif avant/après**
- Documenter chaque mesure : risque couvert, impact, méthode

## Outils

| Outil | Cible | Usage |
|-------|-------|-------|
| Wazuh SCA ([projet 05](../05-soc-wazuh/)) | Windows, Linux | Score de conformité CIS |
| Lynis | Linux | Audit et indice de durcissement |
| PingCastle | Active Directory | Score de risque du domaine |
| GPO | Windows / AD | Application des mesures |

## Mesures prévues

### Linux (SRV-LIN)

- SSH : connexion par clé, `PermitRootLogin no`, port et utilisateurs autorisés
- Pare-feu `ufw` / `nftables` en refus par défaut
- `fail2ban` contre le brute force
- Mises à jour automatiques de sécurité
- `auditd` et désactivation des services inutiles

### Windows et Active Directory (SRV-WIN, PC-ADMIN)

- Stratégie de mots de passe et de verrouillage de compte
- Désactivation de SMBv1, LLMNR et NetBIOS
- **LAPS** pour les mots de passe administrateur locaux
- Pare-feu Windows activé par profil
- Stratégie d'audit avancée (journaux utiles au SOC)
- Groupe « Protected Users » pour les comptes à privilèges

## Résultats avant / après

| Système | Outil | Avant | Après |
|---------|-------|-------|-------|
| SRV-LIN | Lynis (indice) | | |
| SRV-LIN | Wazuh SCA (CIS) | | |
| SRV-WIN | Wazuh SCA (CIS) | | |
| kaloum.local | PingCastle (score, plus bas = mieux) | | |

Les rapports avant/après sont dans [`captures/`](captures/) et les scripts / exports de GPO dans [`configs/`](configs/).

## Vérification

- [ ] Rejouer les attaques du [projet 06](../06-detection-attaques/) : lesquelles échouent maintenant ?
- [ ] Vérifier qu'aucun service légitime n'est cassé

## Difficultés rencontrées et solutions

*À compléter pendant la réalisation.*

## Améliorations prévues

- Automatiser le durcissement Linux avec Ansible
