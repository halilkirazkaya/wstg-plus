## WSTG-INFO-04 — Énumération des applications sur le serveur web

L'énumération d'applications est le processus d'identification de toutes les applications web et de tous les services hébergés sur un seul serveur web ou une seule adresse IP. Étant donné que les serveurs web modernes hébergent souvent plusieurs hôtes virtuels (Virtual Hosts), des environnements de pré-production (staging) hérités ou des interfaces d'administration sur des ports et des chemins non-standard, le fait de ne pas identifier ces actifs cachés laisse une partie importante de la surface d'attaque non testée. Les attaquants utilisent des techniques telles que les transferts de zone DNS, le Brute Force d'hôtes virtuels et les recherches DNS inversées (reverse IP lookups) pour découvrir ces points d'entrée oubliés ou obscurcis, qui sont fréquemment moins sécurisés que l'application de production principale.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Informationnel / Faible |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Outils :** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Plusieurs adresses IP ou alias DNS sont-ils associés à l'infrastructure cible ?
- [ ] Non — seuls une adresse IP unique et un enregistrement A existent  
- [ ] Oui — plusieurs adresses IP ou alias **sont** identifiés, élargissant le périmètre (scope)  

### Le serveur web héberge-t-il plusieurs hôtes virtuels (vhosts) sur la même adresse IP ?
- [ ] Non — seul le domaine principal est desservi par le serveur  
- [ ] Oui — des hôtes virtuels supplémentaires sont identifiés mais l'accès **n'est pas possible** (ex : 403 Forbidden)  
- [ ] Oui — des hôtes virtuels cachés, de développement ou de pré-production (staging) **sont** identifiés et accessibles  

### Les applications sont-elles hébergées sur des ports non-standard ?
- [ ] Non — seuls les ports standards (80/443) sont ouverts  
- [ ] Oui — des ports non-standard sont ouverts mais n'hébergent **pas** d'applications web  
- [ ] Oui — des applications web supplémentaires **sont** identifiées sur des ports non-standard (ex : 8080, 8443, 9000)  

### Des applications distinctes sont-elles mappées sur des chemins URI spécifiques ?
- [ ] Non — une seule application dessert l'intégralité du répertoire racine  
- [ ] Oui — plusieurs applications **sont** découvertes via l'énumération de répertoires (ex : `/api`, `/portal`, `/backup`)  

### Les services de reconnaissance tiers révèlent-ils des applications historiques ou secondaires ?
- [ ] Non — aucune découverte significative via Shodan, Censys ou Wayback Machine  
- [ ] Oui — des instantanés historiques ou des scans de services révèlent des applications qui **sont** actuellement actives mais non liées  

---