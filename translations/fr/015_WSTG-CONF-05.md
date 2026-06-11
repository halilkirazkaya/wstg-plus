## WSTG-CONF-05 — Énumération des interfaces d'administration de l'infrastructure et de l'application

Les interfaces d'administration représentent le plan de contrôle de gestion d'une application et de son infrastructure sous-jacente, accordant souvent un accès privilégié aux configurations système, aux données utilisateur et aux opérations côté serveur. Les attaquants privilégient l'énumération de ces interfaces via le Brute Force de répertoires, l'analyse du fichier `robots.txt` ou l'observation de schémas d'URL prévisibles pour trouver des points d'entrée pouvant manquer d'authentification robuste ou reposer sur des identifiants par défaut. La découverte d'un panneau d'administration exposé augmente considérablement le risque de compromission complète du système, de modification non autorisée des données ou d'interruption de service. Ces interfaces se trouvent fréquemment sur des ports non standard ou des sous-domaines, ce qui en fait des cibles de choix pour les scanners automatisés et la reconnaissance manuelle.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si l'interface permet des modifications de configuration à l'échelle du système, la gestion des utilisateurs ou l'exfiltration de données sans authentification multi-facteurs (MFA).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Les interfaces d'administration sont-elles découvrables via le Fuzzing de répertoires ou la reconnaissance ?
- [ ] Non — aucune interface d'administration n'est exposée ou découvrable  
- [ ] Oui — les interfaces sont découvrables mais l'accès **est restreint** aux plages d'adresses IP internes  
- [ ] Oui — les interfaces **sont** découvrables et accessibles publiquement  

### L'authentification est-elle appliquée sur les portails d'administration découverts ?
- [ ] Oui — l'authentification multi-facteurs (MFA) **est appliquée**  
- [ ] Oui — l'authentification à facteur unique **est appliquée**  
- [ ] Non — aucune authentification n'est requise pour accéder à l'interface *(Critique)*  

### Les chemins d'administration sont-ils masqués ou non standard pour empêcher la découverte ?
- [ ] Oui — les chemins utilisent un nommage non standard, aléatoire ou non prévisible  
- [ ] Non — les chemins utilisent des conventions de nommage courantes (ex : `/admin`, `/manager`, `/console`) et **peuvent** être facilement devinés  

### L'interface donne-t-elle accès à des fonctionnalités système ou applicatives sensibles ?
- [ ] Non — l'interface est un tableau de bord d'état en "lecture seule" avec un impact minimal  
- [ ] Oui — l'interface **peut** modifier la configuration de l'application ou les données utilisateur  
- [ ] Oui — l'interface **peut** exécuter des commandes au niveau système ou effectuer la gestion de bases de données  

---