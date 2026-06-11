## WSTG-INFO-01 — Effectuer une découverte via les moteurs de recherche et une reconnaissance pour détecter les fuites d'informations

La découverte par moteur de recherche consiste à exploiter les moteurs de recherche publics, les pages en cache et les services d'indexation pour identifier les informations que l'organisation cible a involontairement exposées. Les attaquants utilisent des opérateurs de recherche avancés (Google Dorks) et des services d'indexation tiers pour découvrir des fichiers sensibles, des répertoires, des portails de connexion, des messages d'erreur et des métadonnées qui ne devraient pas être accessibles publiquement. Cette phase de reconnaissance est critique car elle ne nécessite aucune interaction directe avec la cible, ce qui la rend pratiquement indétectable tout en révélant potentiellement des points d'entrée à haute valeur tels que des panneaux d'administration (admin panels) exposés, des fichiers de configuration, des dumps de base de données et de la documentation interne.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Outils :** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Des fichiers ou répertoires sensibles sont-ils indexés par les moteurs de recherche ?
- [ ] Non — aucun contenu sensible trouvé dans les résultats des moteurs de recherche  
- [ ] Oui — des fichiers de configuration, des sauvegardes ou des documents internes **sont** indexés *(Critique)*  

### Le Google Dorking révèle-t-il des interfaces d'administration ou de connexion exposées ?
- [ ] Non — les panneaux d'administration et les pages de connexion ne sont **pas** indexés  
- [ ] Oui — les panneaux d'administration **sont** indexés mais nécessitent une authentification multifacteur robuste  
- [ ] Oui — les panneaux d'administration **sont** indexés et accessibles publiquement **sans** contrôles suffisants  

### Les versions en cache des pages exposent-elles des données sensibles qui ont été supprimées depuis ?
- [ ] Non — aucune donnée sensible trouvée dans les instantanés (snapshots) en cache  
- [ ] Oui — les pages en cache contiennent des identifiants, des adresses IP internes ou des informations sensibles  

### Le fichier `robots.txt` divulgue-t-il involontairement des chemins sensibles ?
- [ ] Non — `robots.txt` ne révèle **pas** de structures de répertoires sensibles  
- [ ] Oui — `robots.txt` liste des chemins sensibles qui facilitent la reconnaissance de l'attaquant  
- [ ] Aucun fichier `robots.txt` n'existe  

### Les services tiers (Shodan, Censys, Wayback Machine) révèlent-ils une exposition historique ou actuelle ?
- [ ] Non — aucune découverte significative via les services d'indexation tiers  
- [ ] Oui — des instantanés historiques ou des scans de services révèlent des métadonnées sensibles ou des bannières de service  

---