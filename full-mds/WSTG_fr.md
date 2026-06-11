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

## WSTG-INFO-02 — Fingerprint Web Server

Le fingerprinting (prise d'empreinte) de serveur Web est le processus d'identification du type de logiciel spécifique, de sa version et du système d'exploitation sous-jacent d'un serveur Web cible. Les attaquants effectuent cette reconnaissance pour identifier des vulnérabilités connues, telles que des CVE non corrigées ou des faiblesses de configuration, propres à certaines versions d'Apache, Nginx, IIS ou d'autres technologies de serveur. Cela est généralement réalisé en analysant les en-têtes de réponse HTTP (par exemple, `Server`, `X-Powered-By`), en examinant les comportements uniques des protocoles ou en observant les informations divulguées via les pages d'erreur et les fichiers par défaut. Un fingerprinting réussi permet à un attaquant d'adapter sa stratégie d'exploitation, passant d'un scan générique à des attaques à haute probabilité de succès contre des failles architecturales connues.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **État du Test** | Non réalisé |
| **Sévérité** | Informationnel / Faible* |

> *La sévérité devient Élevée si le fingerprinting révèle une version de serveur obsolète ou en fin de vie (EOL) présentant des vulnérabilités critiques connues.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Outils :** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Des chaînes d'identification sont-elles présentes dans les en-têtes de réponse HTTP ?
- [ ] Non — les en-têtes `Server` et `X-Powered-By` ne sont **pas présents** ou contiennent des valeurs génériques  
- [ ] Oui — les en-têtes révèlent le logiciel du serveur mais **pas** les numéros de version spécifiques  
- [ ] Oui — les en-têtes révèlent le logiciel spécifique du serveur et les numéros de version **exacts**  

### Les pages d'erreur par défaut ou les réponses générées par le système divulguent-elles des détails sur le serveur ?
- [ ] Non — des pages d'erreur personnalisées sont utilisées et **ne divulguent pas** d'informations sur le serveur  
- [ ] Oui — les pages d'erreur par défaut divulguent le nom du logiciel serveur et/ou sa version  
- [ ] Oui — les pages d'erreur divulguent les détails du serveur, la version et le **système d'exploitation sous-jacent**  

### La version du serveur est-elle divulguée via des fichiers par défaut ou des structures de répertoires ?
- [ ] Non — les fichiers d'installation par défaut, les manuels et les scripts de test ont été **supprimés**  
- [ ] Oui — les fichiers par défaut (par ex. `info.php`, `manual/`, `test.html`) sont **présents** et divulguent des données sur la version  

### Le type de serveur peut-il être déduit avec précision via une analyse de comportement unique ?
- [ ] Non — les réponses du serveur sont normalisées et le fingerprinting basé sur le comportement n'est **pas possible**  
- [ ] Oui — des comportements uniques (par ex. l'ordre des en-têtes, des codes d'erreur spécifiques ou des particularités de la pile TCP/IP) **permettent** l'identification du serveur  

### La version du serveur identifiée est-elle actuellement supportée et corrigée ?
- [ ] Oui — la version identifiée est à jour et **supportée** par l'éditeur  
- [ ] Non — la version identifiée est **obsolète** ou en **fin de vie (EOL)** avec des vulnérabilités connues

---

## WSTG-INFO-03 — Examen des métafichiers du serveur Web pour les fuites d'informations

Les métafichiers du serveur Web tels que `robots.txt`, `sitemap.xml` et `security.txt` sont destinés à guider les crawlers (robots d'indexation) et à fournir des métadonnées administratives, mais ils révèlent souvent par inadvertance des structures de répertoires sensibles ou des points de terminaison (endpoints) cachés. Les attaquants analysent ces fichiers pour énumérer la surface d'attaque de l'application, identifiant les directives "Disallow" qui pointent fréquemment vers des panneaux d'administration non liés, des répertoires de sauvegarde ou des environnements de développement. Au-delà des instructions destinées aux moteurs de recherche, les fichiers du répertoire `.well-known/` ou des fichiers comme `humans.txt` peuvent divulguer des piles technologiques (technology stacks), des informations sur les développeurs ou des contacts de sécurité organisationnels. Un examen systématique de ces fichiers permet à un testeur d'obtenir des informations structurelles et techniques sans avoir recours à une découverte agressive par force brute (Brute Force).

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Statut du test** | Non effectué |
| **Sévérité** | Faible / Informationnel* |

> *La sévérité devient "Moyenne" si les métafichiers révèlent des interfaces d'administration non authentifiées ou des sauvegardes de configuration sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Le fichier `robots.txt` existe-t-il et expose-t-il des répertoires sensibles ?
- [ ] Non — `robots.txt` n'existe **pas**  
- [ ] Oui — `robots.txt` existe mais ne contient que des chemins publics standards  
- [ ] Oui — `robots.txt` révèle des chemins de répertoires sensibles ou des interfaces d'administration  
- [ ] Oui — `robots.txt` révèle des identifiants (credentials) ou des versions spécifiques de logiciels dans les commentaires  

### Un fichier `sitemap.xml` est-il présent et révèle-t-il une structure d'application cachée ?
- [ ] Non — `sitemap.xml` n'existe **pas**  
- [ ] Oui — `sitemap.xml` est présent mais ne liste que des URL publiques  
- [ ] Oui — `sitemap.xml` liste des points de terminaison non liés ou des ressources internes qui **peuvent** être consultés  

### Les métafichiers de sécurité et d'organisation exposent-ils des détails techniques ?
- [ ] Non — aucun fichier de métadonnées trouvé à la racine ou dans `.well-known/`  
- [ ] Oui — `security.txt` ou `humans.txt` sont présents mais contiennent des informations standards  
- [ ] Oui — les métafichiers divulguent des noms d'hôtes internes, des identités de développeurs ou des détails sur la pile technologique qui **peuvent** faciliter l'ingénierie sociale ou d'autres attaques  

### Existe-t-il des métafichiers hérités ou spécifiques à un framework ?
- [ ] Non — aucun fichier spécifique à un framework (ex. : `info.php`, `composer.json`, `package.json`) trouvé  
- [ ] Oui — des fichiers comme `README.md`, `CHANGELOG.txt` ou `.DS_Store` sont présents mais nettoyés  
- [ ] Oui — des fichiers spécifiques à un framework ou à une version sont présents et **permettent** un fingerprinting technologique précis

---

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

## WSTG-INFO-05 — Examen du contenu des pages web pour détecter les fuites d'informations

L'examen du contenu des pages web implique l'inspection manuelle et automatisée des réponses de l'application, y compris les fichiers HTML, JavaScript et CSS, afin d'identifier les informations sensibles exposées involontairement aux utilisateurs. Les attaquants scrutent le code source côté client à la recherche de commentaires de développeurs, de champs de formulaire masqués, de chemins de serveurs internes, de clés API codées en dur (hardcoded) et d'informations de débogage (debugging) pouvant faciliter les phases d'exploitation ultérieures. Cette fuite d'informations (Information Leakage) survient souvent lorsque les développeurs oublient de supprimer les métadonnées ou le code de débogage avant le passage en production, révélant potentiellement des failles logiques ou des détails de l'infrastructure backend. Du point de vue d'un attaquant, cela constitue une méthode nécessitant peu d'efforts pour cartographier la structure interne de l'application et découvrir des points d'entrée négligés sans déclencher d'alertes de sécurité ni interagir avec la logique backend du serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **État du test** | Non réalisé |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité devient Élevée si des identifiants (credentials) codés en dur, des clés privées ou des variables d'environnement internes sensibles sont découverts.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Outils :** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Des commentaires de développeurs contenant des informations sensibles sont-ils présents dans le code source ?
- [ ] Non — aucun commentaire ou seulement des commentaires génériques et non sensibles trouvés  
- [ ] Oui — des commentaires existent mais ne contiennent aucune logique ou donnée sensible  
- [ ] Oui — les commentaires révèlent des chemins internes, des requêtes SQL ou des instructions administratives  
- [ ] Oui — les commentaires révèlent des identifiants (credentials) ou des notes de développeurs sensibles *(Élevée)*  

### Les champs de saisie HTML masqués contiennent-ils des données sensibles ou des informations d'état ?
- [ ] Non — aucun champ masqué trouvé ou ils ne contiennent que des jetons (tokens) non sensibles  
- [ ] Oui — des champs masqués sont utilisés pour l'état mais sont chiffrés ou signés  
- [ ] Oui — les champs masqués contiennent en clair (plaintext) des mots de passe, des rôles d'utilisateur ou des identifiants internes  

### Des secrets codés en dur, des clés API ou des adresses IP internes sont-ils présents dans les fichiers JavaScript ?
- [ ] Non — aucune chaîne sensible ou secret trouvé dans les fichiers JS  
- [ ] Oui — les fichiers JS contiennent des clés API publiques avec des restrictions appropriées  
- [ ] Oui — les fichiers JS contiennent des clés API non protégées, des adresses IP internes ou des points de terminaison (endpoints) privés  
- [ ] Oui — les fichiers JS contiennent des identifiants administratifs ou des clés privées codés en dur *(Critique)*  

### L'application révèle-t-elle des versions de framework ou des chemins de fichiers internes dans la source ?
- [ ] Non — les informations sur le framework et les chemins de fichiers sont minifiés ou obfusqués  
- [ ] Oui — les noms et versions de frameworks sont visibles mais à jour (patchés)  
- [ ] Oui — des chemins de fichiers internes ou des chemins de serveurs absolus sont exposés dans le code source  

### Le code source est-il sujet aux fuites en raison d'un manque de minification ou d'obfuscation ?
- [ ] Non — le code de production est entièrement minifié et expurgé des commentaires  
- [ ] Oui — le code est minifié mais les commentaires subsistent dans la source  
- [ ] Oui — les fichiers source maps (`.map`) sont accessibles publiquement, permettant une reconstruction complète du code source

---

## WSTG-INFO-06 — Identification des points d'entrée de l'application

L'identification des points d'entrée de l'application consiste à cartographier l'intégralité de la surface d'attaque en énumérant toutes les manières possibles pour un utilisateur ou un système externe d'interagir avec l'application. Ce processus inclut la découverte de toutes les URL, des points de terminaison API (API endpoints), des paramètres (GET, POST, basés sur les cookies), des en-têtes HTTP (HTTP headers) et des protocoles spécialisés comme WebSockets ou gRPC. Pour un testeur de pénétration, cette étape est vitale car les vulnérabilités sont souvent trouvées dans des API "fantômes" (shadow APIs) non documentées, des points de terminaison hérités (legacy endpoints) ou des structures de paramètres complexes qui ont été négligées lors du développement. Une énumération approfondie garantit qu'aucun composant de l'application ne reste une boîte noire non testée, empêchant ainsi les attaquants de trouver des chemins non surveillés vers le système.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Statut du test** | Non effectué |
| **Sévérité** | Informationnel |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Est-ce que tous les paramètres GET et POST ont été énumérés à travers l'application ?
- [ ] Oui — une énumération exhaustive via un crawling automatisé et manuel **est terminée**  
- [ ] Oui — seuls les paramètres communs ont été identifiés via un crawling standard  
- [ ] Non — les paramètres sont en grande partie **non identifiés** ou non testés  

### Les paramètres non documentés ou cachés (ex: debug, admin) sont-ils recherchés par Fuzzing ?
- [ ] Non — la découverte de paramètres cachés n'est **pas nécessaire** pour ce périmètre spécifique  
- [ ] Oui — le Fuzzing de paramètres (ex: via `Arjun`) **a été effectué** sans résultat  
- [ ] Oui — des paramètres cachés **ont été découverts** et cartographiés pour des tests ultérieurs  
- [ ] Non — la découverte de paramètres cachés **n'a pas** été effectuée  

### Toutes les méthodes HTTP prises en charge (PUT, DELETE, PATCH, etc.) ont-elles été identifiées pour chaque point de terminaison ?
- [ ] Oui — la découverte de méthodes **est appliquée** sur tous les points de terminaison découverts  
- [ ] Oui — la découverte de méthodes **est appliquée** uniquement aux points de terminaison sensibles ou à fort intérêt  
- [ ] Non — seules les méthodes standard GET et POST **sont identifiées**  

### Les points d'entrée non standard tels que WebSockets, gRPC ou les en-têtes personnalisés sont-ils cartographiés ?
- [ ] Non — l'application **n'utilise pas** de protocoles non standard ou d'en-têtes personnalisés  
- [ ] Oui — tous les points d'entrée spécifiques aux protocoles et les en-têtes personnalisés **sont identifiés**  
- [ ] Non — des points d'entrée non standard **existent** mais n'ont pas été cartographiés  

### La surface de l'API (y compris les points de terminaison versionnés comme /v1/ ou /v2/) a-t-elle été entièrement cartographiée ?
- [ ] Non — l'application **n'expose pas** d'API  
- [ ] Oui — toutes les versions d'API et tous les points de terminaison **sont identifiés** et documentés  
- [ ] Oui — seule la version actuelle de l'API en production **est identifiée**  
- [ ] Non — les points de terminaison de l'API **restent non cartographiés** ou seulement partiellement découverts

---

## WSTG-INFO-07 — Cartographier les chemins d'exécution au travers de l'application

La cartographie des chemins d'exécution implique l'identification systématique de tous les points de terminaison (endpoints) accessibles, des flux fonctionnels et des branches de prise de décision au sein d'une application web. Ce processus est essentiel pour garantir qu'aucune fonctionnalité "fantôme" — telle que du code hérité (legacy code), des interfaces de débogage (debug) ou des routes d'API non documentées — ne reste cachée à l'examen de sécurité, car celles-ci manquent souvent de contrôles de sécurité modernes. Les attaquants utilisent une combinaison de spidering automatisé et d'exploration manuelle pour visualiser la structure de l'application, découvrant souvent des vulnérabilités telles que l'accès administratif non autorisé ou des failles de logique métier (business logic flaws) au cours du processus. En corrélant les entrées aux réponses spécifiques du serveur, les testeurs peuvent identifier la logique de traitement sensible qui nécessite des tests approfondis ciblés pour assurer une couverture robuste.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Informationnel |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### L'application a-t-elle été entièrement indexée à l'aide d'un crawling automatisé et manuel ?
- [ ] Oui — une carte complète de toutes les ressources visibles et liées **est terminée**  
- [ ] Oui — le crawling automatisé **est terminé** mais l'exploration manuelle est en attente  
- [ ] Non — l'application n'a **pas** été indexée  

### Les endpoints cachés ou non documentés sont-ils identifiés via l'analyse JS ou le brute-forcing de répertoires ?
- [ ] Non — aucun chemin non documenté découvert via l'analyse par canaux auxiliaires (side-channel analysis)  
- [ ] Oui — des chemins cachés ont été découverts mais ils **ne peuvent pas** être consultés sans autorisation  
- [ ] Oui — des endpoints non documentés **sont** accessibles et fournissent des fonctionnalités sensibles *(Critique / Élevé)*  

### Les chemins d'exécution peuvent-ils être modifiés via des paramètres ou des en-têtes non standard ?
- [ ] Non — les paramètres tels que `debug`, `test` ou `admin` n'affectent **pas** le flux d'exécution  
- [ ] Oui — les paramètres modifient la sortie mais **ne contournent pas** les contrôles de sécurité  
- [ ] Oui — des paramètres modifiant le chemin **sont appliqués** et permettent de contourner la logique prévue  

### Le flux de la logique métier multi-étapes (ex : authentification multi-facteurs, paiement) est-il entièrement cartographié ?
- [ ] Oui — tous les états et transitions dans les processus multi-étapes **sont identifiés**  
- [ ] Non — les transitions complexes de la machine à états (state machine) **ne peuvent pas** être entièrement cartographiées  

### Les fichiers de documentation d'API (Swagger/OpenAPI) ou les cartes côté client (Source Maps) sont-ils exposés ?
- [ ] Non — aucune carte d'architecture ou fichier de documentation n'est exposé publiquement  
- [ ] Oui — la documentation est présente mais **est protégée** par une authentification  
- [ ] Oui — Swagger UI, OpenAPI JSON ou les JS Source Maps **sont activés** et accessibles publiquement

---

## WSTG-INFO-08 — Identification de l'empreinte du framework d'application Web (Fingerprint)

L'identification de l'empreinte (Fingerprinting) d'un framework d'application Web consiste à identifier la pile technologique spécifique et les versions logicielles utilisées pour construire et servir l'application. Les attaquants analysent les en-têtes de réponse HTTP, les cookies spécifiques au framework, les structures de fichiers prévisibles et les artefacts JavaScript uniques pour déterminer si la cible utilise des plateformes telles que React, Angular, Django ou Spring. Cette phase de reconnaissance est vitale car elle permet à un testeur de réduire la surface d'attaque aux vulnérabilités connues (CVE) et aux faiblesses de configuration courantes spécifiques à ce framework. En comprenant l'architecture sous-jacente, un attaquant peut passer de tests génériques à des stratégies d'exploitation hautement ciblées.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Statut du test** | Non effectué |
| **Sévérité** | Informatif |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Les en-têtes de réponse HTTP exposent-ils le nom ou la version du framework ?
- [ ] Non — les en-têtes tels que `X-Powered-By` ou `Server` ne sont **pas** présents ou sont assainis  
- [ ] Oui — le nom du framework est présent mais la version **est masquée**  
- [ ] Oui — le nom du framework et la version **sont tous deux exposés**  

### Les cookies spécifiques au framework ou les structures de répertoires sont-ils identifiables ?
- [ ] Non — les artefacts communs du framework (par ex., chemins `.js`, noms de cookies) sont offusqués ou renommés  
- [ ] Oui — les cookies spécifiques au framework (par ex., `JSESSIONID`, `PHPSESSID`, `csrftoken`) **sont** identifiables  
- [ ] Oui — les structures de répertoires (par ex., `/wp-content/`, `_next/static/`) **sont** visibles et confirment le framework  

### Les messages d'erreur ou les commentaires du code source divulguent-ils des détails sur le framework ?
- [ ] Non — la gestion des erreurs est générique et les commentaires sont supprimés  
- [ ] Oui — les traces de pile (stack traces) spécifiques au framework **sont activées** et visibles dans les réponses  
- [ ] Oui — le code source HTML contient des commentaires ou des balises de métadonnées identifiant le framework  

### Des vulnérabilités connues sont-elles associées à la version du framework identifiée ?
- [ ] Non — le framework identifié est à jour avec **aucune** vulnérabilité publique connue  
- [ ] Oui — la version du framework est obsolète et des CVE spécifiques **peuvent être** identifiés

---

## WSTG-INFO-09 — Identification de l'empreinte de l'application Web (Fingerprint)

L'identification de l'empreinte d'une application web (Fingerprinting) est le processus consistant à identifier le framework spécifique, le système de gestion de contenu (CMS) ou la pile technologique sous-jacente ainsi que ses versions associées. Cette phase de reconnaissance est vitale car elle permet à un attaquant de mettre en correspondance les composants identifiés avec des vulnérabilités connues (CVEs) et des exploits publics, réduisant ainsi considérablement la surface d'attaque. Les informations sont généralement recueillies à partir des en-têtes de réponse HTTP, des chemins de fichiers uniques, des noms de cookies, des métadonnées du code source HTML et des hachages cryptographiques des ressources statiques (assets). En déterminant avec précision la pile technologique, un attaquant peut passer d'un balayage automatisé générique à une exploitation hautement ciblée des faiblesses spécifiques à une version.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Faible / Informationnel |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Les en-têtes de réponse HTTP divulguent-ils la pile technologique ou les numéros de version ?
- [ ] Non — les en-têtes sensibles sont supprimés ou contiennent des valeurs génériques  
- [ ] Oui — des en-têtes par défaut comme `X-Powered-By`, `Server` ou `X-AspNet-Version` **sont** présents  
- [ ] Oui — les en-têtes divulguent des versions spécifiques et obsolètes du serveur web ou du framework applicatif  

### Le framework applicatif ou le CMS est-il identifiable via des chemins de fichiers uniques ou des conventions de nommage ?
- [ ] Non — les chemins de fichiers sont obfusqués et ne révèlent **pas** la technologie sous-jacente  
- [ ] Oui — des structures de répertoires courantes (ex. `/wp-content/`, `/_next/`, `/node_modules/`) **sont** visibles  
- [ ] Oui — des fichiers uniques comme `robots.txt`, `humans.txt` ou `sitemap.xml` contiennent des métadonnées spécifiques à la technologie  

### Des numéros de version spécifiques sont-ils exposés dans le code source HTML ou les ressources statiques ?
- [ ] Non — aucune information de version n'est trouvée dans les commentaires ou les paramètres des ressources  
- [ ] Oui — des commentaires HTML ou des balises meta (ex. `<meta name="generator" content="...">`) **sont** présents  
- [ ] Oui — des chaînes de version sont ajoutées aux noms de fichiers CSS/JS (ex. `jquery.js?v=1.12.4`) et ne **peuvent pas** être désactivées  

### Les pages d'erreur par défaut ou les interfaces d'administration révèlent-elles l'environnement logiciel ?
- [ ] Non — l'application utilise des pages d'erreur personnalisées et génériques pour tous les codes d'état  
- [ ] Oui — les pages d'erreur par défaut du serveur web ou du framework **sont** activées et divulguent des données de version  
- [ ] Oui — les portails de connexion administratifs (ex. `/wp-admin`, `/phpmyadmin`) **sont** accessibles et identifiables  

### Les cookies révèlent-ils le framework de gestion de session ?
- [ ] Non — les cookies de session utilisent des noms génériques ou personnalisés  
- [ ] Oui — des noms de cookies spécifiques à la technologie (ex. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **sont** utilisés

---

## WSTG-INFO-10 — Cartographier l'architecture de l'application

La cartographie de l'architecture de l'application consiste à identifier les technologies sous-jacentes, les composants d'infrastructure et les flux de données qui soutiennent l'application web. En analysant les en-têtes de réponse HTTP, les messages d'erreur, les formats de cookies et les extensions de fichiers, les testeurs peuvent reconstruire un schéma des serveurs web, des serveurs d'applications, des bases de données et des intégrations tierces utilisés. Cette connaissance est fondamentale pour adapter les attaques ultérieures, car elle permet de sélectionner des Exploit spécifiques à la plateforme et d'identifier les goulots d'étranglement potentiels ou les dispositifs intermédiaires mal configurés tels que les Load Balancers et les WAF. Un attaquant exploite cette carte structurelle pour passer d'un balayage générique à une exploitation ciblée de la pile logicielle spécifique et de ses composants interconnectés.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **État du test** | Non effectué |
| **Sévérité** | Informationnel / Faible* |

> *La sévérité devient Moyenne si la topologie du réseau interne ou des adresses IP privées sont divulguées.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Les technologies du serveur web et du serveur d'applications peuvent-elles être identifiées avec précision ?
- [ ] Non — aucune identification **possible** en raison d'un durcissement (Hardening) efficace ou d'une obfuscation  
- [ ] Oui — le type de technologie est connu mais les versions spécifiques **ne peuvent pas** être déterminées  
- [ ] Oui — la technologie et les versions spécifiques **sont** pleinement divulguées via les en-têtes ou les signatures de fichiers *(Informationnel)*  

### Des dispositifs intermédiaires (WAF, Load Balancers, Proxies) sont-ils détectables dans le chemin de communication ?
- [ ] Non — aucun intermédiaire n'est détecté ou ils sont totalement transparents  
- [ ] Oui — l'existence est suspectée via le timing ou le comportement, mais l'identité **n'est pas** confirmée  
- [ ] Oui — des produits spécifiques (ex: Cloudflare, Nginx, F5) **sont** identifiés via des en-têtes uniques ou leur comportement  

### L'application laisse-t-elle fuiter des informations sur sa structure de répertoires internes ou sa topologie réseau ?
- [ ] Non — les chemins internes et les IP ne sont **pas** divulgués  
- [ ] Oui — les chemins internes fuitent dans les messages d'erreur ou les Stack Traces, mais les IP internes ne le **sont pas**  
- [ ] Oui — les structures de répertoires internes et les adresses IP privées **sont** toutes deux divulguées  

### Le type et la version de la base de données backend peuvent-ils être déduits du comportement de l'application ?
- [ ] Non — les détails de la base de données **ne peuvent pas** être déterminés  
- [ ] Oui — le type de base de données (ex: PostgreSQL, MSSQL) est déduit via les messages d'erreur ou le comportement  
- [ ] Oui — le type et la version de la base de données **sont** explicitement divulgués dans les corps de réponse ou les en-têtes  

### L'application est-elle hébergée chez des fournisseurs de services cloud identifiables ou des CDN tiers spécifiques ?
- [ ] Non — l'infrastructure d'hébergement n'est **pas** identifiable  
- [ ] Oui — le fournisseur cloud (AWS, Azure, GCP) est identifié mais les services spécifiques ne le **sont pas**  
- [ ] Oui — des services cloud spécifiques (ex: buckets S3, Lambda, Azure Functions) **sont** cartographiés et accessibles

---

## WSTG-CONF-01 — Tester la configuration de l'infrastructure réseau

Le test de configuration de l'infrastructure réseau consiste à identifier les faiblesses des composants serveur et réseau sous-jacents qui supportent l'application web. Les attaquants ciblent les services mal configurés, les protocoles obsolètes et les ports ouverts inutiles pour établir un point d'appui, effectuer des déplacements latéraux ou mener une phase de reconnaissance. Les vulnérabilités courantes incluent des versions TLS non sécurisées, des bannières de service verbeuses et des interfaces d'administration exposées qui devraient être restreintes aux réseaux internes. En exploitant ces failles au niveau de l'infrastructure, un adversaire peut compromettre la confidentialité des données en transit ou obtenir un accès non autorisé au système d'exploitation hôte.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Statut du test** | Non effectué |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Outils :** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Existe-t-il des ports ouverts inutiles ou des services exposés sur l'hôte de l'application ?
- [ ] Non — seuls les ports requis (ex : 443) sont **accessibles**  
- [ ] Oui — des services supplémentaires sont exposés mais suivent une configuration **sécurisée**  
- [ ] Oui — des services non essentiels (ex : FTP, Telnet, SMB) sont **exposés** publiquement  

### Les bannières ou les en-têtes de service divulguent-ils des informations de version sensibles ?
- [ ] Non — les bannières sont supprimées ou génériques  
- [ ] Oui — les bannières révèlent le logiciel serveur (ex : Apache/nginx) mais **pas** les numéros de version  
- [ ] Oui — les bannières verbeuses révèlent des versions logicielles spécifiques, facilitant l'**Exploit**  

### La configuration TLS respecte-t-elle les meilleures pratiques de sécurité modernes ?
- [ ] Oui — seuls TLS 1.2/1.3 sont **activés** avec des suites de chiffrement fortes  
- [ ] Oui — des protocoles hérités (TLS 1.0/1.1) sont **activés**  
- [ ] Non — des chiffrements faibles ou des protocoles non sécurisés (SSLv2/v3) sont **activés**  

### Les interfaces d'administration ou de gestion sont-elles restreintes aux réseaux autorisés ?
- [ ] Oui — les interfaces (ex : SSH, RDP, panneaux de contrôle) ne sont **pas accessibles** depuis l'internet public  
- [ ] Non — les interfaces sont **accessibles** publiquement mais nécessitent une authentification multi-facteurs  
- [ ] Non — les interfaces sont **accessibles** publiquement avec une authentification à facteur unique ou des identifiants par défaut (Default Credentials)

---

## WSTG-CONF-02 — Test de la configuration de la plateforme d'application

Le test de configuration de la plateforme d'application consiste à auditer le serveur web sous-jacent, le serveur d'application et les paramètres du framework afin de s'assurer qu'ils sont durcis (hardened) contre les exploits courants. Les attaquants recherchent activement des identifiants par défaut, des vulnérabilités de plateforme non corrigées et des interfaces d'administration exposées pour obtenir un accès non autorisé ou exécuter du code. Les mauvaises configurations entraînent souvent la divulgation de variables d'environnement sensibles, de chemins système internes ou la présence d'applications d'exemple inutiles qui étendent la surface d'attaque. D'un point de vue professionnel, une plateforme mal configurée constitue un point d'entrée à haute probabilité pour obtenir un accès initial à l'environnement cible.

| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si les interfaces d'administration sont accessibles avec des identifiants par défaut ou si des scripts d'exemple permettent une exécution de code à distance (Remote Code Execution - RCE).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Des identifiants ou comptes par défaut sont-ils actifs sur la plateforme ?
- [ ] Non — tous les comptes par défaut sont **désactivés** ou les mots de passe ont été modifiés  
- [ ] Oui — des comptes par défaut existent mais ne sont **pas accessibles** depuis le réseau externe  
- [ ] Oui — des comptes par défaut sont **actifs** et accessibles via Internet *(Critique)*  

### La plateforme divulgue-t-elle des informations de version sensibles via des en-têtes ou des pages d'erreur ?
- [ ] Non — les chaînes de version sont **masquées** ou génériques  
- [ ] Oui — des informations de version détaillées **sont divulguées** dans les en-têtes HTTP (ex : `Server`, `X-Powered-By`)  
- [ ] Oui — des traces de pile (stack traces) complètes et des chemins système internes **sont exposés** dans des messages d'erreur détaillés (verbose)  

### Les interfaces d'administration ou de gestion sont-elles correctement restreintes ?
- [ ] Non — les interfaces d'administration ne sont **pas exposées**  
- [ ] Oui — des interfaces existent mais sont **restreintes** par liste blanche d'IP (IP allowlisting) ou par authentification multi-facteurs (Multi-Factor Authentication - MFA)  
- [ ] Oui — des interfaces (ex : `/admin`, `/manager`, `/console`) sont **accessibles publiquement** avec une authentification à facteur unique  

### Des fichiers d'exemple, des scripts ou de la documentation sont-ils présents sur le serveur de production ?
- [ ] Non — tous les fichiers non essentiels ont été **supprimés**  
- [ ] Oui — des fichiers d'exemple ou de la documentation **sont présents** mais ne divulguent pas d'informations sensibles  
- [ ] Oui — des scripts d'exemple (ex : `info.php`, `examples/`) **sont présents** et constituent un vecteur d'attaque direct  

### Le serveur est-il configuré pour supporter des méthodes HTTP non sécurisées ?
- [ ] Non — seuls `GET` et `POST` sont **activés**  
- [ ] Oui — des méthodes potentiellement risquées comme `PUT`, `DELETE` ou `TRACE` sont **activées** mais restreintes  
- [ ] Oui — des méthodes risquées sont **activées** et permettent la modification non autorisée de fichiers ou le vol d'identifiants

---

## WSTG-CONF-03 — Test de la gestion des extensions de fichiers pour les informations sensibles

Le test de la gestion des extensions de fichiers consiste à identifier si le serveur web ou le serveur d'application révèle des informations sensibles en servant des fichiers qui devraient être restreints ou exécutés. Les attaquants recherchent fréquemment des fichiers de sauvegarde (backup), des fichiers de configuration et des fragments de code source (ex. : `.bak`, `.old`, `.env`, `.inc`) qui pourraient être laissés à la racine du site web (web root) et servis en texte brut en raison d'une absence de mappage de gestionnaire (handler mapping) spécifique. Cette vulnérabilité est importante car elle peut mener à l'exposition d'identifiants de base de données, de clés d'API et de la logique métier interne, facilitant une exploitation plus profonde de l'infrastructure. Ces expositions se produisent généralement dans les répertoires publics, les dossiers de téléchargement (upload) ou via des paramètres de type MIME mal configurés sur le serveur.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fichiers de configuration contenant des identifiants ou l'intégralité du code source sont accessibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Les extensions de fichiers de sauvegarde ou temporaires sensibles (ex. : `.bak`, `.old`, `.swp`, `~`) sont-elles accessibles ?
- [ ] Non — le serveur renvoie 403 Forbidden ou 404 Not Found pour les extensions de sauvegarde courantes  
- [ ] Oui — le serveur autorise le téléchargement de fichiers de sauvegarde mais ils **ne contiennent pas** de données sensibles  
- [ ] Oui — le serveur autorise le téléchargement de fichiers de sauvegarde contenant des données sensibles ou du code source *(Haute)*  

### Le serveur expose-t-il des fichiers d'environnement ou de configuration système (ex. : `.env`, `.config`, `.yml`, `.ini`) ?
- [ ] Non — les fichiers de configuration restreints **ne peuvent pas** être consultés et renvoient 403/404  
- [ ] Oui — les fichiers de configuration sensibles **sont** accessibles et divulguent des secrets internes ou des identifiants  

### Le code source peut-il être récupéré en ajoutant des extensions alternatives (ex. : `.php.txt`, `.jsp.old`, `.aspx.bak`) ?
- [ ] Non — le code de l'application n'est **pas** rendu en texte brut quelle que soit la manipulation de l'extension  
- [ ] Oui — le code source est révélé car le serveur **ne possède pas** de gestionnaire (handler) pour l'extension ajoutée  

### Les répertoires d'administration ou de métadonnées (ex. : `.git/`, `.svn/`, `.DS_Store`) sont-ils bloqués pour l'accès public ?
- [ ] Non — ces répertoires/fichiers n'existent **pas** à la racine du site web  
- [ ] Oui — l'accès n'est **pas possible** en raison de règles de réécriture côté serveur ou de permissions  
- [ ] Oui — les répertoires de métadonnées **sont** accessibles et permettent une reconstruction complète du code source  

### Comment le serveur gère-t-il les requêtes pour des fichiers ayant plusieurs extensions (ex. : `file.php.jpg`) ?
- [ ] Non — le serveur donne correctement la priorité à la sécurité de l'extension interne ou bloque la requête  
- [ ] Oui — le serveur traite le fichier selon la première extension, mais un contournement (bypass) n'est **pas possible** pour l'exécution  
- [ ] Oui — le serveur ignore l'extension finale, rendant l'exécution de code ou la divulgation de source **possible**

---

## WSTG-CONF-04 — Revue des anciens fichiers de sauvegarde et non référencés pour les informations sensibles

La revue des anciens fichiers de sauvegarde et des fichiers non référencés consiste à identifier les fichiers oubliés, temporaires ou cachés sur un serveur web qui ne sont pas destinés à un accès public. Ces fichiers incluent souvent des sauvegardes de code source (`.zip`, `.bak`), des fichiers temporaires d'éditeurs de texte (`.swp`, `~`), ou des métadonnées de contrôle de version (`.git`, `.svn`) susceptibles de divulguer des informations sensibles. Les attaquants utilisent des listes de mots (wordlists) automatisées et des outils de découverte pour effectuer du Brute Force sur les conventions de nommage courantes, dans le but d'exfiltrer des identifiants de base de données, des clés d'API (API keys) codées en dur ou une logique facilitant une exploitation ultérieure. Cette vulnérabilité provient généralement d'une maintenance manuelle du serveur ou de pipelines CI/CD défaillants qui ne nettoient pas la racine web de production.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fichiers de configuration, des archives de code source ou des identifiants sont découverts.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Le listage de répertoires (directory listing) est-il activé sur le serveur web ?
- [ ] Non — le listage de répertoires est **désactivé** et renvoie une erreur 403 Forbidden ou une erreur personnalisée  
- [ ] Oui — le listage de répertoires est **activé** sur des répertoires non sensibles  
- [ ] Oui — le listage de répertoires est **activé** sur des répertoires contenant du code source ou des fichiers sensibles *(Critique)*  

### Des fichiers de sauvegarde avec des extensions courantes (ex : .bak, .old, .save) sont-ils découvrables ?
- [ ] Non — les extensions de sauvegarde courantes ne sont **pas trouvées** ou sont bloquées par la configuration du serveur  
- [ ] Oui — des fichiers de sauvegarde existent mais ne contiennent **pas** d'informations sensibles  
- [ ] Oui — des fichiers de sauvegarde de configuration ou de code source **sont** accessibles *(Haute)*  

### Des répertoires de contrôle de version (ex : .git, .svn) ou des métadonnées sont-ils exposés ?
- [ ] Non — les métadonnées de contrôle de version ne sont **pas présentes** ou sont correctement bloquées  
- [ ] Oui — des métadonnées existent mais le dépôt complet **ne peut pas** être reconstruit  
- [ ] Oui — l'intégralité du dépôt de code source **peut** être exfiltrée via les métadonnées exposées  

### Des archives compressées (ex : .zip, .tar.gz, .rar) de l'application sont-elles présentes à la racine web ?
- [ ] Non — aucun fichier d'archive sensible n'a été découvert via Brute Force ou énumération  
- [ ] Oui — des archives ont été trouvées mais elles sont protégées par mot de passe ou contiennent des ressources publiques  
- [ ] Oui — des archives non chiffrées contenant le code source ou les données de l'application **sont** accessibles publiquement  

### L'application laisse-t-elle fuiter des fichiers temporaires créés par des éditeurs de texte ou des IDE ?
- [ ] Non — les fichiers temporaires tels que `.swp`, `~` ou `.DS_Store` ne sont **pas présents**  
- [ ] Oui — des fichiers temporaires non sensibles sont **présents**  
- [ ] Oui — les fichiers temporaires révèlent des fragments de code source ou des chemins internes

---

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

## WSTG-CONF-06 — Test des Méthodes HTTP

Le test des méthodes HTTP consiste à identifier les verbes supportés par le serveur web et l'application afin de s'assurer que seules les fonctionnalités nécessaires sont exposées. Au-delà des méthodes standards `GET` et `POST`, les serveurs peuvent activer par inadvertance des méthodes dangereuses telles que `PUT`, `DELETE` ou `TRACE`, qui peuvent permettre à des attaquants de télécharger des fichiers malveillants, de supprimer du contenu existant ou d'exfiltrer des cookies de session via le Cross-Site Tracing (XST). Les pentesteurs examinent les en-têtes de réponse tels que `Allow` ou `Public` et tentent de contourner les restrictions en utilisant des techniques de substitution de méthode (Method Overriding) ou de falsification de verbes (Verb Tampering) pour accéder à des fonctionnalités non autorisées. Ce test est crucial pour le durcissement de la surface d'attaque et la prévention des mauvaises configurations pouvant mener à un compromis du serveur ou à une manipulation non autorisée des données.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité devient Élevée si `PUT` ou `DELETE` permettent une manipulation de fichiers non autorisée ou si `TRACE` facilite l'exfiltration de jetons de session.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Les méthodes HTTP dangereuses comme `PUT` ou `DELETE` sont-elles désactivées sur l'ensemble de l'application ?
- [ ] Oui — seules les méthodes sûres (GET, POST, HEAD) **sont activées**  
- [ ] Non — `PUT` ou `DELETE` **sont activées** mais nécessitent une authentification valide  
- [ ] Non — `PUT` ou `DELETE` **sont activées** et accessibles sans authentification *(Critique)*  

### La méthode `TRACE` est-elle désactivée pour empêcher le Cross-Site Tracing (XST) ?
- [ ] Oui — la méthode `TRACE` **est désactivée** ou renvoie un code 405 Method Not Allowed  
- [ ] Non — la méthode `TRACE` **est activée** mais ne reflète pas les en-têtes sensibles  
- [ ] Non — la méthode `TRACE` **est activée** et reflète les en-têtes `Cookie` ou `Authorization` *(Moyenne)*  

### Le contournement de méthode HTTP (ex : `X-HTTP-Method-Override`) est-il supporté par le serveur ?
- [ ] Non — le serveur **ne traite pas** les en-têtes de substitution de méthode  
- [ ] Oui — le serveur traite les en-têtes de substitution mais les contrôles d'accès **sont appliqués**  
- [ ] Oui — le serveur traite les en-têtes de substitution et un contournement du contrôle d'accès **est possible**  

### Le serveur répond-il de manière sécurisée aux méthodes HTTP arbitraires ou malformées ?
- [ ] Oui — le serveur renvoie un code 405 Method Not Allowed ou 501 Not Implemented  
- [ ] Non — le serveur renvoie un code 200 OK ou 500 Error pour des méthodes arbitraires comme `JEFF` ou `TEST`  
- [ ] Non — l'utilisation de méthodes arbitraires permet de contourner les filtres d'authentification ou d'autorisation  

### La méthode `OPTIONS` est-elle restreinte ou configurée pour minimiser la divulgation d'informations ?
- [ ] Oui — `OPTIONS` est désactivée ou renvoie un en-tête `Allow` minimal  
- [ ] Non — `OPTIONS` révèle une large gamme de méthodes activées, y compris des verbes DAV ou de débogage

---

## WSTG-CONF-07 — Test HTTP Strict Transport Security

Le protocole HTTP Strict Transport Security (HSTS) est un mécanisme de sécurité qui permet à un serveur web d'informer les navigateurs qu'il ne doit être accessible que via HTTPS, empêchant ainsi les attaques de déclassement de protocole (protocol downgrade) et le détournement de cookies (cookie hijacking). En imposant une connexion chiffrée, le HSTS atténue les attaques de type Man-in-the-Middle (MitM), telles que SSLStrip, où un attaquant intercepte une requête HTTP initiale non sécurisée pour empêcher la transition vers TLS. Du point de vue d'un attaquant, l'absence de cet en-tête ou une durée `max-age` courte offre une fenêtre d'opportunité pour intercepter des jetons de session (session tokens) sensibles ou des identifiants (credentials) lors du handshake initial en clair. Ce test se concentre sur la vérification de la présence, de la durée et de la portée de l'en-tête `Strict-Transport-Security` sur tous les points de terminaison (endpoints) sensibles de l'application.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Basse / Moyenne* |

> *La sévérité devient Moyenne si l'application manipule des données PII, des jetons de session ou des identifiants, car l'absence de HSTS augmente le taux de réussite des attaques MitM.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### L'en-tête `Strict-Transport-Security` est-il présent dans les réponses HTTPS ?
- [ ] Oui — l'en-tête **est présent** sur tous les endpoints sensibles  
- [ ] Oui — l'en-tête **est présent** mais uniquement sur la page d'accueil  
- [ ] Non — l'en-tête **est absent** de l'ensemble de l'application  

### La directive `max-age` est-elle définie sur une durée suffisante ?
- [ ] Oui — `max-age` est défini sur un an ou plus (ex: `31536000`)  
- [ ] Oui — `max-age` est défini mais est **trop court** (ex: moins de 6 mois)  
- [ ] Non — la directive `max-age` **est manquante** ou définie sur `0` *(Critique)*  

### La politique HSTS couvre-t-elle tous les sous-domaines ?
- [ ] Oui — la directive `includeSubDomains` **est appliquée**  
- [ ] Non — la directive `includeSubDomains` **est manquante**, laissant les sous-domaines vulnérables au déclassement  
- [ ] Non — cette fonctionnalité ne s'applique pas car aucun sous-domaine n'existe  

### L'application est-elle enregistrée pour le préchargement HSTS (HSTS Preloading) ?
- [ ] Oui — la directive `preload` **est présente** et le domaine est dans la liste de préchargement HSTS  
- [ ] Oui — la directive `preload` **est présente** mais le domaine n'est **pas encore** dans la liste de préchargement  
- [ ] Non — la directive `preload` **est manquante**, permettant une fenêtre pour une attaque MitM lors de la première visite  

### L'en-tête HSTS est-il envoyé incorrectement via HTTP en clair ?
- [ ] Non — l'en-tête est **uniquement** envoyé via des connexions sécurisées HTTPS  
- [ ] Oui — l'en-tête **est envoyé** via HTTP, ce qui est ignoré par les navigateurs et peut divulguer des détails de configuration

---

## WSTG-CONF-08 — Test de la politique cross-domain RIA

Les politiques cross-domain des applications Internet riches (RIA) impliquent des fichiers tels que `crossdomain.xml` et `clientaccesspolicy.xml` qui définissent la manière dont les clients web—spécifiquement Adobe Flash et Microsoft Silverlight—gèrent les requêtes cross-origin. Ces fichiers sont critiques pour la sécurité car ils contournent explicitement la Same-Origin Policy (SOP), déterminant quels domaines externes sont autorisés à interagir avec l'application et à lire ses réponses. Des configurations trop permissives, comme l'utilisation de caractères génériques (wildcards) dans l'attribut `domain`, permettent à des attaquants d'héberger des objets RIA malveillants sur un site tiers pouvant exfiltrer des données sensibles ou effectuer des actions au nom d'utilisateurs authentifiés. Les pentesters localisent généralement ces fichiers à la racine du site web afin d'évaluer le niveau de confiance accordé aux origines tierces et d'identifier les fuites potentielles de données cross-origin.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si l'application manipule des données utilisateur sensibles ou des identifiants de session pouvant être exfiltrés via une politique permissive.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Les fichiers de politique cross-domain (`crossdomain.xml` ou `clientaccesspolicy.xml`) sont-ils présents à la racine du site web ?
- [ ] Non — les fichiers ne peuvent pas être trouvés dans le répertoire racine  
- [ ] Oui — `crossdomain.xml` (Flash) est présent  
- [ ] Oui — `clientaccesspolicy.xml` (Silverlight) est présent  
- [ ] Oui — les deux fichiers de politique RIA sont présents  

### L'attribut de domaine `allow-access-from` est-il restreint à des origines de confiance ?
- [ ] Non — les fichiers existent mais ne contiennent aucune entrée `allow-access-from`  
- [ ] Oui — des domaines spécifiques et de confiance sont explicitement mis en liste blanche et aucun contournement n'est possible  
- [ ] Oui — des domaines sont mis en liste blanche mais incluent des tiers non fiables ou des sous-domaines  
- [ ] Oui — un caractère générique `*` est utilisé, permettant l'accès depuis n'importe quelle origine *(Critique)*  

### La politique autorise-t-elle des communications non sécurisées via HTTP pour des ressources HTTPS ?
- [ ] Non — `secure="true"` est défini (ou par défaut), empêchant les origines HTTP d'accéder aux données HTTPS  
- [ ] Oui — `secure="false"` est explicitement défini, permettant à des attaquants MITM d'exfiltrer des données sécurisées  

### Les en-têtes HTTP sont-ils trop permissifs dans la configuration cross-domain ?
- [ ] Non — `allow-http-request-headers-from` est restreint ou n'est pas activé  
- [ ] Oui — des en-têtes spécifiques sont autorisés pour des domaines de confiance  
- [ ] Oui — le caractère générique `*` est utilisé pour les en-têtes, permettant à des attaquants d'envoyer des en-têtes personnalisés depuis n'importe quel domaine  

### La configuration `site-control` (master-policy) est-elle restrictive ?
- [ ] Non — `permitted-cross-domain-policies` est défini sur `none` ou `master-only`  
- [ ] Oui — la politique autorise d'autres fichiers sur le serveur à définir leurs propres règles (`all`)

---

## WSTG-CONF-09 — Test des permissions de fichiers

Le test des permissions de fichiers consiste à auditer les niveaux de contrôle d'accès assignés aux fichiers et répertoires sur le serveur web afin de s'assurer que les ressources sensibles ne sont pas sur-exposées à des utilisateurs ou processus non autorisés. Des permissions mal configurées peuvent permettre à des attaquants de lire des fichiers de configuration sensibles contenant des identifiants de base de données, de visualiser du code source propriétaire ou de modifier des scripts côté serveur pour parvenir à une exécution de code à distance (Remote Code Execution). Cette vulnérabilité survient généralement lors de la phase de déploiement lorsque les drapeaux par défaut "world-readable" ou "world-writable" sont laissés sur des répertoires critiques de l'application, tels que les chemins de configuration, les dossiers de logs ou les dépôts d'upload. Du point de vue d'un attaquant, la découverte d'un fichier mal sécurisé sert souvent de catalyseur principal pour une escalade de privilèges (Privilege Escalation) ou un compromis complet du système.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fichiers de configuration sensibles (ex: `.env`, `web.config`) sont lisibles ou si un accès en écriture à la racine web (web root) est possible.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Outils :** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Les fichiers de configuration sensibles de l'application sont-ils protégés contre les accès non autorisés ?
- [ ] Oui — les fichiers de configuration (ex : `.env`, `config.php`) ne sont **pas accessibles** via des requêtes web  
- [ ] Oui — les fichiers sont présents mais l'accès est **restreint** aux seuls utilisateurs locaux autorisés  
- [ ] Non — les fichiers de configuration sensibles **sont accessibles** et divulguent des identifiants ou des secrets  

### Un attaquant peut-il modifier des fichiers ou des répertoires au sein de la racine web (web root) ?
- [ ] Non — tous les répertoires de la racine web sont en **lecture seule** pour l'utilisateur du serveur web  
- [ ] Oui — des répertoires accessibles en écriture existent mais l'exécution de scripts est **désactivée**  
- [ ] Oui — des répertoires accessibles en écriture par tous (world-writable) existent et **permettent** le téléchargement et l'exécution de scripts *(Critique)*  

### Les métadonnées de contrôle de version ou les fichiers de sauvegarde sont-ils exposés en raison de permissions laxistes ?
- [ ] Non — `.git`, `.svn` et les fichiers de sauvegarde (ex : `.bak`, `~`) ne sont **pas présents** ou sont protégés  
- [ ] Oui — des répertoires de métadonnées existent mais le listage de répertoire (directory listing) est **désactivé**  
- [ ] Oui — les métadonnées sensibles ou les sauvegardes sont **entièrement accessibles**, permettant la récupération du code source  

### L'utilisateur du serveur web fonctionne-t-il selon le principe du moindre privilège ?
- [ ] Oui — le processus du serveur web s'exécute en tant qu'utilisateur **dédié à bas privilèges**  
- [ ] Non — le processus du serveur web s'exécute avec des **privilèges inutiles** (ex : `root` ou `Administrator`)  

### Les fichiers de logs sont-ils sécurisés contre la lecture ou la modification non autorisée ?
- [ ] Oui — les fichiers de logs sont **restreints** aux utilisateurs administratifs  
- [ ] Oui — les fichiers de logs sont lisibles mais ne contiennent **pas** de jetons de session ou de PII  
- [ ] Non — les fichiers de logs sont **lisibles publiquement** et divulguent des données de transaction sensibles ou des informations utilisateur

---

## WSTG-CONF-10 — Subdomain Takeover

Le Subdomain Takeover (prise de contrôle de sous-domaine) se produit lorsqu'une entrée DNS (généralement un CNAME, mais occasionnellement des enregistrements A ou MX) pointe vers une ressource déclassée ou inexistante chez un fournisseur cloud tiers ou un service d'hébergement. Les attaquants identifient ces enregistrements DNS "orphelins" (dangling) en recherchant des messages d'erreur spécifiques provenant de fournisseurs tels qu'AWS, GitHub ou Azure, indiquant qu'une ressource n'est plus revendiquée. En enregistrant le nom de la ressource abandonnée sur la plateforme du fournisseur, l'attaquant gagne le contrôle total du sous-domaine, ce qui lui permet d'héberger du contenu malveillant, d'exfiltrer des cookies de session définis au niveau du domaine de base et de contourner les protections Content Security Policy (CSP) ou CORS. Cette vulnérabilité est particulièrement dangereuse car elle exploite la confiance associée à la structure de domaine légitime de l'organisation pour faciliter le phishing et le vol de données à fort impact.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Statut du test** | Non effectué |
| **Gravité** | Élevée / Critique* |

> *La gravité devient Critique si le sous-domaine peut être utilisé pour détourner des cookies de session du domaine de base ou contourner les redirections SSO/OAuth.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Outils :** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### L'application utilise-t-elle des services d'hébergement tiers ou des services cloud via des sous-domaines ?
- [ ] Non — tous les sous-domaines pointent vers un espace IP contrôlé par l'organisation  
- [ ] Oui — les sous-domaines pointent vers des fournisseurs tiers (S3, GitHub Pages, Heroku, etc.)  

### Existe-t-il des enregistrements DNS "orphelins" (dangling) pointant vers des ressources inexistantes ?
- [ ] Non — tous les enregistrements DNS identifiés se résolvent vers des ressources actives et contrôlées  
- [ ] Oui — des enregistrements CNAME ou A existent pour des ressources qui **n'existent plus** chez le fournisseur  
- [ ] Oui — les enregistrements DNS se résolvent en NXDOMAIN ou en pages d'erreur spécifiques au fournisseur *(ex: "NoSuchBucket")*  

### La ressource orpheline identifiée peut-elle être revendiquée avec succès ?
- [ ] Non — le fournisseur dispose de protections contre la revendication de noms abandonnés ou une vérification de compte est requise  
- [ ] Oui — le nom de la ressource **peut** être enregistré sur la plateforme tierce par n'importe quel utilisateur  

### Le domaine de base utilise-t-il des cookies avec une portée "Domain" qui pourraient être compromis ?
- [ ] Non — les cookies sont strictement limités à des sous-domaines spécifiques ou utilisent des flags `Host-Only`  
- [ ] Oui — des cookies sensibles (ID de session, JWT) sont définis pour le domaine parent et **peuvent** être exfiltrés via un sous-domaine détourné  

### Le sous-domaine peut-il être utilisé pour contourner des en-têtes de sécurité ou des flux d'authentification ?
- [ ] Non — aucune dépendance de sécurité sur les sous-domaines identifiés  
- [ ] Oui — le sous-domaine est en liste blanche dans les directives CSP `script-src` ou `connect-src`  
- [ ] Oui — le sous-domaine est une URI de redirection valide pour les configurations OAuth ou SSO

---

## WSTG-CONF-11 — Tester le stockage Cloud

Le test des erreurs de configuration du stockage cloud implique l'identification et l'audit des services de stockage d'objets accessibles publiquement tels que Amazon S3, Azure Blobs ou Google Cloud Storage. Ces ressources contiennent souvent des données sensibles, notamment des sauvegardes, du code source, des PII ou des fichiers de configuration exposés en raison de listes de contrôle d'accès (ACL) ou de politiques de gestion des identités et des accès (IAM) trop permissives. Les attaquants énumèrent les noms des buckets par le biais de la reconnaissance des fichiers JavaScript, du code source HTML et de techniques de Brute Force afin de trouver des points d'entrée pour l'exfiltration de données ou la modification non autorisée de fichiers. L'exploitation peut entraîner un impact significatif, y compris des violations de données complètes ou la capacité de servir des fichiers malveillants à partir d'un domaine de confiance.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Critique* |

> *La sévérité devient Critique si des PII, des identifiants ou des sauvegardes de production sont accessibles, ou si l'accès en écriture non authentifié est activé.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Les points de terminaison de stockage cloud (buckets/conteneurs) sont-ils identifiés et énumérés ?
- [ ] Non — aucun point de terminaison de stockage cloud n'est utilisé ou découvrable  
- [ ] Oui — les points de terminaison de stockage sont identifiés via l'analyse du code ou la surveillance du trafic  
- [ ] Oui — les points de terminaison de stockage sont découverts via Brute Force sur les conventions de nommage  

### Les utilisateurs non authentifiés peuvent-ils lister le contenu du stockage cloud ?
- [ ] Non — le listage du bucket est **désactivé** et renvoie 403 Forbidden ou 404 Not Found  
- [ ] Oui — le listage est **activé** mais aucun fichier sensible n'est présent  
- [ ] Oui — le listage est **activé** et les noms de fichiers/métadonnées sensibles sont visibles  

### Le téléchargement de fichiers sensibles à partir des buckets identifiés est-il possible ?
- [ ] Non — les fichiers sont protégés par des politiques IAM ou une authentification valide est requise  
- [ ] Oui — les fichiers sont accessibles mais nécessitent une URL signée qui **ne peut pas** être facilement devinée  
- [ ] Oui — les fichiers sensibles (sauvegardes, `.env`, PII) sont **publiquement accessibles** pour le téléchargement *(Critique)*  

### L'upload ou la modification de fichiers non authentifiés est-il autorisé ?
- [ ] Non — les uploads ou modifications non authentifiés ne sont **pas possibles**  
- [ ] Oui — un upload authentifié est requis mais un contournement **est possible** via des conditions de politique faibles  
- [ ] Oui — l'écriture, l'écrasement ou la suppression de fichiers non authentifiés est **possible** *(Critique)*  

### Les politiques de Cross-Origin Resource Sharing (CORS) sur le point de terminaison de stockage sont-elles restrictives ?
- [ ] Oui — CORS est **désactivé** ou restreint à des origines de confiance spécifiques  
- [ ] Non — CORS est **activé** avec un caractère joker `*` (wildcard), permettant un accès Web non autorisé

---

## WSTG-CONF-12 — Content Security Policy (CSP)

La Content Security Policy (CSP) est un mécanisme de défense en profondeur essentiel, implémenté via les en-têtes de réponse HTTP pour atténuer les risques tels que le Cross-Site Scripting (XSS), le clickjacking et les attaques par injection de données. En définissant quelles ressources dynamiques sont autorisées à être chargées, une CSP bien configurée restreint la capacité d'un attaquant à exécuter des scripts non autorisés ou à exfiltrer des données sensibles vers des domaines externes. Les Pentesters évaluent la politique pour détecter des directives trop permissives, l'utilisation de mots-clés non sécurisés comme `'unsafe-inline'`, et la dépendance à des CDNs non sécurisés qui pourraient faciliter des contournements (bypasses). Une CSP faible ou absente ne crée pas directement une vulnérabilité, mais augmente considérablement l'impact des attaques par injection réussies en ne fournissant pas de couche de protection secondaire.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Outils :** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Un en-tête Content Security Policy (CSP) est-il présent dans les réponses de l'application ?
- [ ] Oui — l'en-tête `Content-Security-Policy` est **présent** et **appliqué**  
- [ ] Oui — `Content-Security-Policy-Report-Only` est **présent** pour test  
- [ ] Non — aucun en-tête CSP n'est **présent** dans les réponses  

### Les directives `script-src` ou `default-src` sont-elles correctement restreintes ?
- [ ] Oui — les directives utilisent un whitelisting strict, des nonces ou des hashes et aucun contournement n'est **possible**  
- [ ] Oui — les directives sont **correctement configurées** mais la liste blanche inclut des CDNs connus pour être contournables  
- [ ] Non — les directives utilisent des jokers (`*`) ou des schémas `data:`, rendant le contournement **possible**  

### La politique autorise-t-elle l'exécution de scripts inline non sécurisés ou de eval ?
- [ ] Non — `'unsafe-inline'` et `'unsafe-eval'` ne sont **pas présents**  
- [ ] Oui — `'unsafe-inline'` est **présent** mais protégé par un `nonce` ou un `hash`  
- [ ] Oui — `'unsafe-inline'` ou `'unsafe-eval'` sont **activés** sans protections supplémentaires *(Critique)*  

### L'application est-elle protégée contre le clickjacking via la directive `frame-ancestors` ?
- [ ] Oui — `frame-ancestors` est défini sur `'none'` ou `'self'`  
- [ ] Oui — `frame-ancestors` est **activé** mais autorise des domaines tiers de confiance spécifiques  
- [ ] Non — `frame-ancestors` est **manquant**, s'appuyant sur l'ancien en-tête `X-Frame-Options` ou aucune protection  

### Existe-t-il un mécanisme pour signaler les violations de la CSP ?
- [ ] Oui — `report-uri` ou `report-to` est **configuré** et actif  
- [ ] Non — le signalement des violations est **désactivé** ou **non configuré**

---

## WSTG-CONF-13 — Path Confusion

La confusion de chemin (Path Confusion) provient de divergences dans la manière dont les différents composants Web, tels que les proxys inverses (reverse proxies), les répartiteurs de charge (load balancers) et les serveurs d'application backend, analysent et interprètent les chemins URL. Les attaquants exploitent ces incohérences en injectant des caractères spécifiques tels que des points-virgules, des slashes encodés ou des segments de points (dot-segments) pour tromper les filtres de sécurité et accéder à des points de terminaison (endpoints) restreints ou à des ressources statiques. Cette vulnérabilité peut entraîner des défaillances de sécurité critiques, notamment le contournement d'authentification (authentication bypass), l'accès non autorisé aux données et l'empoisonnement du cache Web (Web Cache Poisoning), car le frontal (front-end) peut appliquer des règles de sécurité à un chemin que le backend interprète différemment. Une exploitation réussie se produit généralement à la frontière architecturale où le routage des requêtes et la logique de normalisation divergent à travers la pile technologique (tech stack).


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si la confusion de chemin entraîne un contournement d'authentification ou du contrôle d'accès pour des points de terminaison administratifs sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Les différentes couches architecturales interprètent-elles les délimiteurs de chemin (ex : `;`, `#`, `?`) de manière cohérente ?
- [ ] Oui — toutes les couches normalisent et interprètent les délimiteurs de chemin de manière identique  
- [ ] Non — des incohérences existent mais **ne peuvent pas** être utilisées pour accéder à des ressources restreintes  
- [ ] Non — les divergences permettent à un attaquant de contourner les filtres front-end et d'atteindre la logique backend **est possible**  

### Les contrôles d'accès peuvent-ils être contournés à l'aide de séquences de traversée de chemin (path traversal) ou de caractères encodés ?
- [ ] Non — la normalisation est appliquée de manière cohérente avant les contrôles de sécurité  
- [ ] Oui — le contournement est **possible** via des séquences de segments de points (ex : `/admin/..;/`)  
- [ ] Oui — le contournement est **possible** via des caractères encodés en URL (ex : `%2f`, `%2e%2e%2f`)  

### L'application est-elle vulnérable à l'empoisonnement du cache Web (Web Cache Poisoning) via la confusion de chemin ?
- [ ] Non — la logique de mise en cache n'est pas affectée par l'ambiguïté du chemin ou les informations de chemin supplémentaires  
- [ ] Oui — du contenu malveillant **peut** être mis en cache pour des chemins légitimes en raison de divergences de mappage  
- [ ] Oui — des informations sensibles **peuvent** être mises en cache dans des répertoires publics via des techniques `RCD` (Relative Path Overwrite)  

### Le serveur gère-t-il les « informations de chemin supplémentaires » (PathInfo) de manière sécurisée ?
- [ ] Non — la fonctionnalité est **désactivée** ou n'existe pas  
- [ ] Oui — `PathInfo` est géré et n'interfère **pas** avec les filtres de sécurité  
- [ ] Non — `PathInfo` permet à un attaquant d'ajouter des données qui confondent la logique de routage de l'application

---

## WSTG-CONF-14 — Tester les mauvaises configurations des en-têtes de sécurité HTTP

Le test des mauvaises configurations des en-têtes de sécurité HTTP consiste à vérifier la présence et la mise en œuvre correcte des en-têtes défensifs qui fournissent une protection essentielle contre les vecteurs d'attaque web courants. Bien que ces en-têtes ne corrigent pas les vulnérabilités de code sous-jacentes, leur absence augmente considérablement le risque d'attaques de l'homme du milieu (MITM), d'exploitation du MIME-sniffing et de fuites de données involontaires entre origines via les référents (referrers). Du point de vue d'un attaquant, l'absence d'en-têtes de sécurité est un indicateur très visible d'un environnement mal durci, facilitant souvent des chaînes d'exploitation plus complexes comme le détournement de session (session hijacking) ou l'exfiltration d'informations. Ces configurations sont généralement évaluées au niveau du serveur et doivent être appliquées de manière cohérente à tous les types de réponses, y compris les points de terminaison API et les ressources statiques.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **État du test** | Non effectué |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit de présence des en-têtes de sécurité
| En-tête | Présent | Manquant | Configuration recommandée |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` ou `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Spécifique aux fonctionnalités, ex: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Comment les en-têtes de sécurité sont-ils appliqués sur l'ensemble du périmètre de l'application ?
- [ ] Oui — les en-têtes sont appliqués globalement via un répartiteur de charge (load balancer) ou un proxy inverse (reverse proxy) centralisé et **ne peuvent pas** être surchargés  
- [ ] Oui — les en-têtes sont appliqués aux réponses des documents principaux mais sont **manquants** sur les réponses API ou les ressources statiques  
- [ ] Non — les en-têtes sont appliqués de manière incohérente ou sont **manquants** sur l'ensemble du domaine de l'application  

### L'en-tête Strict-Transport-Security (HSTS) est-il correctement configuré ?
- [ ] Oui — `max-age` est défini sur une longue durée (ex: 2 ans) et `includeSubDomains` est **activé**  
- [ ] Oui — `max-age` est défini mais `includeSubDomains` est **désactivé**, laissant les sous-domaines vulnérables  
- [ ] No — l'en-tête HSTS n'est **pas présent** ou `max-age` est défini sur 0, permettant des attaques par déclassement de protocole (protocol downgrade)  

### La politique Referrer-Policy empêche-t-elle la fuite de paramètres d'URL sensibles ?
- [ ] Oui — la politique est définie sur `no-referrer` ou `same-origin` *(Le plus sécurisé)*  
- [ ] Oui — la politique est définie sur `strict-origin-when-cross-origin`  
- [ ] Non — la politique n'est **pas définie** ou est définie sur `unsafe-url`, risquant de divulguer des jetons ou des PII sensibles dans les en-têtes referer  

### L'en-tête X-Content-Type-Options empêche-t-il le MIME-sniffing ?
- [ ] Oui — la directive `nosniff` est **présente** et correctement implémentée  
- [ ] Non — l'en-tête est **manquant**, permettant potentiellement à un navigateur d'exécuter des fichiers non exécutables (ex: images) comme des scripts

---

## WSTG-IDNT-01 — Test des définitions de rôles

Le test des définitions de rôles consiste à identifier et à documenter les différents rôles d'utilisateurs et niveaux de permission définis au sein de l'application afin de s'assurer qu'ils sont logiquement séparés et qu'ils respectent le principe du moindre privilège (least privilege). Un attaquant analyse l'application pour cartographier les rôles à hauts privilèges par rapport aux rôles d'utilisateurs standards, à la recherche de toute ambiguïté dans les limites de permissions ou de fonctions administratives non documentées. L'identification de ces rôles est la première étape de la découverte de chemins d'escalade de privilèges (privilege escalation), car les incohérences dans les définitions de rôles mènent souvent à un accès non autorisé à des données sensibles ou à des fonctionnalités administratives. Cette phase se déroule généralement lors de la reconnaissance initiale et de la cartographie de la logique métier, en se concentrant sur les profils utilisateurs, les tableaux de bord administratifs et les structures de permissions d'API.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Statut du test** | Non effectué |
| **Sévérité** | Basse / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Les rôles sont-ils clairement définis et documentés dans l'application ou sa documentation ?
- [ ] Oui — les rôles sont entièrement documentés et suivent le principe du **moindre privilège**  
- [ ] Oui — les rôles sont documentés mais contiennent des **permissions redondantes**  
- [ ] Non — aucune documentation ou définition formelle des rôles n'existe  

### Existe-t-il une séparation claire entre les fonctions administratives et les fonctions utilisateur standard ?
- [ ] Oui — les fonctions administratives sont **complètement isolées** des vues utilisateurs standards  
- [ ] Oui — la séparation est présente mais les points de terminaison (endpoints) administratifs sont **prévisibles ou devinables**  
- [ ] Non — les fonctions administratives et standards sont **mélangées** au sein des mêmes interfaces  

### L'application utilise-t-elle un mécanisme centralisé pour le contrôle d'accès basé sur les rôles (RBAC) ?
- [ ] Oui — un RBAC ou ABAC centralisé est **implémenté** et appliqué de manière cohérente  
- [ ] Oui — des contrôles décentralisés existent mais sont **appliqués de manière incohérente** à travers les modules  
- [ ] Non — la logique de contrôle d'accès est **codée en dur** dans les pages ou composants individuels  

### Existe-t-il des rôles non documentés ou cachés dans le système ?
- [ ] Non — tous les rôles découverts correspondent aux fonctionnalités documentées  
- [ ] Oui — des rôles cachés ou "shadow" (ex: `super_admin`, `support`, `test`) **sont présents**  

### Un utilisateur à bas privilèges peut-il énumérer les permissions ou les rôles d'autres utilisateurs ?
- [ ] Non — les utilisateurs **ne peuvent pas** voir ou énumérer les rôles/permissions d'autres entités  
- [ ] Oui — les utilisateurs **peuvent** énumérer les rôles via les pages de profil, les métadonnées ou les réponses d'API *(Moyenne)*

---

## WSTG-IDNT-02 — Test du processus d'enregistrement des utilisateurs

Le test du processus d'enregistrement des utilisateurs consiste à évaluer le flux de travail par lequel de nouvelles identités sont créées afin de s'assurer qu'il ne peut pas être détourné pour un accès non autorisé ou une exploitation automatisée. Les vulnérabilités dans ce processus se manifestent souvent par un manque de vérification d'identité (e-mail/SMS), une vulnérabilité à la création de comptes en masse via des bots, ou une divulgation d'informations via des messages d'erreur verbeux qui révèlent des noms d'utilisateurs existants. Les attaquants exploitent ces failles pour effectuer une User Enumeration, mener des campagnes de spam à grande échelle ou manipuler les paramètres d'enregistrement pour obtenir des privilèges élevés lors de la phase de création de compte. Ce test est essentiel pour protéger l'intégrité de l'application et empêcher les attaquants de peupler le système avec des comptes frauduleux ou malveillants.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Une vérification d'identité est-elle requise avant qu'un nouveau compte ne devienne actif ?
- [ ] Oui — l'enregistrement nécessite un token unique, limité dans le temps, envoyé par e-mail ou SMS  
- [ ] Oui — la vérification est requise mais les tokens sont **faibles** ou **prévisibles**  
- [ ] Non — les comptes sont activés **immédiatement** sans aucune étape de vérification  

### Le processus d'enregistrement empêche-t-il l'User Enumeration ?
- [ ] Oui — l'application renvoie des réponses **identiques** pour les e-mails/noms d'utilisateur nouveaux et existants  
- [ ] Non — les messages d'erreur (ex: "E-mail déjà utilisé") **permettent** à un attaquant de cartographier les utilisateurs enregistrés  
- [ ] Non — des différences de temps de réponse du serveur (Timing Attacks) **révèlent** si un utilisateur existe  

### Des contrôles anti-automatisation sont-ils mis en œuvre pour empêcher l'enregistrement massif ?
- [ ] Oui — des mécanismes CAPTCHA ou de proof-of-work sont **présents** et **efficaces**  
- [ ] Oui — des contrôles existent mais un bypass **est possible** via les points de terminaison API ou la manipulation de headers  
- [ ] Non — aucun Rate Limiting ou CAPTCHA n'est **appliqué** au point de terminaison d'enregistrement  

### Les paramètres d'enregistrement peuvent-ils être manipulés pour obtenir une escalade de privilèges ?
- [ ] No — les rôles d'utilisateur sont attribués **strictement** par la logique côté serveur  
- [ ] Oui — des paramètres tels que `role`, `admin` ou `group_id` **peuvent** être modifiés dans la requête pour obtenir un accès plus élevé  

### La politique de mot de passe est-elle appliquée lors de la phase d'enregistrement ?
- [ ] Oui — des exigences de mot de passe fort sont **appliquées** côté serveur  
- [ ] Non — les mots de passe faibles (ex: "password123") sont **acceptés** par le système

---

## WSTG-IDNT-03 — Test du processus de provisionnement de compte

Le processus de provisionnement de compte (Account Provisioning) régit la manière dont les nouvelles identités sont créées, vérifiées et dotées de privilèges au sein de l'environnement d'une application. Les attaquants ciblent ce flux de travail pour effectuer des enregistrements de masse automatisés à des fins de spam ou de déni de service (DoS), contourner les étapes de vérification d'identité pour créer des comptes frauduleux, ou manipuler des paramètres pour s'octroyer des permissions élevées lors de la phase d'inscription. Les vulnérabilités se manifestent souvent dans les formulaires d'inscription en libre-service, les systèmes sur invitation uniquement ou les consoles de provisionnement administratif où des défauts de logique métier permettent la création de comptes non autorisés ou une escalade de privilèges. Du point de vue d'un attaquant, compromettre le flux de provisionnement fournit un ancrage légitime qui peut être utilisé comme base pour une exploitation plus profonde, l'exfiltration de données ou des attaques d'ingénierie sociale.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Critique si un attaquant peut provisionner des comptes administratifs ou contourner les restrictions d'inscription globales.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### L'auto-inscription est-elle strictement contrôlée ou limitée aux utilisateurs autorisés ?
- [ ] Oui — l'inscription est **désactivée** ou nécessite une approbation administrative manuelle  
- [ ] Oui — l'inscription est ouverte mais restreinte à des domaines de messagerie **autorisés** ou à des jetons d'invitation (tokens)  
- [ ] Non — l'inscription est **ouverte** à tout utilisateur sans restriction de domaine ou de jeton  

### Un mécanisme de vérification d'identité robuste (ex : email/SMS) est-il requis avant l'activation du compte ?
- [ ] Oui — la vérification est **requise** et **ne peut pas** être contournée  
- [ ] Oui — la vérification est **requise** mais **peut** être contournée via une manipulation de paramètres ou des jetons prévisibles  
- [ ] Non — les comptes sont **actifs immédiatement** après la soumission sans aucune vérification  

### L'utilisateur peut-il manipuler les paramètres de rôle ou de permission lors de la requête d'inscription ?
- [ ] Non — les rôles sont **assignés côté serveur** et aucun paramètre côté client n'existe  
- [ ] Oui — des paramètres de rôle existent mais la manipulation n'est **pas possible** en raison de la validation côté serveur  
- [ ] Oui — les paramètres de rôle/permission (ex : `role=admin`, `is_staff=true`) **peuvent** être modifiés pour obtenir un accès privilégié *(Critique)*  

### Des limitations de débit (Rate Limiting) ou des contrôles anti-automatisation sont-ils mis en œuvre pour empêcher la création massive de comptes ?
- [ ] Oui — les limitations de débit et les CAPTCHAs sont **actifs** et efficaces  
- [ ] Oui — des contrôles existent mais un contournement **est possible** via l'usurpation d'en-tête (header spoofing) ou des services de résolution de CAPTCHA  
- [ ] Non — aucune limitation de débit ni aucun contrôle anti-automatisation n'est **appliqué**  

### Le processus de provisionnement divulgue-t-il des informations sur les comptes existants (User Enumeration) ?
- [ ] Non — les messages de réponse sont **identiques** pour les utilisateurs existants et non existants  
- [ ] Oui — des réponses différentes ou des variations de timing **permettent** l'énumération des utilisateurs existants

---

## WSTG-IDNT-04 — Test d'énumération de comptes et de comptes utilisateurs devinables

L'énumération de comptes se produit lorsqu'une application révèle l'existence ou l'inexistence d'un compte utilisateur à travers des variations dans les réponses HTTP, les messages d'erreur ou des différences de temps mesurables (timing differences). Les attaquants exploitent ce comportement en soumettant systématiquement des listes de noms d'utilisateur pour identifier les comptes valides, qui sont ensuite ciblés pour des attaques de type Credential Stuffing, Brute Force ou d'ingénierie sociale. Cette vulnérabilité se manifeste couramment dans les portails de connexion, les fonctionnalités de réinitialisation de mot de passe, les formulaires d'inscription et les points de terminaison d'API qui gèrent des identifiants spécifiques à l'utilisateur. Du point de vue d'un attaquant, une énumération réussie réduit considérablement la surface d'attaque en transformant une tentative de Brute Force aveugle en une exploitation ciblée d'identités connues et valides.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **État du test** | Non réalisé |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Les messages d'erreur d'authentification sont-ils cohérents dans tous les scénarios ?
- [ ] Oui — des messages d'erreur génériques sont utilisés à la fois pour les noms d'utilisateur invalides et les mots de passe invalides  
- [ ] Oui — les messages sont similaires, mais de légères variations de ponctuation ou de casse **sont présentes**  
- [ ] Non — les messages d'erreur indiquent explicitement "Utilisateur non trouvé" ou "Mot de passe incorrect" *(Vulnérable)*  

### Le flux de réinitialisation de mot de passe ou "mot de passe oublié" laisse-t-il fuiter l'existence d'un compte ?
- [ ] Non — l'application renvoie un message générique, que l'e-mail/nom d'utilisateur existe ou non  
- [ ] Oui — des contrôles sont en place, mais un contournement (bypass) **est possible** via la longueur de la réponse ou les codes d'état  
- [ ] Oui — l'application confirme explicitement si un e-mail de réinitialisation a été envoyé ou si le compte **n'existe pas**  

### Le processus d'inscription est-il protégé contre l'énumération d'utilisateurs ?
- [ ] Non — l'inscription ne laisse pas fuiter d'informations ou utilise des CAPTCHA/Rate Limiting pour empêcher les vérifications en masse  
- [ ] Oui — des contrôles sont en place, mais un contournement **est possible** via le timing ou les différences de réponse de l'API  
- [ ] Oui — l'application informe immédiatement l'utilisateur si le nom d'utilisateur ou l'e-mail **est déjà utilisé**  

### Des différences de timing sont-elles mesurables lors de la comparaison de noms d'utilisateur valides vs invalides ?
- [ ] Non — les temps de réponse sont cohérents quelle que soit la validité du compte grâce à des comparaisons en temps constant  
- [ ] Oui — des différences de timing existent mais sont trop faibles pour être mesurées de manière fiable sur le réseau  
- [ ] Oui — des divergences de timing mesurables **sont présentes** (par exemple, en raison de l'exécution du hachage du mot de passe uniquement pour les utilisateurs valides)  

### L'application utilise-t-elle des conventions de nommage prévisibles ou devinables pour les comptes ?
- [ ] Non — les identifiants de compte sont aléatoires (UUID) ou définis par l'utilisateur et complexes  
- [ ] Oui — les identifiants de compte suivent un modèle (par exemple, `emp001`, `emp002`) et l'énumération **est possible**  
- [ ] Oui — les identifiants sont des entiers séquentiels, rendant l'énumération complète de la base de données **possible**

---

## WSTG-IDNT-05 — Test de politique de noms d'utilisateur faible ou non appliquée

Le test des politiques de noms d'utilisateur faibles ou non appliquées se concentre sur les règles régissant la création d'identifiants de compte et leur vulnérabilité à l'énumération ou au spoofing. Des politiques faibles permettent souvent des séquences prévisibles, des mots du dictionnaire courants ou des identifiants qui reflètent les identifiants internes des employés, ce qui réduit considérablement l'espace de recherche pour les attaques de type Brute Force et Credential Stuffing. Du point de vue d'un attaquant, l'identification de ces modèles est la première étape de la découverte de comptes à grande échelle, souvent réalisée en analysant les messages d'erreur lors de l'enregistrement, le comportement de réinitialisation de mot de passe ou les métadonnées dans les profils publics. Garantir une politique robuste empêche les attaquants de cartographier systématiquement la base d'utilisateurs et facilite la défense contre le Phishing ciblé et les tentatives automatisées d'Authentication Bypass.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **État du test** | Non effectué |
| **Sévérité** | Basse / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Les noms d'utilisateur sont-ils basés sur des modèles hautement prévisibles ou séquentiels ?
- [ ] Non — les noms d'utilisateur sont aléatoires, définis par l'utilisateur ou sont des chaînes à haute entropie  
- [ ] Oui — les noms d'utilisateur suivent un format prévisible (ex : `user1001`, `user1002`) mais l'authentification est robuste  
- [ ] Oui — les noms d'utilisateur sont **strictement séquentiels** ou suivent un format d'entreprise connu, facilitant une énumération aisée  

### L'application impose-t-elle des exigences de longueur minimale et de complexité pour les noms d'utilisateur ?
- [ ] Oui — des politiques strictes de longueur et de jeu de caractères **sont appliquées** lors de l'enregistrement  
- [ ] Oui — des politiques existent mais autorisent des noms d'utilisateur extrêmement courts (ex : 1-2 caractères) ou trop simples  
- [ ] Non — **aucune politique** n'est appliquée concernant la longueur ou les types de caractères des noms d'utilisateur  

### Des noms d'utilisateur valides peuvent-ils être énumérés via les réponses de l'application ?
- [ ] Non — l'application renvoie des messages d'erreur génériques et présente un délai de réponse (timing) cohérent pour toutes les tentatives  
- [ ] Oui — l'application renvoie des messages d'erreur distincts (ex : "Nom d'utilisateur déjà utilisé") mais un Rate Limiting **est appliqué**  
- [ ] Oui — l'application **divulgue** des noms d'utilisateur valides via des erreurs d'enregistrement, des réinitialisations de mot de passe ou des différences de timing  

### La politique empêche-t-elle l'enregistrement de noms d'utilisateur administratifs communs ou réservés ?
- [ ] Oui — l'application bloque les noms communs (ex : `admin`, `root`, `support`) et les termes réservés au système  
- [ ] Non — les utilisateurs **peuvent** enregistrer des comptes avec des noms sensibles ou administratifs  

### La politique de noms d'utilisateur est-elle cohérente sur tous les points d'entrée (API, Mobile, Web) ?
- [ ] Oui — les politiques **sont appliquées** de manière cohérente sur toutes les interfaces et versions  
- [ ] Non — les points de terminaison hérités (legacy) ou des versions spécifiques d'API **n'appliquent pas** les mêmes contraintes de nom d'utilisateur

---

## WSTG-ATHN-01 — Test du transport des identifiants via un canal chiffré

Tester le transport des identifiants via un canal chiffré permet de s'assurer que les données d'authentification sensibles, telles que les noms d'utilisateur, les mots de passe et les jetons de session (session tokens), sont protégées contre l'interception pendant le transit. Les attaquants positionnés sur le réseau — par exemple via des attaques Man-in-the-Middle (MitM) sur un Wi-Fi public ou des réseaux internes compromis — peuvent utiliser des outils de capture de paquets (packet sniffing) pour intercepter des identifiants en clair si TLS/SSL n'est pas correctement appliqué. Cette vulnérabilité se produit généralement sur les points de terminaison de connexion (login endpoints), les formulaires de réinitialisation de mot de passe et les en-têtes d'authentification API lorsque l'application ne parvient pas à imposer HTTPS ou effectue une redirection incorrecte depuis HTTP. Une exploitation réussie accorde à l'attaquant un accès complet aux comptes utilisateurs, entraînant un compromis total des données utilisateur et un risque potentiel de mouvement latéral au sein de l'application.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Le protocole HTTPS est-il imposé pour l'ensemble du processus d'authentification ?
- [ ] Oui — HSTS est **activé** et tout le trafic est strictement forcé via HTTPS  
- [ ] Oui — une redirection vers HTTPS a lieu, mais HSTS n'est **pas activé**  
- [ ] Non — les identifiants **peuvent** être transmis via HTTP non chiffré  

### Les pages et formulaires de connexion sont-ils servis via un canal chiffré ?
- [ ] Oui — la page contenant le formulaire de connexion est fournie via HTTPS  
- [ ] Non — la page de connexion est servie via HTTP, permettant le détournement d'action de formulaire (form-action hijacking) ou le sniffing d'identifiants  

### Le flag `Secure` est-il appliqué à tous les cookies de session sensibles ?
- [ ] Oui — le flag `Secure` est **appliqué** à tous les cookies d'authentification et de session  
- [ ] Oui — le flag `Secure` est **appliqué** uniquement à certains cookies  
- [ ] Non — le flag `Secure` n'est **pas appliqué**, permettant aux cookies d'être exfiltrés via des requêtes non chiffrées  

### Le serveur prend-il en charge des versions TLS obsolètes ou des suites de chiffrement (cipher suites) non sécurisées ?
- [ ] Non — seuls le TLS moderne (1.2+) et des chiffrements forts sont **activés**  
- [ ] Oui — des versions héritées (TLS 1.0/1.1) ou des chiffrements faibles (RC4, DES, CBC) sont **supportés**  

### L'application empêche-t-elle la transmission d'identifiants vers des domaines tiers via des canaux non chiffrés ?
- [ ] Oui — aucune donnée sensible n'est envoyée vers des domaines externes ou tous les appels externes sont forcés via HTTPS  
- [ ] Non — des en-têtes d'authentification ou des identifiants sont envoyés à des points de terminaison tiers (ex: analytique ou SSO) via HTTP

---

## WSTG-ATHN-02 — Test des identifiants par défaut

Le test des identifiants par défaut consiste à identifier les interfaces d'administration, les services ou les composants d'application qui utilisent encore des noms d'utilisateur et des mots de passe définis en usine ou fournis par le fournisseur. Cette vulnérabilité est une cible prioritaire pour les attaquants car elle offre souvent une voie immédiate vers la compromission complète du système, un accès administratif ou une exécution de code à distance (Remote Code Execution) avec un effort minimal. Elle se produit généralement dans les logiciels du commerce, les plateformes CMS, les outils de gestion de bases de données et le matériel intégré comme les appareils IoT ou les équipements réseau. L'exploitation est généralement réalisée en recoupant les versions de logiciels identifiées avec des bases de données publiques de mots de passe par défaut ou en utilisant des outils de Credential Stuffing automatisés lors des phases initiales de reconnaissance et d'exploitation.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Critique / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Les interfaces d'administration ou de gestion sont-elles exposées sur Internet ou sur des segments de réseau non approuvés ?
- [ ] Non — aucune interface de gestion n'est accessible  
- [ ] Oui — des interfaces sont présentes mais limitées par des contrôles au niveau réseau (ex: VPN, IP Allowlist)  
- [ ] Oui — les interfaces **sont** accessibles publiquement et nécessitent une authentification  

### Les identifiants par défaut fournis par le fournisseur ont-ils été modifiés pour tous les composants identifiés ?
- [ ] Oui — tous les identifiants par défaut **ont été modifiés** sur l'ensemble des composants *(Le plus sécurisé)*  
- [ ] Oui — les identifiants **ont été modifiés** pour l'application principale, mais les composants secondaires (ex: CMS, DB) conservent les valeurs par défaut  
- [ ] Non — les identifiants par défaut **sont actifs** sur une ou plusieurs interfaces identifiables *(Critique)*  

### L'application impose-t-elle un changement de mot de passe lors de la première connexion pour les nouveaux comptes ou les comptes administratifs ?
- [ ] Oui — un changement de mot de passe **est obligatoire** avant toute action administrative  
- [ ] Non — l'application **permet** l'utilisation indéfinie des identifiants par défaut  

### Des mécanismes de verrouillage ou de limitation de débit (Rate Limiting) sont-ils actifs pour empêcher les tests automatisés d'identifiants par défaut ?
- [ ] Oui — un Rate Limiting agressif ou un verrouillage de compte (Lockout) **est appliqué**  
- [ ] Oui — des contrôles sont présents mais un contournement **est possible** via la manipulation d'en-têtes ou la rotation d'adresses IP  
- [ ] Non — aucun mécanisme de Rate Limiting ou de verrouillage n'est activé  

### Les plugins tiers, middlewares ou outils d'administration (ex: phpMyAdmin, Tomcat Manager) utilisent-ils des identifiants uniques ?
- [ ] Non — les composants tiers n'existent pas dans l'environnement  
- [ ] Oui — tous les composants tiers utilisent des identifiants uniques non définis par défaut  
- [ ] Non — au moins un composant tiers **est accessible** via des identifiants par défaut connus

---

## WSTG-ATHN-03 — Test du mécanisme de verrouillage de compte faible

Un mécanisme de verrouillage de compte (Account Lockout) est un contrôle de défense en profondeur critique conçu pour atténuer les attaques de Brute Force et de Credential Stuffing en désactivant un compte après un nombre spécifié de tentatives d'authentification échouées. Sans une politique de verrouillage robuste, les attaquants peuvent utiliser des outils automatisés pour deviner systématiquement les mots de passe sur plusieurs comptes (Password Spraying) ou cibler un seul compte à haute valeur jusqu'à réussir. Cette vulnérabilité se manifeste généralement sur les portails de connexion, les formulaires de réinitialisation de mot de passe et les points de terminaison d'API (API endpoints) qui manquent de Rate Limiting ou de protection au niveau du compte. Du point de vue d'un attaquant, un mécanisme de verrouillage faible ou absent permet des tentatives de mot de passe infinies, tandis qu'un mécanisme trop agressif peut être exploité pour provoquer un déni de service distribué (DDoS) contre des utilisateurs légitimes en verrouillant intentionnellement leurs comptes.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si l'application traite des données personnelles sensibles (PII), des données financières, ou si les comptes administratifs sont susceptibles d'être forcés par Brute Force sans aucun contrôle secondaire.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Un mécanisme de verrouillage de compte est-il appliqué après un nombre défini de tentatives échouées ?
- [ ] Oui — le compte est verrouillé après un seuil défini et raisonnable (ex: 5-10 tentatives)  
- [ ] Oui — le compte est verrouillé mais seulement après un nombre excessivement élevé de tentatives  
- [ ] Non — le compte **ne peut pas** être verrouillé et permet un Brute Force indéfini  

### Le mécanisme de verrouillage peut-il être contourné par des techniques d'évasion courantes ?
- [ ] Non — le verrouillage est appliqué côté serveur et n'est **pas** affecté par des modifications côté client  
- [ ] Oui — le verrouillage **peut être** contourné par la rotation d'adresses IP ou l'utilisation d'un pool de proxys  
- [ ] Oui — le verrouillage **peut être** contourné en manipulant les en-têtes HTTP comme `X-Forwarded-For` ou `User-Agent`  
- [ ] Oui — le verrouillage **peut être** contourné en effaçant les cookies ou en démarrant de nouvelles sessions  

### Le mécanisme de verrouillage prévoit-il une procédure de récupération de compte ou d'expiration ?
- [ ] Oui — le compte est automatiquement déverrouillé après une durée définie ou via une vérification par e-mail  
- [ ] Oui — le compte nécessite une intervention administrative pour être déverrouillé  
- [ ] Non — le verrouillage est permanent sans procédure de récupération claire, augmentant le risque de DoS  

### L'application fait-elle la distinction entre un nom d'utilisateur valide et invalide lors du verrouillage ?
- [ ] Non — la réponse de l'application est identique pour les utilisateurs existants et non existants  
- [ ] Oui — l'application révèle si un compte est verrouillé, permettant ainsi l'**énumération d'utilisateurs** (Username Enumeration)  

### Le mécanisme de verrouillage est-il sensible à une attaque par déni de service (DoS) ?
- [ ] Non — des contrôles tels que CAPTCHA ou le Rate Limiting basé sur l'IP empêchent le verrouillage de comptes en masse  
- [ ] Oui — un attaquant **peut** systématiquement verrouiller tous les utilisateurs connus en fournissant des mots de passe incorrects

---

## WSTG-ATHN-04 — Test de contournement du schéma d'authentification

Le contournement du schéma d'authentification consiste à identifier et à exploiter des failles dans la logique applicative permettant à un attaquant d'accéder à des ressources protégées sans fournir d'identifiants valides. Cette vulnérabilité survient généralement lorsque l'application s'appuie sur des contrôles côté client, ne parvient pas à appliquer l'autorisation côté serveur pour chaque requête, ou laisse des « backdoors » administratives et des points de terminaison (endpoints) de débogage exposés en production. Les attaquants exploitent ces faiblesses en effectuant du Forced Browsing vers des URL sensibles, en manipulant des paramètres HTTP tels que `authenticated=true`, ou en falsifiant des en-têtes (Spoofing) pour tromper l'application et lui faire croire qu'une session est déjà établie. Une exploitation réussie entraîne une rupture complète du périmètre d'authentification, pouvant conduire à une exfiltration de données non autorisée ou à un compromis total du système.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Les pages internes sensibles peuvent-elles être consultées directement sans session active ?
- [ ] Non — l'accès direct entraîne une redirection vers la page de connexion ou une réponse 403/404  
- [ ] Oui — certaines pages sensibles sont accessibles mais fournissent des données limitées ou non sensibles  
- [ ] Oui — des pages administratives ou spécifiques à l'utilisateur sensibles **sont** entièrement accessibles sans authentification *(Critique)*  

### L'application s'appuie-t-elle sur des indicateurs ou des paramètres côté client pour déterminer le statut d'authentification ?
- [ ] Non — le statut d'authentification est géré strictement via l'état de session côté serveur  
- [ ] Oui — des indicateurs côté client existent mais leur modification **ne donne pas** accès aux zones protégées  
- [ ] Oui — la modification de paramètres (ex. : `is_authenticated=true`, `user_role=admin`) **permet** le contournement de l'authentification  

### L'authentification peut-elle être contournée en falsifiant ou en manipulant des en-têtes HTTP spécifiques ?
- [ ] Non — les en-têtes tels que `X-Forwarded-For`, `Referer` ou les en-têtes personnalisés n'ont aucun impact sur l'authentification  
- [ ] Oui — la manipulation d'en-têtes (ex. : `X-Custom-IP-Authorization`, `X-Remote-User`) **est possible** et accorde un accès non autorisé  

### Existe-t-il des chemins alternatifs ou des artefacts de développement qui contournent le flux de connexion standard ?
- [ ] Non — toutes les ressources protégées sont filtrées par un contrôle d'authentification centralisé et prêt pour la production  
- [ ] Oui — des points de terminaison hérités (legacy), des routes de débogage (ex. : `/debug/login`) ou des versions d'API oubliées **sont** accessibles sans identifiants  

### L'application omet-elle de ré-authentifier ou de vérifier les sessions pour les actions à hauts privilèges ?
- [ ] Non — les actions à hauts privilèges ou sensibles nécessitent un jeton de session récent ou valide  
- [ ] Oui — une fois qu'un contournement est trouvé sur un point de terminaison, il **peut** être utilisé pour effectuer des actions administratives à travers toute l'application

---

## WSTG-ATHN-05 — Test de la fonctionnalité « Se souvenir de moi » vulnérable

La fonctionnalité « Se souvenir de moi » (Remember Me) ou d'authentification persistante est conçue pour maintenir l'état authentifié d'un utilisateur après le redémarrage du navigateur en stockant un jeton (token) ou un identifiant côté client. Si cette fonctionnalité est mal implémentée — par exemple en stockant des mots de passe en clair, des hachages (hashes) faibles ou des jetons à faible entropie dans des cookies — elle expose l'utilisateur à une prise de contrôle de compte (Account Takeover) via Cross-Site Scripting (XSS), un accès physique ou une interception réseau. Les testeurs d'intrusion (pentesters) évaluent l'entropie de ces jetons de persistance, les indicateurs de sécurité (flags) appliqués au mécanisme de stockage et le cycle de vie de la session pour s'assurer que la commodité de l'accès persistant ne contourne pas les contrôles de sécurité critiques tels que l'authentification multi-facteurs (MFA) ou l'invalidation de session. L'exploitation implique souvent la collecte de ces jetons à longue durée de vie pour obtenir un accès non autorisé aux comptes sans nécessiter les identifiants d'origine.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des identifiants en clair ou des hachages non salés sont stockés dans le cookie persistant, ou si le jeton permet de contourner l'authentification multi-facteurs (MFA).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### L'application implémente-t-elle une fonctionnalité « Se souvenir de moi » ou « Rester connecté » ?
- [ ] Non — la fonctionnalité **n'existe pas** dans l'application  
- [ ] Oui — la fonctionnalité existe et utilise des jetons cryptographiquement forts et imprévisibles  
- [ ] Oui — la fonctionnalité existe mais utilise des jetons prévisibles ou à faible entropie *(Moyenne)*  
- [ ] Oui — la fonctionnalité existe et stocke des identifiants en clair ou des mots de passe encodés en base64 *(Haute)*  

### Les indicateurs de sécurité (flags) sont-ils correctement appliqués au cookie persistant ?
- [ ] Oui — les flags `HttpOnly`, `Secure` et `SameSite` sont **appliqués**  
- [ ] Oui — certains flags sont appliqués mais `HttpOnly` est **manquant**  
- [ ] Oui — certains flags sont appliqués mais `Secure` est **manquant** sur HTTPS  
- [ ] Non — aucun indicateur de sécurité n'est **appliqué** au cookie persistant  

### Le jeton « Se souvenir de moi » reste-t-il valide après une déconnexion manuelle ?
- [ ] Non — le jeton est invalidé côté serveur immédiatement après la déconnexion  
- [ ] Oui — le jeton reste valide mais nécessite un mot de passe pour les actions sensibles  
- [ ] Oui — le jeton reste valide et permet un accès complet au compte **sans** réauthentification *(Moyenne)*  

### La session persistante est-elle interrompue lors d'un changement de mot de passe ?
- [ ] Oui — tous les jetons persistants sont révoqués lorsque l'utilisateur change son mot de passe  
- [ ] Non — le jeton « Se souvenir de moi » reste valide après qu'un changement ou une réinitialisation de mot de passe **a été effectué** *(Haute)*  

### Le jeton persistant contourne-t-il l'authentification multi-facteurs (MFA) lors des visites ultérieures ?
- [ ] Non — la MFA est requise même lorsqu'un jeton « Se souvenir de moi » est présent  
- [ ] Oui — la MFA est contournée, mais le jeton est lié à une empreinte numérique d'appareil (device fingerprint) spécifique  
- [ ] Oui — la MFA est **entièrement contournée** en utilisant uniquement le cookie persistant depuis n'importe quel appareil *(Haute)*

---

## WSTG-ATHN-06 — Testing for Browser Cache Weakness

Une faiblesse de la mise en cache du navigateur (Browser cache weakness) survient lorsque des informations sensibles sont stockées dans le cache local du navigateur et peuvent être récupérées par un utilisateur non autorisé ayant accès à la même machine physique. Cette absence d'en-têtes de contrôle de cache appropriés permet à des données potentiellement sensibles, telles que des informations personnelles, des détails de compte ou des identifiants de session (Session identifiers), de persister après que l'utilisateur s'est déconnecté ou a fermé sa session. Des attaquants ou des utilisateurs ultérieurs sur des terminaux partagés ou publics peuvent exploiter cela en naviguant dans l'historique du navigateur, en utilisant le bouton « Retour » ou en inspectant les fichiers du cache local pour exfiltrer les réponses mises en cache. S'assurer de la mise en œuvre de directives strictes telles que `Cache-Control: no-store` et `Pragma: no-cache` est critique pour toute application gérant du contenu authentifié afin de garantir que les données sensibles ne soient jamais écrites sur le disque.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **État du test** | Non effectué |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité devient Moyenne si l'application est fréquemment consultée depuis des bornes partagées/publiques ou si elle contient des données PII/PHI hautement sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Outils :** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Les en-têtes de contrôle de cache appropriés sont-ils présents sur les pages authentifiées ou sensibles ?
- [ ] Oui — `Cache-Control: no-store, no-cache` et `Pragma: no-cache` sont **présents** et **correctement configurés**  
- [ ] Oui — certains en-têtes sont présents mais `no-store` est **manquant**, permettant la mise en cache sur disque  
- [ ] Non — aucun en-tête lié au cache n'est **appliqué**, reposant sur le comportement par défaut du navigateur  

### L'application utilise-t-elle la directive `private` pour les données spécifiques à l'utilisateur ?
- [ ] Oui — `Cache-Control: private` est **activé** pour tout le contenu personnalisé afin d'empêcher la mise en cache par des proxys  
- [ ] Non — le contenu est marqué comme `public` ou ne contient pas la directive `private`, ce qui le rend **possible à mettre en cache** par des proxys intermédiaires  

### Les informations sensibles sont-elles accessibles via le bouton « Retour » du navigateur après une déconnexion réussie ?
- [ ] Non — le navigateur demande une réauthentification ou la page n'est **pas chargée** depuis le cache local  
- [ ] Oui — les informations sensibles sont **toujours visibles** via le bouton retour après la fin de la session  

### Les fichiers non-HTML sensibles (ex. PDF, exports CSV, réponses API JSON) sont-ils protégés contre la mise en cache ?
- [ ] Non — aucun fichier non-HTML sensible n'est généré ou géré par l' application  
- [ ] Oui — des en-têtes de contrôle de cache stricts sont **appliqués** à tous les téléchargements de fichiers sensibles et aux points de terminaison API (API endpoints)  
- [ ] Non — les téléchargements sensibles ou les réponses API sont **stockés** dans le cache local du navigateur ou dans des répertoires temporaires  

### L'application utilise-t-elle `Expires: 0` ou une date passée pour empêcher l'utilisation de données périmées ?
- [ ] Oui — l'en-tête `Expires` est défini sur `0` ou une date historique, **garantissant** que le navigateur traite le contenu comme immédiatement périmé  
- [ ] Non — l'en-tête `Expires` est **manquant** ou défini sur une date future pour les pages sensibles

---

## WSTG-ATHN-07 — Test de politique de mots de passe faible

Le test des politiques de mots de passe faibles consiste à évaluer les exigences de complexité, de longueur et d'entropie appliquées par une application lors de la création de compte et de la mise à jour des identifiants. Des politiques insuffisantes compromettent directement la confidentialité des comptes utilisateurs en les rendant vulnérables aux attaques par dictionnaire automatisées, aux tentatives de Brute Force et au Credential Stuffing. Ces vulnérabilités résident généralement dans les points de terminaison d'inscription, les flux de réinitialisation de mot de passe et les panneaux de gestion administrative des utilisateurs. Du point de vue d'un attaquant, une politique permissive réduit considérablement le coût de calcul et le temps nécessaire pour compromettre avec succès les identifiants en utilisant des listes de mots courantes ou du cassage basé sur des modèles.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Faible* |

> *La sévérité devient Élevée (High) si l'application manque de mécanismes de verrouillage de compte (account lockout) ou de Rate Limiting pour empêcher les tentatives de devinette automatisées.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Outils :** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Une longueur minimale de mot de passe est-elle imposée par l'application ?
- [ ] Oui — la longueur minimale est de 12 caractères ou plus *(Le plus sécurisé)*  
- [ ] Oui — la longueur minimale est comprise entre 8 et 11 caractères  
- [ ] Oui — la longueur minimale est **inférieure à** 8 caractères  
- [ ] Non — aucune longueur minimale n'est imposée  

### Des exigences de complexité (majuscules, minuscules, chiffres, symboles) sont-elles imposées ?
- [ ] Oui — plusieurs classes de caractères sont obligatoires et un contournement n'est **pas possible**  
- [ ] Oui — les classes de caractères sont suggérées mais **non** imposées  
- [ ] Non — aucune exigence de complexité n'est appliquée  

### L'application bloque-t-elle les mots de passe courants/faibles ou les mots de passe contenant des données spécifiques à l'utilisateur ?
- [ ] Oui — les mots de passe faibles connus (ex. : "password123") et les mots de passe basés sur le nom d'utilisateur **ne peuvent pas** être utilisés  
- [ ] Oui — les mots de passe courants sont bloqués, mais les données spécifiques à l'utilisateur (ex. : nom d'utilisateur, email) **peuvent** être utilisées  
- [ ] Non — toute chaîne de caractères répondant aux exigences de longueur/complexité est acceptée  

### Les exigences de complexité ou de longueur peuvent-elles être contournées via une modification côté client ?
- [ ] Non — les politiques sont strictement appliquées côté serveur (**server-side**)  
- [ ] Oui — les politiques sont appliquées via JavaScript et **peuvent** être contournées en interceptant la requête  

### La politique de mot de passe est-elle clairement communiquée à l'utilisateur sans divulguer de détails d'implémentation ?
- [ ] Oui — la politique est visible lors de la saisie et fournit des messages d'erreur génériques  
- [ ] Oui — la politique est visible mais les messages d'erreur révèlent des expressions régulières (Regex) spécifiques ou des failles logiques  
- [ ] Non — la politique n'est **pas** visible, forçant une découverte par tâtonnement (trial-and-error)

---

## WSTG-ATHN-08 — Test des réponses aux questions de sécurité faibles

Le test des réponses aux questions de sécurité faibles consiste à évaluer les mécanismes d'authentification basée sur la connaissance (KBA - Knowledge-Based Authentication) utilisés pour vérifier l'identité d'un utilisateur, généralement lors des flux de récupération de mot de passe ou d'authentification multi-facteurs. Cela est important car les questions de sécurité reposent souvent sur des informations statiques et non secrètes qui peuvent être facilement découvertes via le renseignement d'origine source ouverte (OSINT) ou l'ingénierie sociale, menant à une prise de contrôle de compte (Account Takeover) non autorisée. Ces vulnérabilités surviennent généralement dans les modules « Mot de passe oublié » ou « Déverrouillage de compte » où les utilisateurs fournissent des réponses à des questions prédéfinies. Du point de vue d'un attaquant, il s'agit d'une cible de choix pour le Brute Force automatisé ou la recherche ciblée, car de nombreux utilisateurs fournissent des réponses prévisibles ou vérifiables publiquement à des questions courantes telles que « Quel est le nom de jeune fille de votre mère ? » ou « Quel lycée avez-vous fréquenté ? ».


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si la question de sécurité est l'unique condition pour une réinitialisation complète du mot de passe et une prise de contrôle du compte sans vérification supplémentaire.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Outils :** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### L'application implémente-t-elle des questions de sécurité pour la vérification de l'identité ou la récupération de mot de passe ?
- [ ] Non — les questions de sécurité ne sont **pas utilisées** pour l'authentification ou la récupération  
- [ ] Oui — les questions de sécurité sont **utilisées** comme facteur secondaire optionnel  
- [ ] Oui — les questions de sécurité sont **utilisées** comme mécanisme de récupération principal/unique *(Risque élevé)*  

### Les questions de sécurité sont-elles sélectionnées dans une liste fixe ou sont-elles définies par l'utilisateur ?
- [ ] Non — les questions sont entièrement définies par l'utilisateur et nécessitent une entropie élevée  
- [ ] Oui — les questions sont fixes mais incluent des options non découvrables par OSINT  
- [ ] Oui — les questions proviennent d'une liste fixe de sujets courants et découvrables par OSINT *(Vulnérable)*  

### Un Rate Limiting ou un verrouillage de compte est-il appliqué aux tentatives de réponse aux questions de sécurité ?
- [ ] Oui — un Rate Limiting strict et un verrouillage de compte **sont appliqués** pour empêcher le Brute Force  
- [ ] Oui — le Rate Limiting **est appliqué** mais un contournement **est possible** via la rotation d'IP ou la manipulation d'en-têtes  
- [ ] Non — **aucun Rate Limiting** n'est appliqué sur les tentatives de réponse  

### L'application impose-t-elle une complexité ou une validation sur le contenu des réponses ?
- [ ] Oui — la validation empêche les mots courants et garantit la complexité/l'unicité des réponses  
- [ ] Oui — une validation de base est présente mais **peut** être contournée avec des listes de mots courantes ou des attaques par dictionnaire  
- [ ] Non — **aucune validation** n'est effectuée ; les réponses simples ou à un seul caractère **sont autorisées**  

### Le défi de la question de sécurité peut-il être contourné par des failles logiques ?
- [ ] Non — le flux de récupération nécessite strictement une réponse valide pour continuer  
- [ ] Oui — le flux de récupération **peut** être contourné en manipulant les codes de réponse HTTP ou les paramètres de session  
- [ ] Oui — la réponse est reflétée dans le code source ou dans des champs cachés de la page

---

## WSTG-ATHN-09 — Test des fonctionnalités de changement ou de réinitialisation de mot de passe faibles

Les mécanismes de changement et de réinitialisation de mot de passe sont des composants critiques de la gestion de l'authentification qui, s'ils sont mal implémentés, permettent à des attaquants de prendre le contrôle de comptes utilisateurs (Account Takeover). Ces vulnérabilités impliquent généralement des jetons (Tokens) de réinitialisation prévisibles, une absence de verrouillage de compte (Account Lockout) lors de tentatives de Brute Force, un acheminement non sécurisé des jetons, ou la possibilité de changer de mot de passe sans vérifier le mot de passe actuel. Les attaquants exploitent ces failles en interceptant ou en devinant les liens de récupération, en effectuant une Host Header Injection pour rediriger les e-mails de réinitialisation, ou en utilisant la fixation de session (Session Fixation) pour maintenir l'accès après un changement de mot de passe. Garantir que ces processus nécessitent une vérification forte et utilisent un caractère aléatoire cryptographique (cryptographic randomness) est essentiel pour maintenir l'intégrité du compte et prévenir les accès non autorisés.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Le mot de passe actuel est-il requis pour définir un nouveau mot de passe lors d'un changement standard ?
- [ ] Oui — le mot de passe actuel **est requis** et vérifié  
- [ ] Oui — le mot de passe actuel **est requis** mais peut être contourné via une Parameter Pollution ou une manipulation de paramètres  
- [ ] Non — le mot de passe actuel **n'est pas** demandé, permettant une prise de contrôle de compte via Session Hijacking *(Élevée)*  

### Les jetons de réinitialisation de mot de passe sont-ils cryptographiquement sécurisés et imprévisibles ?
- [ ] Oui — les jetons ont une entropie élevée, sont aléatoires et **ne sont pas prévisibles**  
- [ ] Oui — les jetons sont générés mais présentent une faible entropie ou des motifs prévisibles (ex : basés sur l'horodatage)  
- [ ] Non — les jetons sont **prévisibles** ou basés sur des données utilisateur statiques comme l'e-mail/ID encodé en Base64 *(Critique)*  

### Le lien de réinitialisation de mot de passe peut-il être redirigé via Host Header Injection ?
- [ ] Non — l'application ignore ou valide l'en-tête `Host` pour la génération du lien  
- [ ] Oui — l'application utilise l'en-tête `Host` ou `X-Forwarded-Host` pour construire les liens, mais une validation **est présente**  
- [ ] Oui — les liens **peuvent** être redirigés vers un domaine contrôlé par l'attaquant via la manipulation d'en-têtes *(Élevée)*  

### Le jeton de réinitialisation a-t-il une durée de vie limitée et une invalidation immédiate ?
- [ ] Oui — les jetons expirent rapidement et sont invalidés **immédiatement** après utilisation ou changement de mot de passe  
- [ ] Oui — les jetons expirent après une longue durée (ex : 24h+) ou permettent **plusieurs utilisations**  
- [ ] Non — les jetons **n'expirent jamais** ou restent valides après que le mot de passe a été modifié avec succès  

### Existe-t-il des limites de débit (Rate Limiting) ou des verrouillages sur les points de terminaison de réinitialisation et de changement de mot de passe ?
- [ ] Oui — un Rate Limiting strict et des CAPTCHAs **sont appliqués** pour empêcher l'énumération et le Brute Force  
- [ ] Oui — des contrôles limités existent mais un contournement **est possible** via la rotation d'IP ou le spoofing d'en-tête  
- [ ] Non — aucun Rate Limiting **n'est appliqué**, permettant l'énumération de comptes en masse ou le Brute Force de jetons

---

## WSTG-ATHN-10 — Test d'authentification affaiblie dans les canaux alternatifs

Le test d'authentification affaiblie dans les canaux alternatifs consiste à identifier et à analyser les voies secondaires d'accès aux comptes—telles que les API mobiles, les workflows de récupération de mot de passe, les interfaces du centre d'assistance ou les sous-domaines hérités (Legacy)—qui pourraient ne pas appliquer la même rigueur sécuritaire que le portail web principal. Ces canaux alternatifs manquent souvent d'authentification multi-facteurs (MFA) robuste, de limitation de débit (Rate Limiting) stricte ou d'exigences de complexité de mot de passe, créant ainsi un « maillon faible » qui compromet l'ensemble du système d'authentification. Les attaquants ciblent spécifiquement ces points d'entrée négligés pour effectuer du bourrage d'identifiants (Credential Stuffing) ou contourner la MFA en exploitant les divergences dans la manière dont les différentes interfaces gèrent la vérification d'identité. Garantir la parité sur toutes les surfaces d'authentification est essentiel pour empêcher les accès non autorisés et maintenir l'intégrité de la session et des données de l'utilisateur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Des canaux d'authentification alternatifs sont-ils présents et identifiés ?
- [ ] Non — un seul canal d'authentification existe  
- [ ] Oui — plusieurs canaux identifiés (par exemple : API d'application mobile, client de bureau, portail hérité, services SOAP)  

### Les canaux alternatifs appliquent-ils une parité de sécurité avec le canal principal ?
- [ ] Oui — tous les canaux imposent une complexité de mot de passe et des exigences MFA **identiques**  
- [ ] Oui — les canaux imposent la complexité du mot de passe mais la MFA n'est **pas requise** ou **peut** être contournée  
- [ ] Non — les canaux alternatifs ont des exigences d'authentification **nettement plus faibles**  

### La limitation de débit (Rate Limiting) et le verrouillage de compte sont-ils cohérents sur tous les canaux ?
- [ ] Oui — la limitation de débit et le verrouillage sont **appliqués de manière cohérente** sur tous les points de terminaison  
- [ ] Oui — des contrôles sont présents mais un contournement **est possible** sur des canaux spécifiques (par exemple : API mobile)  
- [ ] Non — la limitation de débit ou le verrouillage n'est **pas appliqué** aux voies d'authentification secondaires  

### La MFA peut-elle être contournée en passant à un canal alternatif ?
- [ ] Non — la MFA est obligatoire et **ne peut pas** être contournée quel que soit le point d'entrée  
- [ ] Oui — la MFA est requise sur le portail web mais **n'est pas appliquée** sur l'API mobile ou l'API héritée  

### Les flux de récupération de mot de passe ou de « mot de passe oublié » sont-ils plus faibles que la connexion standard ?
- [ ] Non — les flux de récupération utilisent une vérification hors bande (out-of-band) forte et ne divulguent pas d'informations  
- [ ] Oui — les flux de récupération utilisent des questions de sécurité faibles qui **peuvent** être facilement devinées ou recherchées  
- [ ] Oui — les flux de récupération sont vulnérables à l'énumération ou **ne requièrent pas** le même niveau de preuve qu'une connexion standard

---

## WSTG-ATHN-11 — Test de l'authentification multi-facteurs (MFA)

Le test de l'authentification multi-facteurs (MFA) évalue la robustesse de la couche de sécurité secondaire conçue pour empêcher l'accès non autorisé, même lorsque les identifiants principaux sont compromis. Les attaquants tentent fréquemment de contourner la MFA en identifiant des points de terminaison où elle est appliquée de manière incohérente, tels que des versions d'API héritées, des backends mobiles ou des workflows de réinitialisation de mot de passe. L'exploitation implique souvent la manipulation des réponses du serveur, le brute-forcing de codes à courte durée de vie (OTP), ou l'exploitation de conditions de concurrence (race conditions) et de failles de gestion de session qui permettent à un utilisateur de passer à un état authentifié sans fournir le second facteur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### La MFA est-elle appliquée de manière cohérente sur tous les portails d'authentification ?
- [ ] Oui — La MFA est requise pour toutes les tentatives de connexion via le web, le mobile et les API.  
- [ ] Oui — mais la MFA **n'est pas appliquée** sur des points de terminaison spécifiques (ex : `/api/v1/login` hérité ou portails spécifiques au mobile).  
- [ ] Non — La MFA **n'est pas implémentée** dans l'application *(Critique)*.  

### L'étape de vérification MFA peut-elle être contournée via une navigation directe par URL ou une manipulation de réponse ?
- [ ] Non — la navigation directe ou la manipulation de paramètres **ne peuvent pas** contourner le défi.  
- [ ] Oui — la navigation directe vers des URL de tableaux de bord internes **est possible** sans compléter le défi MFA.  
- [ ] Oui — la manipulation de la réponse du serveur (ex : changer un `401 Unauthorized` en `200 OK`) **est possible** pour obtenir l'accès.  

### Le processus de vérification du code MFA est-il protégé contre les attaques par force brute (Brute Force) ?
- [ ] Oui — un Rate Limiting strict ou un verrouillage de compte **est appliqué** après plusieurs tentatives d'OTP échouées.  
- [ ] Oui — le Rate Limiting existe mais **il est possible** de le contourner via la rotation d'adresses IP ou la manipulation d'en-têtes.  
- [ ] Non — aucun Rate Limiting n'est appliqué, et le Brute Force automatisé des codes **est possible**.  

### L'application maintient-elle un état de session sécurisé pendant la transition MFA ?
- [ ] Oui — des jetons de session à hauts privilèges sont émis **uniquement après** la réussite de la MFA.  
- [ ] Non — un cookie de session entièrement fonctionnel est émis **avant** que la MFA ne soit complétée, permettant l'accès à certaines fonctionnalités.  
- [ ] Non — l'application utilise un identifiant statique ou une transition de session prévisible qui **peut** être détournée.  

### Les facteurs MFA alternatifs (SMS, Email, codes de secours) sont-ils vulnérables à l'exploitation ?
- [ ] Non — tous les facteurs utilisent des valeurs sécurisées, non prévisibles et limitées dans le temps.  
- [ ] Oui — les codes de secours sont prévisibles ou **peuvent** être énumérés.  
- [ ] Oui — la fonctionnalité "Renvoyer le code" peut être détournée pour effectuer un flooding SMS/Email ou révéler des détails de contact partiels.

---

## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Les vulnérabilités de traversée de répertoire (Directory Traversal) et d'inclusion de fichiers surviennent lorsqu'une application utilise une entrée contrôlable par l'utilisateur pour construire des chemins vers des fichiers ou des répertoires sans validation ou assainissement (sanitization) suffisants. Les attaquants exploitent ces failles en injectant des séquences telles que `../` pour naviguer en dehors du répertoire prévu, accédant potentiellement à des fichiers système sensibles, des données de configuration ou au code source de l'application. Dans des cas plus graves impliquant l'inclusion de fichiers locaux (LFI) ou l'inclusion de fichiers distants (RFI), un attaquant peut parvenir à une exécution de code à distance (RCE) en incluant des scripts malveillants ou en exploitant l'empoisonnement de logs (log poisoning) et les wrappers PHP. Ces vulnérabilités résident généralement dans les paramètres utilisés pour le chargement dynamique de contenu, les moteurs de template ou les points de terminaison de récupération d'images où la logique côté serveur gère les chemins du système de fichiers.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Statut du test** | Non effectué |
| **Sévérité** | Haute / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Outils :** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Les paramètres acceptent-ils des noms de fichiers ou des chemins pour un traitement côté serveur ?
- [ ] Non — aucun paramètre ne semble interagir avec le système de fichiers  
- [ ] Oui — des paramètres existent mais utilisent une liste d'autorisation (allowlist) stricte d'identifiants de fichiers  
- [ ] Oui — les paramètres acceptent directement des noms de fichiers ou des chemins  

### L'entrée est-elle assainie pour empêcher les séquences de traversée de répertoire ?
- [ ] Oui — l'entrée est validée par rapport à une liste d'autorisation stricte et les séquences de traversée ne sont **pas possibles**  
- [ ] Oui — l'entrée est assainie en supprimant les séquences `../`, mais un contournement récursif **est possible**  
- [ ] Non — aucun assainissement ou validation n'est **appliqué** aux entrées liées aux chemins  

### Est-il possible d'accéder à des fichiers en dehors du répertoire restreint via des séquences de traversée ?
- [ ] Non — l'application ou l'OS empêche l'accès aux fichiers en dehors du périmètre défini  
- [ ] Oui — l'accès aux fichiers à l'intérieur de la racine web (web root) **est possible**  
- [ ] Oui — l'accès à des fichiers système sensibles (ex : `/etc/passwd`, `C:\Windows\win.ini`) **est possible** *(Critique)*  

### Le serveur permet-il l'inclusion d'URLs distantes (RFI) ?
- [ ] Non — l'inclusion de fichiers distants est **désactivée** au niveau du serveur/de l'application  
- [ ] Oui — des fichiers distants **peuvent** être inclus mais l'exécution n'est **pas possible**  
- [ ] Oui — des fichiers distants **peuvent** être inclus et exécutés sur le serveur *(Critique)*  

### Les filtres peuvent-ils être contournés en utilisant l'encodage ou des caractères spéciaux ?
- [ ] Non — les filtres gèrent efficacement divers encodages et les octets nuls (null bytes)  
- [ ] Oui — le contournement **est possible** en utilisant l'encodage URL, le double encodage URL ou l'Unicode 16 bits  
- [ ] Oui — le contournement **est possible** en utilisant l'injection d'octets nuls (`%00`) ou des wrappers de système de fichiers (ex : `php://filter`)

---

## WSTG-ATHZ-02 — Test de contournement du schéma d'autorisation

Le contournement des schémas d'autorisation se produit lorsqu'une application ne parvient pas à appliquer les contrôles d'accès, permettant à un attaquant d'accéder à des ressources ou des fonctions en dehors de ses permissions prévues. Cette vulnérabilité se manifeste généralement sous la forme d'une Escalade de Privilèges Horizontale (Horizontal Privilege Escalation), où un attaquant accède aux données appartenant à un autre utilisateur de même rang, ou d'une Escalade de Privilèges Verticale (Vertical Privilege Escalation), où un utilisateur peu privilégié obtient des capacités administratives. Les attaquants exploitent ces failles en manipulant des paramètres de requête tels que les IDs de ressources, en modifiant les jetons de session (session tokens) ou en exploitant des en-têtes HTTP comme `X-Original-URL` pour contourner les restrictions basées sur le chemin. Assurer une autorisation robuste est essentiel pour maintenir la confidentialité des données et empêcher des actions administratives non autorisées qui pourraient compromettre l'ensemble du système.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Outils :** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### L'escalade de privilèges horizontale est-elle empêchée entre les utilisateurs de même rôle ?
- [ ] Oui — les contrôles d'autorisation **sont appliqués** à chaque demande de ressource et le contournement n'est **pas possible**  
- [ ] Oui — les contrôles d'autorisation **sont appliqués** mais peuvent être contournés via IDOR ou manipulation de paramètres  
- [ ] Non — les utilisateurs **peuvent** accéder aux données appartenant à d'autres utilisateurs en changeant simplement un ID de ressource *(Élevée)*  

### L'escalade de privilèges verticale est-elle empêchée pour les utilisateurs peu privilégiés ?
- [ ] Non — l'application n'a qu'un seul niveau de privilège  
- [ ] Oui — les fonctions administratives **ne peuvent pas** être accédées par des utilisateurs peu privilégiés  
- [ ] Oui — les fonctions administratives sont masquées dans l'interface utilisateur mais **peuvent** être accédées via une URL directe  
- [ ] Non — les utilisateurs peu privilégiés **peuvent** effectuer des actions administratives en manipulant les rôles ou les permissions *(Critique)*  

### L'autorisation peut-elle être contournée en utilisant la manipulation de verbes HTTP (HTTP verb tampering) ?
- [ ] Non — les contrôles d'accès sont appliqués indépendamment de la méthode HTTP utilisée  
- [ ] Oui — changer la méthode (par exemple, de `GET` à `POST` ou `PUT`) **contourne** les contrôles d'autorisation  

### Les points de terminaison (endpoints) administratifs ou restreints sont-ils accessibles sans aucune authentification ?
- [ ] Non — tous les endpoints sensibles nécessitent une session valide et les permissions appropriées  
- [ ] Oui — certains endpoints administratifs sont accessibles si l'attaquant connaît l'URL directe  
- [ ] Oui — l'accès non authentifié à des fonctions sensibles **est possible** *(Critique)*  

### Les en-têtes HTTP permettent-ils de contourner l'autorisation basée sur le chemin ?
- [ ] Non — l'application n'est **pas** sensible aux contournements basés sur les en-têtes  
- [ ] Oui — des en-têtes tels que `X-Original-URL`, `X-Rewrite-URL` ou `X-Forwarded-For` **peuvent** être utilisés pour contourner les contrôles d'accès

---

## WSTG-ATHZ-03 — Test de l'élévation de privilèges

L'élévation de privilèges (Privilege Escalation) se produit lorsqu'un attaquant exploite des vulnérabilités pour accéder à des ressources ou des fonctionnalités réservées à des utilisateurs disposant de niveaux d'autorisation plus élevés ou d'identités différentes. Dans l'escalade verticale, un utilisateur standard tente d'accéder à des fonctions administratives, tandis que l'escalade horizontale implique l'accès à des données appartenant à un autre utilisateur ayant le même niveau de privilège. Ces failles se manifestent généralement par des listes de contrôle d'accès (ACL) mal implémentées, des références directes non sécurisées à des objets (IDOR) ou la manipulation de paramètres au sein des jetons de session ou d'identité. Du point de vue d'un attaquant, cela est souvent réalisé en manipulant des identifiants numériques, en modifiant des paramètres basés sur les rôles dans les cookies ou les JWT, ou en appelant directement des points de terminaison d'API (API endpoints) masqués qui ne font l'objet d'aucun contrôle d'autorisation côté serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Outils :** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### L'application implémente-t-elle plusieurs niveaux de privilèges ou le multi-tenancy ?
- [ ] Non — l'application est un système à utilisateur unique ou à rôle unique  
- [ ] Oui — plusieurs rôles ou locataires (tenants) **sont** définis et nécessitent des tests d'autorisation  

### Une élévation de privilèges horizontale (accès de même niveau) est-elle possible ?
- [ ] Non — les contrôles d'autorisation **sont appliqués** à tous les identifiants de ressources et propriétaires de sessions  
- [ ] Oui — l'accès aux données d'autres utilisateurs **est possible** via IDOR ou manipulation de paramètres  
- [ ] Oui — la fuite de données **est possible** mais la modification des données d'autres utilisateurs **n'est pas possible**  

### Une élévation de privilèges verticale (bas vers haut) est-elle possible ?
- [ ] Non — les points de terminaison administratifs appliquent strictement les contrôles d'accès basés sur les rôles (RBAC)  
- [ ] Oui — les fonctions administratives **peuvent** être accédées par des utilisateurs peu privilégiés via un accès direct par URL  
- [ ] Oui — les fonctions administratives **peuvent** être accédées en manipulant des en-têtes liés aux rôles, des cookies ou des revendications JWT  

### Les rôles ou permissions des utilisateurs peuvent-ils être modifiés via Mass Assignment ou Parameter Pollution ?
- [ ] Non — les champs basés sur les rôles sont strictement en lecture seule et **ne peuvent pas** être modifiés par les utilisateurs  
- [ ] Oui — les permissions des utilisateurs **peuvent** être élevées en incluant des paramètres cachés (ex: `role=admin`) lors des mises à jour de profil ou de l'inscription  

### Les contrôles d'autorisation sont-ils appliqués de manière cohérente sur toutes les versions d'API et méthodes HTTP ?
- [ ] Oui — les contrôles **sont appliqués** de manière cohérente sur toutes les versions et méthodes  
- [ ] Non — les versions d'API héritées (ex: `/v1/`) ou des méthodes HTTP spécifiques (ex: `PUT`, `DELETE`, `PATCH`) **contournent** l'autorisation  
- [ ] Non — les fonctions administratives sont masquées dans l'interface utilisateur mais **activées** au niveau de l'API pour tous les utilisateurs

---

## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Les Insecure Direct Object References (IDOR) se produisent lorsqu'une application fournit un accès direct aux objets en se basant sur une entrée fournie par l'utilisateur sans effectuer de contrôle d'autorisation pour vérifier les permissions du demandeur. Cette vulnérabilité impacte de manière significative la confidentialité et l'intégrité, car elle permet à des attaquants de consulter, modifier ou supprimer des données appartenant à d'autres utilisateurs ou au système. Elle se manifeste généralement dans les paramètres d'URL, les données du corps POST ou les clés JSON qui font référence à des clés de base de données internes, des noms de fichiers ou des identifiants de compte. Du point de vue d'un attaquant, l'exploitation implique la manipulation de ces identifiants — souvent par l'incrémentation d'entiers ou le fuzzing d'UUID — pour accéder à des ressources en dehors de leur périmètre autorisé.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Outils :** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### L'application utilise-t-elle des identifiants d'objets directs dans les paramètres de requête ?
- [ ] Non — l'application utilise des références indirectes ou des mappages chiffrés *(Plus sécurisé)*  
- [ ] Oui — l'application utilise des identifiants non prévisibles (ex. UUID longs/hashs)  
- [ ] Oui — l'application utilise des identifiants prévisibles (ex. entiers incrémentaux ou noms d'utilisateur)  

### Un contrôle d'autorisation côté serveur est-il effectué pour chaque requête impliquant un identifiant d'objet ?
- [ ] Oui — les contrôles côté serveur **ne peuvent pas** être contournés pour aucun identifiant  
- [ ] Oui — les contrôles côté serveur sont présents mais un contournement **est possible** via la parameter pollution ou la manipulation d'en-têtes  
- [ ] Non — aucun contrôle d'autorisation **n'est appliqué** pour vérifier la propriété de l'objet demandé  

### Un attaquant peut-il accéder aux objets appartenant à d'autres utilisateurs ou les modifier (IDOR horizontal) ?
- [ ] Non — l'accès aux données d'autres utilisateurs **n'est pas possible**  
- [ ] Oui — la consultation des données d'autres utilisateurs **est possible** mais la modification **ne l'est pas**  
- [ ] Oui — la consultation et la modification des données d'autres utilisateurs **sont possibles** *(Critique)*  

### Est-il possible d'accéder à des objets de niveau administratif ou système (IDOR vertical) ?
- [ ] Non — l'accès à des objets dotés de privilèges supérieurs **n'est pas possible**  
- [ ] Oui — l'accès aux configurations administratives ou aux fichiers système **est possible**  

### L'application divulgue-t-elle des identifiants d'objets via la recherche, les métadonnées ou d'autres points de terminaison ?
- [ ] Non — les identifiants **ne sont pas exposés** involontairement  
- [ ] Oui — les identifiants d'autres utilisateurs/objets **peuvent** être énumérés via des points de terminaison secondaires ou des profils publics

---

## WSTG-ATHZ-05 — Test des faiblesses OAuth

Les faiblesses OAuth englobent une variété de failles dans l'implémentation des protocoles OAuth 2.0 ou OpenID Connect (OIDC), entraînant souvent une prise de contrôle totale de compte (Account Takeover) ou un accès non autorisé aux données. Ces vulnérabilités se manifestent généralement lors du flux d'autorisation (Authorization Flow), spécifiquement concernant la validation de l'URI de redirection (`redirect_uri`), l'entropie et la vérification du paramètre `state`, et la manipulation sécurisée des jetons d'accès (Access Tokens) ou d'identité (ID Tokens). Les attaquants exploitent ces failles en interceptant les codes d'autorisation, en redirigeant les jetons vers des domaines contrôlés par l'attaquant via des redirections ouvertes (Open Redirects), ou en effectuant des falsifications de requête intersites (CSRF) pour lier la session d'une victime au compte d'un attaquant. Étant donné qu'OAuth sert souvent de mécanisme d'authentification principal, une seule mauvaise configuration peut compromettre l'intégrité de l'ensemble du système d'identité des utilisateurs.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Outils :** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Le paramètre `state` est-il implémenté et validé pour prévenir le CSRF ?
- [ ] Oui — `state` est obligatoire, unique par session et strictement validé sur le serveur  
- [ ] Oui — `state` est présent mais statique ou prévisible entre différentes sessions  
- [ ] Oui — `state` est présent mais l'application **ne le valide pas** lors du rappel (Callback)  
- [ ] Non — le paramètre `state` **n'est pas** utilisé dans la requête d'autorisation *(Critique)*  

### L'URI de redirection (`redirect_uri`) est-elle strictement validée par rapport à une liste blanche ?
- [ ] Oui — seules les correspondances exactes avec une liste blanche (Whitelist) pré-enregistrée sont **acceptées**  
- [ ] Oui — la validation est appliquée mais un contournement (Bypass) **est possible** via un parcours de répertoire (Path Traversal) ou une manipulation de sous-domaine  
- [ ] Oui — la validation est appliquée mais un contournement **est possible** via la pollution de paramètres (Parameter Pollution) ou l'injection de fragments  
- [ ] Non — l'application **autorise** des URL arbitraires dans le paramètre `redirect_uri` *(Critique)*  

### Le paramètre `response_type` peut-il être manipulé pour modifier le flux ?
- [ ] Non — l'application n'accepte que le flux prévu (par exemple, `code`) et rejette les autres  
- [ ] Oui — passer de `code` à `token` (Implicit flow) **est possible** et expose les jetons dans l'URL  
- [ ] Oui — les flux hybrides ou des valeurs de `response_type` non autorisées sont **activés** et traités  

### Les jetons d'accès ou codes d'autorisation sont-ils fuis via les en-têtes Referer ou l'historique du navigateur ?
- [ ] Non — les jetons/codes sont gérés de manière sécurisée et les pages sensibles utilisent une politique de référent (`Referrer-Policy`) appropriée  
- [ ] Oui — les jetons/codes **sont** fuis vers des domaines tiers via l'en-tête `Referer`  
- [ ] Oui — les jetons/codes **sont** visibles dans l'historique du navigateur en raison d'une mise en cache non sécurisée ou de requêtes GET  

### L'application applique-t-elle le principe du « Moindre Privilège » pour les étendues (Scopes) OAuth ?
- [ ] Oui — l'application ne demande que les privilèges (Scopes) minimaux nécessaires à la fonctionnalité  
- [ ] Non — l'application demande par défaut des privilèges excessifs ou administratifs  
- [ ] Non — l'utilisateur **ne peut pas** examiner ou refuser des privilèges spécifiques lors de la phase de consentement

---

## WSTG-SESS-01 — Test du schéma de gestion de session

Le test du schéma de gestion de session se concentre sur l'analyse de la manière dont l'application gère le cycle de vie et la structure des identifiants de session afin de s'assurer qu'ils sont robustes contre la prédiction et le détournement (Session Hijacking). Les attaquants capturent plusieurs jetons (tokens) de session pour effectuer une analyse statistique, à la recherche de modèles, d'une faible entropie ou d'incréments prévisibles permettant de forger des sessions valides pour d'autres utilisateurs. Les faiblesses du schéma se manifestent souvent par des vulnérabilités de fixation de session (Session Fixation) ou par l'utilisation de valeurs insuffisamment aléatoires, généralement trouvées dans les cookies HTTP, les paramètres d'URL ou les implémentations d'en-têtes personnalisés. Compromettre le schéma de session est une attaque à fort impact qui accorde à un adversaire un accès complet à l'état authentifié d'une victime sans avoir besoin de ses identifiants principaux.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **État du test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy pour l'analyse d'entropie)`, `Cookie Editor`

### L'identifiant de session est-il suffisamment complexe et aléatoire ?
- [ ] Oui — le jeton passe les tests statistiques de caractère aléatoire (haute entropie) et aucun contournement n'est possible  
- [ ] Oui — le jeton est long mais contient des modèles prévisibles ou des segments statiques  
- [ ] Non — le jeton est séquentiel, basé sur des horodatages (timestamps) prévisibles, ou présente une entropie **critiquement faible** *(Critique)*  

### Où l'identifiant de session est-il transmis et stocké ?
- [ ] L'identifiant est stocké dans un cookie `Secure` et `HttpOnly` *(Le plus sécurisé)*  
- [ ] L'identifiant est stocké dans un cookie mais ne possède pas les drapeaux `Secure` ou `HttpOnly`  
- [ ] L'identifiant est transmis via des paramètres d'URL ou des requêtes `GET` **uniquement** *(Risque élevé)*  

### L'application génère-t-elle un nouvel identifiant de session après une authentification réussie ?
- [ ] Oui — un nouvel ID de session unique **est généré** immédiatement après la connexion  
- [ ] Non — l'ID de session reste le même avant et après la connexion *(Session Fixation possible)*  

### L'identifiant de session est-il lié à une adresse IP spécifique ou à un User-Agent pour empêcher le réemploi ?
- [ ] Oui — la session est invalidée si l'empreinte (fingerprint) du client change de manière significative  
- [ ] Non — la session **peut** être réutilisée sur différentes adresses IP ou appareils sans vérification supplémentaire  

### Les identifiants de session sont-ils correctement invalidés côté serveur après une déconnexion ou un délai d'expiration ?
- [ ] Oui — l'état de la session côté serveur est **complètement détruit** lors de la déconnexion  
- [ ] Non — la session reste valide sur le serveur même si le cookie côté client est supprimé  
- [ ] Non — la session **n'expire jamais** ou possède un délai d'expiration (timeout) excessivement long

---

## WSTG-SESS-02 — Test des attributs de cookies

La gestion de session repose souvent sur les cookies HTTP pour maintenir l'état, ce qui fait des attributs de sécurité de ces cookies une cible privilégiée pour les attaquants. En omettant ou en configurant mal des attributs tels que `HttpOnly`, `Secure` et `SameSite`, les applications exposent les jetons de session au vol via Cross-Site Scripting (XSS), à l'interception sur des canaux non chiffrés ou à des attaques de type Cross-Site Request Forgery (CSRF). Les testeurs d'intrusion examinent ces drapeaux (flags) pour déterminer si un attaquant peut exfiltrer des identifiants de session à partir du Document Object Model (DOM) du navigateur ou forcer le navigateur à les envoyer dans des requêtes cross-origin. Une configuration appropriée des attributs est une mesure de défense en profondeur fondamentale pour garantir que les jetons sensibles restent confinés à des contextes sécurisés et autorisés.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analyse de l'implémentation des drapeaux de cookies
| Attribut | Présent | Manquant |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Le drapeau `HttpOnly` est-il appliqué aux cookies de session sensibles ?
- [ ] Oui — appliqué à **tous** les cookies de session sensibles *(Le plus sécurisé)*  
- [ ] Oui — appliqué uniquement à certains cookies de session  
- [ ] Non — le drapeau n'est **pas appliqué**, permettant l'accès via des scripts côté client *(Critique)*  

### Le drapeau `Secure` est-il appliqué pour les cookies transmis via HTTPS ?
- [ ] Oui — appliqué à **tous** les cookies transmis via TLS  
- [ ] Non — le drapeau n'est **pas appliqué**, permettant aux cookies d'être envoyés via HTTP en clair  

### L'attribut `SameSite` est-il utilisé pour atténuer les risques de CSRF ?
- [ ] Oui — défini sur `Strict` ou `Lax` pour les cookies de session  
- [ ] Oui — défini sur `None` mais avec le drapeau `Secure` présent  
- [ ] Non — l'attribut est **manquant** ou défini sur `None` **sans** le drapeau `Secure`  

### Les attributs `Domain` et `Path` sont-ils restreints au périmètre minimal nécessaire ?
- [ ] Oui — restreints au sous-domaine et au chemin d'application **spécifiques**  
- [ ] Non — la portée est trop **large** (ex: défini sur un domaine parent ou à la racine `/`), augmentant la surface d'attaque  

### Les cookies de session sensibles expirent-ils correctement via les attributs `Expires` ou `Max-Age` ?
- [ ] Oui — des dates d'expiration appropriées sont définies  
- [ ] Non — les cookies sont persistants avec des durées de vie excessivement **longues**  
- [ ] Non — les cookies sont limités à la session mais **manquent** d'une application de délai d'expiration (timeout) côté serveur

---

## WSTG-SESS-03 — Session Fixation

La fixation de session (Session Fixation) se produit lorsqu'une application ne parvient pas à invalider ou à renouveler l'identifiant de session après qu'un utilisateur s'est authentifié avec succès, permettant ainsi à un attaquant d'imposer un jeton de session connu à une victime. Si l'application conserve l'ID de session pré-authentification après la connexion, un attaquant peut fournir à une victime un lien spécifiquement conçu contenant un ID de session fixe et, par la suite, détourner la session authentifiée. Cette vulnérabilité se manifeste généralement dans les formulaires de connexion ou via des identifiants de session acceptés par des paramètres d'URL et des cookies. Du point de vue de l'attaquant, l'exploitation consiste à « fixer » la session via un lien malveillant ou une vulnérabilité d'injection d'en-tête (Header Injection) et à attendre que la victime fournisse ses identifiants, contournant ainsi la nécessité de voler un jeton (token) actif.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **État du test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### L'identifiant de session change-t-il après une authentification réussie ?
- [ ] Oui — un nouvel identifiant de session **est émis** et l'ancien est invalidé *(Plus sécurisé)*  
- [ ] Oui — un nouvel identifiant de session **est émis** mais l'ancien **reste valide**  
- [ ] Non — l'identifiant de session **reste le même** avant et après l'authentification *(Critique)*  

### L'application accepte-t-elle les identifiants de session fournis via des paramètres d'URL ?
- [ ] Non — les IDs de session sont gérés **exclusivement** via des cookies  
- [ ] Oui — les IDs de session sont acceptés dans l'URL mais sont **ignorés** ou **renouvelés** lors de la connexion  
- [ ] Oui — les IDs de session dans l'URL sont **acceptés** et **conservés** après l'authentification  

### Un ID de session défini par l'attaquant peut-il être imposé à l'application ?
- [ ] Non — l'application **rejette** les IDs de session invalides ou inexistants fournis par l'utilisateur  
- [ ] Oui — l'application **accepte** et initialise une session en utilisant n'importe quel ID arbitraire fourni dans un cookie  
- [ ] Oui — l'application **accepte** et initialise une session en utilisant n'importe quel ID arbitraire fourni dans un paramètre d'URL  

### Les sessions pré-authentification sont-elles correctement terminées ?
- [ ] Oui — la session anonyme est **entièrement détruite** lors de l'événement de connexion  
- [ ] Non — la session anonyme n'est **pas détruite**, permettant une récolte potentielle de sessions  

### Le cookie de session est-il protégé contre l'injection côté client ?
- [ ] Oui — les flags `HttpOnly` et `Secure` sont **appliqués** pour empêcher la fixation basée sur des scripts  
- [ ] Non — les flags ne sont **pas appliqués**, permettant de définir les IDs de session via Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Test de l'exposition des variables de session

L'exposition des variables de session survient lorsque des identifiants de session sensibles ou des données relatives à l'état sont transmis via des canaux non sécurisés tels que les chaînes de requête (query strings) d'URL, les en-têtes Referer ou les journaux système. Cette exposition augmente considérablement le risque de détournement de session (session hijacking), car les identifiants peuvent être mis en cache dans l'historique du navigateur, enregistrés par des proxys intermédiaires ou divulgués à des sites tiers via l'en-tête Referer. Les Pentesters identifient ces expositions en surveillant toutes les requêtes sortantes et en inspectant les journaux de l'application ou les configurations du serveur pour détecter d'éventuelles fuites de données par inadvertance. Dans le monde réel, les attaquants exploitent ces fuites en collectant des jetons de session (session tokens) valides à partir de machines partagées ou en analysant les modèles de trafic pour contourner l'authentification et obtenir un accès non autorisé aux comptes d'utilisateurs.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si la variable exposée est un jeton de session permettant une prise de contrôle immédiate du compte (account takeover).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### L'identifiant de session est-il transmis dans la chaîne de requête (query string) de l'URL ?
- [ ] Non — les identifiants de session **ne sont pas** présents dans la chaîne de requête de l'URL  
- [ ] Oui — les identifiants sont présents mais la session est de courte durée ou présente un risque faible  
- [ ] Oui — des IDs de session uniques **sont** présents dans la chaîne de requête de l'URL *(Critique)*  

### L'application divulgue-t-elle des jetons de session via l'en-tête Referer ?
- [ ] Non — la politique `Referrer-Policy` empêche la fuite ou aucun lien vers des tiers n'existe  
- [ ] Oui — les jetons de session (session tokens) **sont** transmis à des domaines tiers via l'en-tête Referer  

### Les variables de session sont-elles stockées dans les journaux (logs) du serveur ou de l'application ?
- [ ] Non — les variables de session sont masquées ou exclues des journaux  
- [ ] Oui — les variables de session **sont** enregistrées en clair dans les journaux de l'application ou du serveur web  

### L'application utilise-t-elle des requêtes GET pour des opérations modifiant l'état ?
- [ ] Non — l'application utilise `POST` ou d'autres méthodes non-idempotentes pour les opérations sensibles  
- [ ] Oui — `GET` est utilisé, provoquant la mise en cache des données de session dans l'historique du navigateur et les proxys intermédiaires  

### L'en-tête `Cache-Control` est-il configuré pour empêcher la mise en cache des données relatives à la session ?
- [ ] Oui — `Cache-Control: no-store` (ou similaire) **est appliqué** aux réponses sensibles  
- [ ] Oui — des contrôles sont en place mais un contournement (bypass) **est possible** via des cas particuliers du navigateur  
- [ ] Non — les réponses sensibles contenant des données de session **peuvent** être mises en cache par le navigateur

---

## WSTG-SESS-05 — Test de Cross-Site Request Forgery (CSRF)

Le Cross-Site Request Forgery (CSRF) est une vulnérabilité par laquelle un attaquant trompe le navigateur d'une victime pour lui faire exécuter une action non souhaitée sur un site web différent sur lequel la victime est actuellement authentifiée. Cette exploitation tire parti du comportement du navigateur qui joint automatiquement des identifiants « ambiants », tels que des cookies de session ou des en-têtes Authorization, aux requêtes sortantes. Les attaquants ciblent généralement des opérations modifiant l'état (state-changing), comme le changement de mots de passe, la mise à jour d'adresses e-mail ou l'exécution de transferts financiers, en hébergeant des scripts malveillants ou des formulaires cachés sur un site tiers. Une exploitation réussie peut mener à une prise de contrôle totale du compte (Account Takeover) ou à une modification non autorisée des données à l'insu de l'utilisateur ou sans son consentement.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si l'action vulnérable permet la prise de contrôle d'un compte, une élévation de privilèges ou des transactions financières non autorisées.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Outils :** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Des jetons anti-CSRF sont-ils implémentés pour les requêtes modifiant l'état ?
- [ ] Oui — des jetons (tokens) uniques et cryptographiquement forts sont requis pour toutes les actions modifiant l'état  
- [ ] Oui — des jetons sont présents mais ne sont **pas** uniques par session ou sont prévisibles  
- [ ] Non — les jetons anti-CSRF ne sont **pas** implémentés  

### La validation côté serveur du jeton anti-CSRF est-elle robuste ?
- [ ] Oui — le serveur valide strictement le jeton et aucun contournement (bypass) n'est **possible**  
- [ ] Oui — la validation est effectuée mais **peut** être contournée en supprimant le paramètre du jeton  
- [ ] Oui — la validation est effectuée mais **peut** être contournée en fournissant un jeton vide ou factice  
- [ ] Oui — la validation est effectuée mais le jeton n'est **pas** lié à la session de l'utilisateur  

### L'application s'appuie-t-elle sur des méthodes de protection CSRF facilement contournables ?
- [ ] Non — l'application ne s'appuie pas uniquement sur des en-têtes faibles ou des vérifications d'origine  
- [ ] Oui — la protection repose uniquement sur l'en-tête `Referer` ou `Origin` qui **peut** être usurpé ou supprimé  
- [ ] Oui — la protection repose sur la vérification de l'en-tête `X-Requested-With` qui **peut** être contournée via des mauvaises configurations CORS  

### Les cookies de session sont-ils configurés avec l'attribut `SameSite` ?
- [ ] Oui — `SameSite` est défini sur `Strict` ou `Lax` pour tous les cookies liés à la session  
- [ ] Non — l'attribut `SameSite` est **manquant**, ce qui laisse le comportement par défaut au navigateur  
- [ ] Non — `SameSite` est explicitement défini sur `None` sans l'attribut `Secure` ou mesures d'atténuation supplémentaires  

### La protection CSRF peut-elle être contournée en changeant la méthode de requête HTTP ?
- [ ] Non — la protection est appliquée quelle que soit la méthode HTTP utilisée  
- [ ] Oui — les jetons ne sont validés que sur les requêtes `POST`, mais l'action **peut** être effectuée via `GET`  
- [ ] Oui — le changement de méthode (par ex. vers `PUT` ou `DELETE`) contourne la vérification du jeton

---

## WSTG-SESS-06 — Test de la fonctionnalité de déconnexion

La fonctionnalité de déconnexion est un contrôle de sécurité critique conçu pour mettre fin à la session d'un utilisateur et invalider les identifiants de session associés, tant du côté client que du côté serveur. Un échec dans l'implémentation correcte de la déconnexion permet des attaques de type Session Fixation ou Hijacking, car un attaquant accédant à une machine après qu'un utilisateur se soit "déconnecté" pourrait potentiellement réutiliser le jeton de session (Session Token) toujours actif. Les Pentesters évaluent cela en vérifiant si le cookie de session est effacé du navigateur, si l'état de la session côté serveur est explicitement détruit, et si la navigation via le bouton "retour" permet d'accéder à des données sensibles mises en cache. Une implémentation sécurisée de la déconnexion garantit qu'une fois l'action déclenchée, aucune requête ultérieure utilisant l'ancien jeton de session n'est autorisée par l'application.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **État du Test** | Non réalisé |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Un mécanisme de déconnexion fonctionnel est-il présent et accessible ?
- [ ] Oui — le bouton de déconnexion existe et déclenche une requête de terminaison  
- [ ] Non — le bouton de déconnexion est **manquant** ou ne **déclenche pas** d'événement de terminaison  

### L'identifiant de session est-il invalidé côté serveur ?
- [ ] Oui — le serveur rejette toutes les requêtes ultérieures utilisant l'ancien jeton de session  
- [ ] Non — le serveur **continue d'accepter** l'ancien jeton de session après la déconnexion  

### L'application efface-t-elle les cookies de session dans le navigateur lors de la déconnexion ?
- [ ] Oui — les cookies sont écrasés avec une date d'expiration passée ou une valeur vide  
- [ ] Oui — les cookies subsistent mais ne sont **plus valides** sur le serveur  
- [ ] Non — les cookies restent dans le navigateur et **conservent** leurs valeurs d'origine  

### Des contenus authentifiés sensibles peuvent-ils être consultés via le bouton 'Retour' du navigateur après la déconnexion ?
- [ ] Non — l'en-tête `Cache-Control: no-store` ou des en-têtes similaires empêchent la consultation de pages sensibles après la déconnexion  
- [ ] Oui — les pages sensibles sont mises en cache et **peuvent** être consultées par navigation après la terminaison de la session

---

## WSTG-SESS-07 — Test du délai d'expiration de session

Le test du délai d'expiration de session (Session Timeout) évalue si une application met fin efficacement à la session d'un utilisateur après une période d'inactivité prédéfinie ou une durée totale. Ce contrôle est essentiel pour atténuer les risques de Session Hijacking, en particulier sur les postes de travail partagés ou dans les scénarios où un attaquant capture un identifiant de session via une interception réseau ou une XSS. Les Pentesters analysent à la fois les délais d'expiration d'inactivité (Idle Timeouts), qui se déclenchent après une période d'inactivité, et les délais d'expiration absolus (Absolute Timeouts), qui limitent la durée de vie totale d'une session indépendamment de l'activité. Du point de vue d'un attaquant, des sessions indéfinies ou excessivement longues offrent une fenêtre d'opportunité étendue pour maintenir un accès non autorisé et contourner la nécessité d'une ré-authentification.

| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### L'application implémente-t-elle un délai d'expiration de session d'inactivité ?
- [ ] Oui — la session expire après une courte période (ex : 15-30 minutes) d'inactivité  
- [ ] Oui — la session expire mais la durée du délai est **excessivement longue** (ex : > 24 heures)  
- [ ] Non — la session **reste active** indéfiniment pendant l'inactivité  

### Le délai d'expiration de la session est-il appliqué côté serveur ?
- [ ] Oui — le serveur rejette les requêtes avec des jetons expirés quel que soit l'état côté client  
- [ ] Non — le délai d'expiration est **uniquement** appliqué via JavaScript côté client ou meta-refresh  

### L'application implémente-t-elle un délai d'expiration de session absolu ?
- [ ] Oui — la session est terminée après une durée maximale fixe indépendamment de l'activité  
- [ ] Non — la session **peut** être prolongée indéfiniment tant qu'il y a une activité continue  

### L'identifiant de session est-il invalidé sur le serveur lors de l'expiration ?
- [ ] Oui — le jeton de session **ne peut pas** être réutilisé une fois le seuil d'expiration atteint  
- [ ] Non — le jeton de session **est toujours valide** sur le serveur même après que le client a été redirigé vers la page de connexion  

### La session peut-elle être maintenue active indéfiniment à l'aide de requêtes heartbeat automatisées ?
- [ ] Non — un délai d'expiration absolu ou des contrôles secondaires **empêchent** une extension de session infinie  
- [ ] Oui — des requêtes périodiques en arrière-plan (ex : AJAX) **peuvent** maintenir la session indéfiniment

---

## WSTG-SESS-08 — Session Puzzling

Le Session Puzzling, également connu sous le nom de Surcharge de Variables de Session (Session Variable Overloading), se produit lorsqu'une application utilise la même variable de session pour plusieurs objectifs à travers différents modules ou ne parvient pas à réinitialiser correctement les états de session lors des transitions de flux de travail. Les attaquants exploitent ce comportement en alimentant des variables de session dans un contexte donné — tel qu'un aperçu de profil non authentifié ou une inscription en plusieurs étapes — puis en naviguant vers une zone protégée où l'application fait incorrectement confiance à ces variables préexistantes. Cela peut entraîner des impacts critiques, notamment le contournement de l'authentification (Authentication Bypass), l'escalade de privilèges (Privilege Escalation) ou l'accès non autorisé à des données sensibles en trompant l'application pour lui faire croire qu'une session se trouve dans un état de privilège plus élevé qu'elle ne l'est réellement. Cette vulnérabilité est particulièrement présente dans les applications complexes avec état (stateful) où la logique de gestion de session est appliquée de manière incohérente entre les différents composants fonctionnels.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### L'application utilise-t-elle les mêmes noms de variables de session à des fins différentes à travers des modules distincts ?
- [ ] Non — les noms de variables sont uniques ou les portées (scopes) sont isolées  
- [ ] Oui — des noms de variables partagés existent mais l'état est effacé entre les modules  
- [ ] Oui — des noms de variables partagés existent et l'état **n'est pas** effacé  

### Des actions non authentifiées peuvent-elles influencer des variables de session utilisées dans des contextes authentifiés ?
- [ ] Non — les variables de session sont initialisées uniquement **après** une authentification réussie  
- [ ] Oui — les variables de session peuvent être définies avant la connexion mais n'affectent **pas** l'état post-connexion  
- [ ] Oui — les variables de session définies pendant la pré-authentification **sont** approuvées après l'authentification *(Critique)*  

### L'application réinitialise-t-elle l'identifiant de session et ses variables associées lors d'un changement de niveau de privilège ?
- [ ] Oui — la session est entièrement réinitialisée et un nouvel ID **est émis**  
- [ ] Partiel — un nouvel ID est émis mais certaines variables de session **persistent**  
- [ ] Non — l'ID de session et toutes les variables **restent** inchangés  

### Des privilèges administratifs peuvent-ils être obtenus en manipulant des variables de session via un compte non privilégié ?
- [ ] Non — les variables de privilège **ne peuvent pas** être modifiées par les utilisateurs  
- [ ] Oui — les variables peuvent être modifiées mais l'application les **valide** par rapport à la base de données backend  
- [ ] Oui — les variables peuvent être modifiées et l'application **fait confiance** à l'état de la session de manière implicite *(Critique)*  

### L'état de la session est-il effacé immédiatement après l'achèvement d'un flux de travail spécifique (ex : réinitialisation de mot de passe, paiement) ?
- [ ] Oui — les variables de session du flux de travail sont détruites une fois celui-ci terminé  
- [ ] Non — les variables du flux de travail **persistent** et peuvent être réutilisées dans d'autres zones de l'application

---

## WSTG-SESS-09 — Testing for Session Hijacking

Le Session Hijacking (détournement de session) se produit lorsqu'un attaquant capture, prédit ou fixe un identifiant de session (SID) valide pour obtenir un accès non autorisé à la session active d'un utilisateur. Cette vulnérabilité se manifeste généralement par une sécurité insuffisante de la couche de transport, une absence de flags de sécurité sur les cookies ou des algorithmes de génération de SID prévisibles qui permettent à un attaquant de contourner entièrement l'authentification. Du point de vue de l'attaquant, une exploitation réussie permet une usurpation d'identité (impersonation) complète de la victime sans nécessiter ses identifiants (credentials), ce qui est souvent réalisé par sniffing réseau, Cross-Site Scripting (XSS) ou des attaques de Session Fixation.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Les identifiants de session sont-ils protégés pendant le transit ?
- [ ] Oui — Le flag `Secure` est **activé** et HSTS est strictement appliqué  
- [ ] Oui — Le flag `Secure` est **activé** mais HSTS est **désactivé**  
- [ ] Non — Le flag `Secure` n'est **pas appliqué**, permettant l'interception du SID sur des canaux non chiffrés *(Élevée)*  

### L'identifiant de session est-il protégé contre l'accès par script côté client ?
- [ ] Oui — Le flag `HttpOnly` est **appliqué** à tous les cookies liés à la session  
- [ ] Non — Le flag `HttpOnly` n'est **pas appliqué**, permettant l'exfiltration du SID via XSS *(Critique)*  

### L'application implémente-t-elle une liaison de session (session binding) aux attributs du client ?
- [ ] Oui — La session est liée aux attributs du client (IP/User-Agent) et la réutilisation n'est **pas possible**  
- [ ] Oui — La liaison de session est présente mais un contournement **est possible** via le spoofing d'en-tête (header spoofing)  
- [ ] No — Aucune liaison de session n'existe ; le SID **est valide** lorsqu'il est utilisé depuis n'importe quelle source  

### L'identifiant de session est-il suffisamment aléatoire pour empêcher la prédiction ?
- [ ] Oui — Le SID utilise un générateur de nombres pseudo-aléatoires cryptographiquement sûr (CSPRNG)  
- [ ] Non — La longueur du SID est insuffisante ou suit une séquence/un motif **prévisible**  

### Les sessions concurrentes sont-elles gérées pour limiter la fenêtre d'attaque ?
- [ ] Non — L'application impose strictement une seule session active par utilisateur  
- [ ] Oui — Plusieurs sessions concurrentes sont **autorisées** mais surveillées  
- [ ] Oui — Des sessions concurrentes illimitées **sont possibles** à travers différents appareils/emplacements

---

## WSTG-SESS-10 — Testing JSON Web Tokens

Les JSON Web Tokens (JWT) sont couramment utilisés pour la gestion de session sans état (stateless) et la propagation d'identité, mais leur sécurité repose entièrement sur l'implémentation correcte des algorithmes de signature et sur une vérification stricte des claims. Les attaquants tentent fréquemment de contourner l'authentification en exploitant les failles de l'algorithme "none", en effectuant un brute-force sur des clés secrètes HS256 faibles, ou en réalisant des attaques par changement d'algorithme (algorithm-switching) où une clé publique asymétrique est utilisée comme un secret HMAC symétrique. Ces vulnérabilités se manifestent généralement dans les cookies de session ou les en-têtes Authorization, permettant potentiellement à un attaquant d'élever ses privilèges ou d'usurper l'identité de n'importe quel utilisateur en modifiant des claims tels que `role` ou `user_id` sans invalider l'intégrité cryptographique du token.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Outils :** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### L'algorithme "none" est-il accepté par le serveur ?
- [ ] Non — le serveur **rejette** les tokens utilisant `alg: none` dans l'en-tête  
- [ ] Oui — le serveur **accepte** les tokens avec `alg: none` et une signature vide *(Critique)*  

### Comment la signature JWT est-elle vérifiée et protégée contre le brute-force ?
- [ ] Oui — des clés asymétriques fortes (RS256/ES256) ou des secrets HMAC à haute entropie sont utilisés  
- [ ] Oui — HS256 est utilisé mais le secret **peut** être cassé par brute-force en raison d'une faible entropie ou de valeurs par défaut  
- [ ] No — la vérification de la signature **n'est pas appliquée** ou **peut** être contournée via un changement d'algorithme (de RS256 à HS256)  

### Les claims standard (exp, nbf, iat) sont-ils strictement appliqués par le backend ?
- [ ] Oui — le claim `exp` (expiration) est présent et **ne peut pas** être contourné  
- [ ] Oui — `exp` est présent mais le serveur **ne l'applique pas** lors de la validation  
- [ ] Non — les claims d'expiration sont **manquants** ou ignorés, permettant un rejeu de session indéfini  

### L'implémentation empêche-t-elle l'injection de clés via les en-têtes (kid, jku, x5u) ?
- [ ] Oui — les en-têtes sont assainis et seules les clés/sources internes de confiance sont **autorisées**  
- [ ] Non — l'en-tête `kid` (Key ID) est vulnérable à une traversée de répertoire (directory traversal) ou à une injection SQL pour pointer vers un fichier connu  
- [ ] Non — les en-têtes `jku` ou `x5u` **peuvent** pointer vers des URL contrôlées par l'attaquant pour charger des clés malveillantes

---

## WSTG-SESS-11 — Test de sessions simultanées (Concurrent Sessions)

Le test de sessions simultanées (Concurrent Sessions) évalue si une application permet à un seul compte utilisateur de maintenir plusieurs sessions actives simultanément à travers différents navigateurs, appareils ou adresses IP. L'absence de restriction sur les sessions simultanées augmente la fenêtre d'opportunité pour les attaquants d'utiliser des jetons de session (Session Tokens) détournés ou des identifiants compromis sans alerter l'utilisateur légitime ou mettre fin aux sessions existantes. Ce comportement est généralement évalué en s'authentifiant depuis plusieurs sources et en surveillant la stabilité de la session, ce qui révèle souvent des faiblesses dans la logique de gestion de session (Session Management) ou une absence de règles métier axées sur la sécurité. Du point de vue d'un attaquant, l'absence de contrôle de simultanéité permet une persistance furtive après un compromis initial et facilite les attaques automatisées en parallèle.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **État du test** | Non réalisé |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Les sessions simultanées sont-elles limitées par la politique de l'application ?
- [ ] Oui — une seule session **est autorisée** par utilisateur ; les nouvelles connexions invalident les anciennes *(Le plus sécurisé)*  
- [ ] Oui — un nombre fixe de sessions simultanées **est imposé** et ne peut être dépassé  
- [ ] Non — un nombre illimité de sessions simultanées **est possible**  

### Comment l'application réagit-elle lorsque la limite de sessions est atteinte ?
- [ ] La session active la plus ancienne **est automatiquement terminée** (atténuation de la fixation de session/détournement de session)  
- [ ] L'utilisateur **est invité** à mettre fin aux sessions existantes avant que la nouvelle session ne soit autorisée  
- [ ] La nouvelle tentative de connexion **est bloquée** jusqu'à ce qu'une déconnexion manuelle soit effectuée depuis un autre appareil  
- [ ] Aucune action n'est entreprise — plusieurs sessions restent **activées** simultanément  

### L'application fournit-elle une interface de gestion de session pour l'utilisateur ?
- [ ] Oui — les utilisateurs **peuvent** consulter les sessions actives (IP, appareil, heure) et les terminer à distance  
- [ ] Oui — les utilisateurs **peuvent** consulter les sessions actives mais **ne peuvent pas** les terminer à distance  
- [ ] Non — les utilisateurs **ne peuvent pas** voir ni gérer d'autres sessions actives associées à leur compte  

### L'utilisateur est-il averti lorsqu'une activité de connexion simultanée est détectée ?
- [ ] Oui — une alerte (e-mail, SMS ou in-app) **est déclenchée** pour les connexions provenant de nouveaux appareils ou emplacements  
- [ ] Non — aucune notification **n'est fournie** lorsque des sessions simultanées sont établies

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Le Reflected Cross Site Scripting (XSS) se produit lorsqu'une application inclut des données non fiables dans une réponse HTTP sans validation ou encodage suffisant, provoquant l'exécution du Payload dans le contexte du navigateur de la victime. Les attaquants délivrent des payloads malveillants via l'ingénierie sociale, généralement par le biais d'URLs ou de formulaires forgés, pour détourner des sessions utilisateur, exfiltrer des cookies sensibles ou effectuer des actions non autorisées au nom de l'utilisateur. Cette vulnérabilité se trouve couramment dans les paramètres de recherche, les messages d'erreur et tout Endpoint qui reflète une entrée directement dans l'interface utilisateur. L'exploitation repose sur l'interaction de la victime avec un lien malveillant, ce qui en fait un vecteur d'attaque non persistant mais très efficace pour cibler des utilisateurs administratifs ou authentifiés spécifiques.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Outils :** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Les entrées fournies par l'utilisateur sont-elles réfléchies dans le corps de la réponse ?
- [ ] Non — l'entrée n'est **jamais** réfléchie vers l'utilisateur  
- [ ] Oui — l'entrée est réfléchie mais correctement encodée et un Bypass n'est **pas possible**  
- [ ] Oui — l'entrée est réfléchie **sans** aucun encodage ni assainissement (sanitization)  

### L'encodage de sortie contextuel est-il implémenté ?
- [ ] Oui — un encodage approprié (HTML, Attribut, JavaScript) est **appliqué** en fonction du contexte de réflexion spécifique  
- [ ] Oui — l'encodage est **appliqué** mais est insuffisant pour le contexte spécifique (par exemple, encodage HTML à l'intérieur d'une balise `<script>`)  
- [ ] Non — l'encodage n'est **pas appliqué**  

### Les filtres de validation d'entrée ou du Web Application Firewall (WAF) peuvent-ils être contournés ?
- [ ] Non — les filtres bloquent efficacement tous les payloads XSS communs et obfusqués  
- [ ] Oui — des filtres sont présents mais un Bypass est **possible** en utilisant l'encodage de caractères, des balises non standard ou des polyglottes  
- [ ] Non — aucun filtre ou mécanisme de validation n'est en place  

### Quel est le contexte d'exécution de l'entrée réfléchie ?
- [ ] Sûr — l'entrée est réfléchie dans un emplacement non exécutable (par exemple, à l'intérieur de balises `<div>` ou `<span>` standards)  
- [ ] Risqué — l'entrée est réfléchie à l'intérieur d'attributs HTML ou dans le `src`/`href` de balises  
- [ ] Critique — l'entrée est réfléchie directement à l'intérieur de blocs `<script>`, de gestionnaires d'événements (event handlers) ou de littéraux de gabarits (template literals)  

### Des en-têtes de sécurité de navigateur modernes sont-ils présents pour atténuer l'impact du XSS ?
- [ ] Oui — la `Content-Security-Policy` (CSP) est **activée** avec une directive script-src restrictive  
- [ ] Oui — la `Content-Security-Policy` (CSP) est **activée** mais contient `unsafe-inline` ou des listes blanches faibles  
- [ ] Non — aucun en-tête CSP ou l'ancien `X-XSS-Protection` n'est présent

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Le Stored Cross Site Scripting (XSS), également connu sous le nom de XSS persistant, se produit lorsqu'une application reçoit des données d'un utilisateur et les stocke dans une base de données persistante, un système de fichiers ou tout autre support de stockage sans validation ou encodage adéquat. Lorsqu'une victime navigue ultérieurement sur une page qui récupère et affiche ces données non assainies, le script malveillant s'exécute dans le contexte de son navigateur sous l'origine de l'application. Cette vulnérabilité est particulièrement dangereuse car elle ne nécessite pas que la victime clique sur un lien spécifiquement conçu ; tout utilisateur consultant la page affectée devient une cible, ce qui peut mener à un détournement de session (Session Hijacking), à des actions non autorisées ou à la collecte d'identifiants (Credential Harvesting). Les attaquants ciblent généralement les sections de commentaires, les profils d'utilisateurs, les forums de discussion et les journaux d'administration (Logs) où les entrées sont systématiquement rendues pour d'autres utilisateurs ou administrateurs.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **État du test** | Non réalisé |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Outils :** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Existe-t-il des points d'entrée qui stockent les entrées utilisateur pour un affichage ultérieur à d'autres utilisateurs ?
- [ ] Non — l'application ne stocke pas d'entrées contrôlables par l'utilisateur pour un rendu ultérieur  
- [ ] Oui — l'entrée est stockée (ex: profils, commentaires) mais n'est **pas** rendue dans un contexte HTML  
- [ ] Oui — l'entrée est stockée et rendue à d'autres utilisateurs ou administrateurs  

### Un encodage de sortie ou un assainissement (Sanitization) est-il appliqué lors du rendu des données stockées ?
- [ ] Oui — un encodage de sortie sensible au contexte **est appliqué** et un contournement (Bypass) n'est **pas possible** *(Le plus sécurisé)*  
- [ ] Oui — un assainissement (ex: `DOMPurify`) **est appliqué** mais un contournement **est possible** via des erreurs de configuration  
- [ ] Non — les données sont rendues directement dans le DOM **sans** encodage ni assainissement *(Sévérité Haute)*  

### La validation des entrées ou l'assainissement peuvent-ils être contournés à l'aide de Payloads alternatifs ?
- [ ] Non — les filtres gèrent de manière sécurisée les différents encodages, les balises non standard et les gestionnaires d'événements  
- [ ] Oui — un contournement **est possible** en utilisant l'encodage d'entités HTML ou des balises spécifiques (ex: `<svg>`, `<img>`)  
- [ ] Oui — un contournement **est possible** via des Payloads polyglottes ou du XSS basé sur la mutation (mXSS)  

### Une Content Security Policy (CSP) fournit-elle une couche de défense secondaire ?
- [ ] Oui — une CSP stricte est **activée** et empêche l'exécution de scripts inline et de sources non autorisées  
- [ ] Oui — une CSP est **activée** mais est mal configurée (ex: `unsafe-inline` ou un `script-src` trop large)  
- [ ] Non — aucune CSP n'est **activée** pour l'application  

### Est-il possible de réaliser un "Stored XSS to RCE" ou d'autres chaînes à fort impact (Blind XSS) ?
- [ ] Non — l'impact est limité au contexte du navigateur côté client  
- [ ] Oui — le Stored XSS **peut** être utilisé pour cibler des administrateurs (Blind XSS) dans des panneaux internes  
- [ ] Oui — le Stored XSS **peut** être enchaîné avec d'autres vulnérabilités (ex: CSRF, exfiltration de données sensibles)

---

## WSTG-INPV-03 — Testing for HTTP Verb Tampering

Le HTTP Verb Tampering (manipulation de verbe HTTP) exploite les faiblesses dans la manière dont les serveurs web et les frameworks d'application autorisent l'accès à des ressources spécifiques en fonction de la méthode HTTP utilisée. Les attaquants tentent de contourner les contraintes de sécurité en substituant des méthodes standards comme `GET` ou `POST` par des alternatives telles que `HEAD`, `PUT`, `OPTIONS`, ou même des chaînes de caractères arbitraires et non-standards que le backend pourrait traiter de manière incohérente. Cette vulnérabilité survient généralement lorsque les configurations de sécurité — telles que les filtres web.xml de Java EE ou les règles d'autorisation .NET — énumèrent explicitement les méthodes autorisées mais ne parviennent pas à prendre en compte les autres, ou lorsqu'une approche "deny-by-method" (refus par méthode) est utilisée au lieu d'un "deny-all" (tout refuser). Une exploitation réussie peut entraîner un accès non autorisé à des fonctions d'administration, une modification de données ou une divulgation d'informations concernant la configuration interne du serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fonctions administratives ou des modifications de données (PUT/DELETE) peuvent être effectuées sans authentification.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### L'application répond-elle à des méthodes HTTP non-standards ou arbitraires ?
- [ ] Non — l'application rejette toutes les méthodes à l'exception de celles explicitement requises  
- [ ] Oui — l'application répond aux méthodes standards (OPTIONS, TRACE) mais **ne peut pas** contourner la sécurité  
- [ ] Oui — l'application accepte des chaînes arbitraires (ex : FOO, TEST) et les traite comme des requêtes `GET` ou `POST`  

### L'authentification/autorisation peut-elle être contournée en modifiant la méthode HTTP ?
- [ ] Non — les contrôles de sécurité sont appliqués globalement quel que soit le verbe HTTP  
- [ ] Oui — des contrôles sont en place mais un contournement **est possible** en utilisant la méthode `HEAD`  
- [ ] Oui — les contrôles de sécurité ne sont **pas appliqués** aux méthodes alternatives, permettant un accès non autorisé aux points de terminaison (endpoints) restreints  

### Des méthodes dangereuses telles que `PUT` ou `DELETE` sont-elles activées sur le serveur ?
- [ ] Non — les méthodes dangereuses sont **désactivées** ou renvoient un code 405 Method Not Allowed  
- [ ] Oui — les méthodes sont activées mais nécessitent une authentification valide avec des privilèges élevés  
- [ ] Oui — les méthodes sont **activées** et permettent le téléversement de fichiers non autorisé ou la suppression de ressources *(Critique)*  

### La méthode `OPTIONS` révèle-t-elle des informations sensibles sur les verbes autorisés ?
- [ ] Non — `OPTIONS` est **désactivée** ou renvoie une réponse générique  
- [ ] Oui — `OPTIONS` renvoie l'en-tête `Allow` mais n'expose pas de méthodes internes restreintes  
- [ ] Oui — `OPTIONS` révèle des méthodes internes uniquement ou administratives qui aident à une exploitation ultérieure  

### La méthode `TRACE` est-elle activée, permettant potentiellement du Cross-Site Tracing (XST) ?
- [ ] Non — les méthodes `TRACE` et `TRACK` sont **désactivées**  
- [ ] Oui — `TRACE` est **activée** et renvoie les en-têtes HTTP, y compris les cookies/jetons sensibles

---

## WSTG-INPV-04 — Test de l'HTTP Parameter Pollution

L'HTTP Parameter Pollution (HPP) se produit lorsqu'une application reçoit plusieurs paramètres HTTP portant le même nom et les traite de manière incohérente ou non sécurisée. En fournissant des paramètres en double, un attaquant peut manipuler la logique interne de l'application, contournant potentiellement les Web Application Firewalls (WAF), les filtres de validation des entrées ou les mécanismes de contrôle d'accès. Cette vulnérabilité dépend fortement du serveur web sous-jacent ou du framework applicatif, qui peut choisir la première, la dernière ou une version concaténée des paramètres répétés. L'exploitation cible généralement les points de terminaison qui transmettent des paramètres à des API back-end, des requêtes de base de données ou des mécanismes de redirection d'URL, entraînant des impacts allant du Cross-Site Scripting (XSS) à l'exfiltration de données non autorisée.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Comment l'application gère-t-elle plusieurs paramètres HTTP ayant le même nom ?
- [ ] Non — les paramètres en double sont **rejetés** ou ignorés par le serveur  
- [ ] Oui — le serveur sélectionne systématiquement uniquement la **première** ou la **dernière** occurrence  
- [ ] Oui — le serveur **concatène** les valeurs, permettant potentiellement une injection  

### L'HPP peut-elle être exploitée pour contourner les contrôles de sécurité tels qu'un WAF ou un filtre d'entrée ?
- [ ] Non — les contrôles de sécurité inspectent correctement **toutes** les occurrences d'un paramètre  
- [ ] Oui — un WAF ou un filtre n'inspecte que la **première** occurrence, permettant à une **seconde** occurrence malveillante de passer  
- [ ] Oui — la concaténation permet à un attaquant de **diviser** un Payload sur plusieurs paramètres pour échapper à la détection  

### L'application reflète-t-elle les paramètres pollués dans la réponse sans assainissement approprié ?
- [ ] Non — les paramètres sont assainis ou encodés avant d'être reflétés dans l'UI  
- [ ] Oui — la pollution entraîne un contenu reflété, mais l'assainissement **est appliqué**  
- [ ] Oui — la pollution entraîne un contenu reflété et l'assainissement **n'est pas appliqué**, menant à un XSS ou à des erreurs logiques  

### Les paramètres transmis aux appels API internes ou aux systèmes back-end sont-ils vulnérables à la pollution ?
- [ ] Non — les requêtes back-end utilisent des méthodes de construction sûres et non dynamiques  
- [ ] Oui — l'HPP permet à un attaquant d'**injecter** ou de **réécrire** des paramètres dans la structure de la requête back-end  

### L'application présente-t-elle un comportement différent lorsque les paramètres sont pollués dans les requêtes GET par rapport aux requêtes POST ?
- [ ] Non — le comportement est cohérent pour toutes les méthodes HTTP  
- [ ] Oui — seuls les paramètres GET sont vulnérables à la pollution  
- [ ] Oui — seuls les paramètres POST (body) sont vulnérables à la pollution

---

## WSTG-INPV-05 — SQL Injection

L'Injection SQL (SQL Injection) se produit lorsque des entrées fournies par l'utilisateur sont incorporées dans des requêtes SQL sans paramétrage ou assainissement adéquat, permettant aux attaquants de manipuler la logique de la requête. Une exploitation réussie peut entraîner une exfiltration de données non autorisée, un contournement de l'authentification (Authentication Bypass), une modification de données et, dans certains cas, une exécution de code à distance (Remote Code Execution) via des fonctionnalités de base de données telles que `xp_cmdshell` ou `LOAD_FILE()`. Cette vulnérabilité affecte couramment les formulaires de connexion, les fonctionnalités de recherche, les paramètres d'API REST, les en-têtes HTTP et tout point de terminaison (endpoint) construisant des requêtes SQL dynamiques à partir d'entrées contrôlées par l'utilisateur. Du point de vue d'un attaquant, l'identification d'un point d'injection est la porte d'entrée principale vers une compromission complète de la base de données et un mouvement latéral potentiel au sein de l'infrastructure.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Statut du test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Outils :** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Les paramètres contrôlables par l'utilisateur sont-ils traités à l'aide de méthodes d'accès aux bases de données sécurisées ?
- [ ] Oui — l'application utilise exclusivement un ORM ou des requêtes paramétrées et un contournement n'est **pas possible**  
- [ ] Oui — des requêtes paramétrées sont utilisées mais un contournement **est possible** via des cas limites (ex : clauses `ORDER BY`)  
- [ ] Non — la concaténation de chaînes brutes est utilisée pour construire des requêtes SQL **sans** paramétrage  

### Le mécanisme d'authentification peut-il être contourné via une injection SQL ?
- [ ] Non — les paramètres de connexion ne sont **pas** vulnérables à l'injection  
- [ ] Oui — un contournement de l'authentification **est possible** à l'aide de payloads basés sur des tautologies (ex : `' OR 1=1 --`)  

### L'exfiltration de données est-elle possible via des techniques en bande (in-band) ou hors bande (out-of-band) ?
- [ ] Non — aucune exfiltration de données identifiée  
- [ ] Oui — une exfiltration basée sur UNION ou sur des erreurs (error-based) **est possible**  
- [ ] Oui — une exfiltration aveugle (Blind), qu'elle soit basée sur le booléen (Boolean-based) ou sur le temps (Time-based), **est possible**  
- [ ] Oui — une exfiltration hors bande (Out-of-band, DNS/HTTP) **est possible**  

### Les fonctions de l'application présentent-elles des vulnérabilités d'injection SQL de second ordre (Second-order SQL injection) ?
- [ ] Non — les données stockées sont manipulées de manière sécurisée lorsqu'elles sont récupérées pour des requêtes ultérieures  
- [ ] Oui — une entrée utilisateur est stockée puis utilisée ultérieurement dans une requête SQL non sécurisée **sans** validation  

### Les privilèges du compte de service de la base de données sont-ils restreints au minimum requis ?
- [ ] Oui — le compte de service dispose du **moindre privilège** (ex : limité à des tables/actions spécifiques)  
- [ ] Non — le compte de service dispose de privilèges élevés (ex : `DROP TABLE`, privilèges `FILE`, ou `xp_cmdshell` **activé**)

---

## WSTG-INPV-06 — Testing for LDAP Injection

L'Injection LDAP (LDAP Injection) survient lorsqu'une application intègre des données fournies par l'utilisateur dans des filtres LDAP (Lightweight Directory Access Protocol) sans assainissement ou échappement adéquat, permettant ainsi à un attaquant de manipuler la logique de la requête. En injectant des caractères spéciaux tels que des astérisques, des parenthèses et des opérateurs logiques, les attaquants peuvent modifier le filtre de recherche pour contourner les mécanismes d'authentification ou exfiltrer des informations sensibles de l'annuaire, notamment des noms d'utilisateur, des adhésions à des groupes et des attributs organisationnels. Cette vulnérabilité se manifeste généralement dans des fonctionnalités telles que les recherches d'annuaires d'entreprise, les portails d'employés ou les systèmes d'authentification unique (SSO) qui s'interfacent avec Active Directory ou OpenLDAP. Du point de vue de l'attaquant, une exploitation réussie implique souvent des techniques de Blind Injection pour énumérer la structure de l'annuaire ou les valeurs des attributs bit par bit lorsque la sortie directe de la requête est supprimée par l'application.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Outils :** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### L'application utilise-t-elle LDAP pour l'authentification ou les recherches d'annuaire ?
- [ ] Non — LDAP n'est **pas utilisé** dans l'architecture de l'application  
- [ ] Oui — LDAP est utilisé et toutes les entrées sont strictement assainies ou paramétrées  
- [ ] Oui — LDAP est utilisé et les entrées utilisateur sont concaténées dans les filtres **sans** échappement approprié  

### Les méta-caractères LDAP sont-ils correctement échappés ou filtrés dans les entrées utilisateur ?
- [ ] Oui — les caractères tels que `(`, `)`, `&`, `|`, `*`, et `\` ne sont **pas autorisés** ou sont correctement échappés  
- [ ] Non — les méta-caractères **peuvent** être injectés pour modifier la structure du filtre LDAP  

### Le mécanisme d'authentification peut-il être contourné via une injection ?
- [ ] Non — la logique de connexion n'est **pas susceptible** d'être manipulée par une requête  
- [ ] Oui — l'authentification **peut** être contournée en utilisant une injection de OU logique (`|`) ou de caractère générique (`*`) dans les champs de nom d'utilisateur ou de mot de passe  

### Une exfiltration aveugle (blind) de données est-elle possible depuis le service d'annuaire ?
- [ ] Non — les résultats de recherche sont limités et aucune différence de temps ou de réponse booléenne n'est observée  
- [ ] Oui — les attributs de l'annuaire **peuvent** être énumérés bit par bit via des techniques de Boolean-based Blind Injection  
- [ ] Oui — les enregistrements complets de l'annuaire **sont** reflétés dans la réponse de l'application en raison de la manipulation de la requête  

### Les composants secondaires de l'application (ex : serveurs de messagerie, SSO) sont-ils vulnérables à l'injection d'attributs LDAP ?
- [ ] Non — les attributs LDAP sont gérés de manière sécurisée dans tous les composants  
- [ ] Oui — un attaquant **peut** modifier ses propres attributs (ex : email, appartenance à un groupe) via une injection pour obtenir une escalade de privilèges ou rediriger les communications

---

## WSTG-INPV-07 — Injection XML

L'injection XML (XML Injection) se produit lorsqu'une application incorpore des données fournies par l'utilisateur dans un document ou un flux XML sans validation, assainissement (sanitization) ou échappement (escaping) appropriés des méta-caractères XML. En injectant des balises XML spécifiques ou des éléments structurels, un attaquant peut manipuler la logique du document, contourner les mécanismes d'authentification ou interférer avec l'intégrité des données traitées par le backend. Cette vulnérabilité est couramment rencontrée dans les services web basés sur SOAP, les API REST acceptant le XML, les assertions SAML et les applications qui génèrent dynamiquement des fichiers de configuration ou des rapports XML. Du point de vue de l'attaquant, l'objectif est de sortir du contexte de données prévu pour altérer la structure du document, ce qui peut potentiellement conduire à une élévation de privilèges non autorisée ou à l'exécution d'une logique métier non souhaitée.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Haute / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Outils :** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### L'application accepte-t-elle et traite-t-elle des entrées au format XML provenant de l'utilisateur ?
- [ ] Non — l'application ne traite **pas** d'entrées XML  
- [ ] Oui — les entrées XML sont traitées via des API (SOAP/REST)  
- [ ] Oui — le XML est traité via des téléversements de fichiers (SVG, DOCX, etc.)  

### Les méta-caractères XML (ex : `<`, `>`, `&`, `'`, `"`) sont-ils correctement échappés ou assainis ?
- [ ] Oui — tous les méta-caractères sont **correctement** échappés ou assainis *(Le plus sécurisé)*  
- [ ] Oui — un certain filtrage est appliqué mais un contournement (bypass) **est possible** via l'encodage  
- [ ] Non — les entrées brutes sont directement intégrées dans les structures XML **sans** assainissement  

### La validation de schéma côté serveur (XSD/DTD) est-elle imposée pour toutes les entrées XML ?
- [ ] Oui — une validation de schéma stricte est **activée** et empêche les modifications structurelles  
- [ ] Oui — la validation est **activée** mais n'empêche **pas** l'injection de balises dans les champs de données  
- [ ] Non — la validation de schéma est **désactivée** ou non implémentée  

### Un attaquant peut-il injecter de nouvelles balises XML pour altérer la structure ou la logique du document ?
- [ ] Non — la structure reste intacte quelles que soient les entrées  
- [ ] Oui — des balises peuvent être injectées mais **ne peuvent pas** altérer la logique métier  
- [ ] Oui — l'injection de balises **est possible** et permet de contourner la logique ou de modifier les données *(Critique)*  

### L'analyseur XML (XML parser) peut-il être manipulé à l'aide de sections CDATA ou de commentaires XML ?
- [ ] Non — l'analyseur traite ou rejette correctement les marqueurs CDATA/commentaires  
- [ ] Oui — l'injection de `<![CDATA[...]]>` ou `<!-- ... -->` **est possible** pour masquer ou contourner les filtres

---

## WSTG-INPV-08 — Test d'injection SSI (Server-Side Includes)

L'injection Server-Side Includes (SSI) se produit lorsqu'une application ne parvient pas à assainir les entrées utilisateur avant de les incorporer dans des fichiers HTML traités par le moteur SSI du serveur. Les attaquants exploitent cette vulnérabilité en injectant des directives SSI, telles que `<!--#exec cmd="id" -->`, pour exécuter des commandes shell arbitraires, lire des fichiers locaux ou exfiltrer des variables d'environnement. Cette vulnérabilité se trouve généralement dans les applications servant des extensions de fichiers héritées telles que `.shtml`, `.shtm` ou `.stm`, ou lorsque le serveur web est explicitement configuré pour analyser le SSI dans des fichiers HTML standards. Une exploitation réussie accorde à l'attaquant les mêmes permissions que le processus du serveur web, ce qui conduit souvent à un compromis complet du système ou à l'exposition de données sensibles.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Le serveur web prend-il en charge et traite-t-il les directives SSI ?
- [ ] Non — le serveur **ne prend pas** en charge le SSI ou les extensions de fichiers comme `.shtml` sont **désactivées**  
- [ ] Oui — le SSI **est activé** mais les entrées utilisateur ne sont **pas** reflétées dans les pages traitées  
- [ ] Oui — le SSI **est activé** et les entrées utilisateur **sont** reflétées dans les pages traitées  

### Des commandes système arbitraires peuvent-elles être exécutées via la directive `#exec` ?
- [ ] Non — la directive `#exec` est **désactivée** ou restreinte par la configuration du serveur  
- [ ] Oui — l'exécution de commandes **est possible** via des payloads `#exec cmd` ou `#exec cgi`  

### L'exfiltration de fichiers sensibles ou de variables d'environnement est-elle possible ?
- [ ] Non — les directives comme `#include` ou `#config` sont **désactivées**  
- [ ] Oui — la lecture de fichiers locaux (ex. : `/etc/passwd`) **est possible** via `#include virtual`  
- [ ] Oui — les variables d'environnement du serveur **peuvent** être exfiltrées via `#printenv` ou `#echo`  

### Les contrôles de validation des entrées ou du WAF sont-ils efficaces contre les payloads SSI ?
- [ ] Oui — une validation stricte des entrées **est appliquée** et aucun contournement n'est **possible**  
- [ ] Oui — la validation/WAF **est appliqué(e)** mais un contournement **est possible** en utilisant l'encodage de caractères ou une syntaxe SSI différente  
- [ ] Non — aucune validation ou protection WAF n'est présente pour les caractères SSI (`<`, `!`, `#`, `-`, `"`)

---

## WSTG-INPV-09 — Test d'injection XPath (XPath Injection)

L'injection XPath (XPath Injection) se produit lorsqu'une application utilise des informations fournies par l'utilisateur pour construire une requête XPath pour des données XML, permettant à un attaquant d'interférer avec la logique de la requête. En injectant une syntaxe spécifique telle que des guillemets simples, des crochets et des opérateurs logiques, un attaquant peut naviguer dans la structure du document XML, contourner l'authentification ou exfiltrer des nœuds de données sensibles. Cette vulnérabilité se trouve couramment dans les services web, les interfaces de gestion de configuration et les systèmes hérités qui s'appuient sur des magasins de données basés sur XML plutôt que sur des bases de données SQL traditionnelles. Du point de vue d'un attaquant, cela est souvent exploité à l'aide de techniques basées sur les booléens ou les erreurs pour cartographier systématiquement l'arborescence XML et récupérer des valeurs cachées du backend.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Outils :** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Les entrées utilisateur utilisées pour construire les requêtes XPath sont-elles correctement assainies ou paramétrées ?
- [ ] Oui — les entrées ne sont **pas** utilisées dans les requêtes XPath ou sont strictement paramétrées  
- [ ] Oui — une validation/un encodage robuste des entrées **est appliqué** et un contournement n'est **pas possible**  
- [ ] Oui — une certaine validation **est appliquée** mais un contournement **est possible** en utilisant des caractères encodés  
- [ ] Non — l'entrée utilisateur est directement concaténée dans les requêtes XPath *(Critique)*  

### L'application révèle-t-elle des détails structurels XML/XPath via des messages d'erreur ?
- [ ] Non — des pages d'erreur génériques sont affichées et **aucun** détail XML n'est divulgué  
- [ ] Oui — les messages d'erreur révèlent la présence d'un traitement XML mais **aucun** détail de chemin  
- [ ] Oui — des messages d'erreur verbeux révèlent la syntaxe XPath, les noms de nœuds ou les chemins de fichiers  

### Est-il possible de contourner la logique d'authentification ou d'autorisation en utilisant la logique XPath ?
- [ ] Non — la logique d'authentification ne repose **pas** sur XML/XPath ou est implémentée de manière sécurisée  
- [ ] Oui — les vérifications de connexion ou de permissions peuvent être contournées à l'aide de payloads basés sur la logique comme `' or 1=1 or '`  

### La structure ou les données du document XML peuvent-elles être énumérées via une injection aveugle (Blind Injection) ?
- [ ] Non — l'application n'est **pas** sensible à l'inférence basée sur les booléens ou sur le temps  
- [ ] Oui — les noms de nœuds et les données **peuvent** être exfiltrés en utilisant l'inférence basée sur les booléens  
- [ ] Oui — le parcours complet de l'arborescence XML et l'extraction de données **sont possibles**

---

## WSTG-INPV-10 — Injection IMAP SMTP

Les vulnérabilités d'injection IMAP et SMTP surviennent lorsqu'une application web filtre incorrectement les données fournies par l'utilisateur avant de les incorporer dans des commandes envoyées à un serveur de messagerie. En injectant des caractères Carriage Return et Line Feed (CRLF), un attaquant peut mettre fin à la commande prévue et ajouter des instructions de messagerie arbitraires, telles que l'ajout de destinataires supplémentaires (CC/BCC) ou la modification du corps du message. Ces failles se trouvent le plus souvent dans les passerelles web-to-mail, les formulaires de contact et les clients de messagerie qui communiquent directement avec les serveurs de messagerie via des protocoles tels qu'IMAP4 ou SMTP. Une exploitation réussie permet le relais non autorisé de spam, l'exfiltration de contenus d'e-mails sensibles et, dans certains cas, la compromission totale de l'intégrité du serveur de messagerie.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### L'application traite-t-elle les entrées fournies par l'utilisateur pour construire des commandes IMAP ou SMTP ?
- [ ] Non — l'application n'interagit **pas** avec les serveurs de messagerie via les entrées utilisateur  
- [ ] Oui — l'application interagit avec les serveurs de messagerie mais utilise une API ou une bibliothèque sécurisée  
- [ ] Oui — l'application construit des commandes de messagerie brutes en utilisant des paramètres contrôlés par l'utilisateur  

### L'entrée est-elle validée pour empêcher l'injection de séquences Carriage Return (`\r`) et Line Feed (`\n`) ?
- [ ] Oui — les caractères CRLF sont **strictement** filtrés, encodés ou rejetés  
- [ ] Oui — l'entrée est validée par rapport à une liste d'autorisation (allowlist) stricte de caractères  
- [ ] Non — les caractères CRLF ne sont **pas** filtrés, permettant la terminaison et l'injection de commandes  

### Un attaquant peut-il injecter des en-têtes de messagerie supplémentaires tels que `Bcc:` ou `Cc:` ?
- [ ] Non — la modification d'en-tête n'est **pas possible** en raison de contraintes d'entrée strictes  
- [ ] Oui — l'injection d'en-têtes supplémentaires **est possible** via des séquences CRLF  
- [ ] Oui — le relais de messagerie vers des domaines externes **est activé** et exploitable *(Critique)*  

### L'application permet-elle la manipulation de commandes IMAP pour accéder à des boîtes aux lettres non autorisées ?
- [ ] Non — l'application n'utilise **pas** IMAP ou limite strictement l'accès aux boîtes aux lettres à l'utilisateur authentifié  
- [ ] Oui — l'injection de commandes IMAP **est possible** mais limitée à l'énumération de métadonnées  
- [ ] Oui — l'injection de commandes IMAP permet la lecture ou la suppression des e-mails d'autres utilisateurs  

### Le serveur de messagerie backend est-il vulnérable au pipelining de commandes ?
- [ ] Non — le serveur de messagerie rejette les commandes groupées ou les commandes multiples au cours d'une seule session  
- [ ] Oui — le serveur de messagerie traite plusieurs commandes injectées via un seul champ d'entrée

---

## WSTG-INPV-11 — Injection de Code (Code Injection)

L'injection de code se produit lorsqu'une application évalue des fragments de code fournis par l'utilisateur au sein de son environnement d'exécution, généralement via des fonctions spécifiques au langage non sécurisées telles que `eval()`, `exec()` ou `system()`. Cette vulnérabilité diffère de l'injection de commandes car elle cible le contexte d'exécution du langage de programmation (par exemple, PHP, Python, Node.js) plutôt que le shell du système d'exploitation sous-jacent. Les attaquants exploitent ces points pour exécuter une logique arbitraire, exfiltrer des variables d'environnement ou obtenir une exécution de code à distance (Remote Code Execution - RCE) complète sur le serveur. Les Pentesters doivent examiner les fonctionnalités qui traitent des expressions mathématiques, des templates côté serveur (Server-Side Templates) ou des paramètres de configuration dynamiques où l'entrée pourrait être interprétée comme du code exécutable.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Outils :** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Est-ce que des fonctionnalités de l'application évaluent des entrées dynamiques via des fonctions d'évaluation spécifiques au langage ?
- [ ] Non — les fonctions d'évaluation dynamique **ne sont pas** présentes dans la base de code  
- [ ] Oui — des fonctions comme `eval()`, `exec()` ou `include()` sont utilisées mais n'acceptent **pas** d'entrées utilisateur  
- [ ] Oui — des fonctions sont utilisées et traitent des entrées fournies par l'utilisateur  

### Des mécanismes de validation ou d'assainissement (Sanitization) des entrées sont-ils appliqués avant l'évaluation ?
- [ ] Oui — l'entrée est strictement validée par rapport à une **liste blanche (Whitelist) codée en dur**  
- [ ] Oui — l'entrée est assainie via une liste noire (Blacklisting), mais un **contournement (Bypass) est possible**  
- [ ] Non — l'entrée est transmise directement à la fonction d'évaluation et **aucun contrôle** n'est appliqué  

### L'environnement d'exécution est-il isolé (Sandboxed) ou restreint pour empêcher l'accès au niveau système ?
- [ ] Oui — l'exécution a lieu dans un bac à sable (Sandbox) hautement restreint où les appels système **ne peuvent pas** être effectués  
- [ ] Oui — certaines restrictions existent, mais une **évasion de bac à sable (Sandbox Escape) est possible**  
- [ ] Non — l'environnement permet un accès complet aux API du langage et aux fonctions de l'OS sous-jacentes *(Critique)*  

### L'injection de code peut-elle être utilisée pour exfiltrer des informations sensibles ?
- [ ] Non — l'exécution est aveugle (Blind) et aucune communication hors bande (Out-of-band) **n'est possible**  
- [ ] Oui — des variables d'environnement ou des fichiers locaux **peuvent** être exfiltrés via des requêtes HTTP/DNS  
- [ ] Oui — les résultats du code exécuté sont **directement reflétés** dans la réponse de l'application  

### L'application permet-elle l'inclusion de fichiers à distance (Remote File Inclusion) ou le chargement dynamique de bibliothèques ?
- [ ] Non — seuls des chemins de code locaux et prédéfinis peuvent être exécutés  
- [ ] Oui — l'application **peut** être forcée à charger et exécuter du code provenant d'une source distante ou contrôlée par l'attaquant

---

## WSTG-INPV-12 — Command Injection

L'injection de commandes (Command Injection) se produit lorsqu'une application transmet des données non sécurisées fournies par l'utilisateur, telles que des entrées de formulaire, des en-têtes HTTP ou des cookies, à un shell système. Cette vulnérabilité permet à un attaquant d'exécuter des commandes arbitraires du système d'exploitation sur le serveur, généralement avec les privilèges du processus de l'application web. Du point de vue d'un attaquant, cela est exploité en utilisant des métacaractères de shell tels que des points-virgules, des pipes ou des backticks pour enchaîner des commandes malveillantes au flux d'exécution prévu. L'impact est fréquemment catastrophique, menant à une compromission totale du système, à l'exfiltration de données non autorisée ou à un mouvement latéral au sein du réseau interne.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **État du test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Outils :** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### L'application invoque-t-elle des commandes de niveau système ou des exécutables shell ?
- [ ] Non — l'application n'interagit **pas** avec le shell de l'OS  
- [ ] Oui — l'application utilise des API internes ou des bibliothèques de haut niveau pour les tâches système  
- [ ] Oui — l'application invoque directement des commandes shell via des appels système tels que `system()`, `exec()` ou `popen()`  

### Les entrées utilisateur sont-elles validées ou assainies avant d'être transmises aux appels système ?
- [ ] Oui — une liste d'autorisation (allow-listing) stricte et une validation des entrées **sont appliquées**  
- [ ] Oui — l'assainissement (sanitization) ou l'échappement est utilisé mais un contournement (bypass) **est possible**  
- [ ] Non — les entrées utilisateur sont transmises directement au shell **sans** validation  

### Les métacaractères de shell peuvent-ils être utilisés pour manipuler le flux d'exécution des commandes ?
- [ ] Non — les métacaractères sont correctement filtrés ou échappés  
- [ ] Oui — une exécution en bande (in-band) où la sortie de la commande est reflétée dans la réponse **est possible**  
- [ ] Oui — une exécution aveugle (blind) où aucune sortie n'est reflétée **est possible**  

### Une exfiltration hors bande (Out-of-band - OOB) est-elle possible depuis l'hôte ?
- [ ] Non — le serveur n'a pas de sortie (egress) ou les signaux OOB (DNS/HTTP) échouent  
- [ ] Oui — l'exfiltration de données via des signaux OOB basés sur DNS ou HTTP **est possible**  

### Le processus de l'application s'exécute-t-il avec des privilèges système élevés ?
- [ ] Non — l'application s'exécute avec un compte de service dédié à faibles privilèges  
- [ ] Oui — l'application s'exécute en tant que `root`, `Administrator` ou un utilisateur à hauts privilèges *(Critique)*

---

## WSTG-INPV-13 — Format String Injection

L'injection de chaîne de format (Format String Injection) se produit lorsqu'une application web transmet une entrée contrôlée par l'utilisateur directement dans l'argument de chaîne de format d'une fonction variadique, telle que la famille `printf` en C ou des wrappers de journalisation (logging) de bas niveau. Bien que moins courante dans les frameworks web modernes à code managé, cette vulnérabilité reste critique dans les applications CGI héritées (legacy), les extensions natives ou les systèmes de journalisation spécifiques interagissant avec des bibliothèques système de bas niveau. Les attaquants exploitent cela en fournissant des spécificateurs de format tels que `%x` ou `%p` pour divulguer des adresses mémoire sensibles et des données de la pile (stack), ou le spécificateur `%n` pour effectuer des écritures mémoire arbitraires. Une exploitation réussie entraîne généralement un compromis complet du système via une exécution de code à distance (Remote Code Execution - RCE) ou, au minimum, une condition de déni de service (DoS) impactante par le biais de plantages de processus.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### L'application traite-t-elle les entrées utilisateur via du code natif ou des interfaces CGI héritées ?
- [ ] Non — l'application est entièrement construite sur des frameworks à code managé (ex: JVM, CLR)  
- [ ] Oui — l'application utilise JNI, des extensions C/C++ natives ou des CGI binaires hérités où des vulnérabilités de type format string **peuvent** exister  

### Les entrées fournies par l'utilisateur sont-elles transmises comme argument principal à des fonctions sensibles au format ?
- [ ] Non — les entrées sont transmises comme arguments de données, et les chaînes de format sont **statiques** et **codées en dur**  
- [ ] Oui — l'entrée utilisateur est directement concaténée dans l'argument de chaîne de format, mais une désinfection (sanitization) **est appliquée**  
- [ ] Oui — l'entrée utilisateur est directement utilisée comme chaîne de format et aucune désinfection **n'est appliquée**  

### Est-il possible de divulguer le contenu de la mémoire à l'aide de spécificateurs de format ?
- [ ] Non — l'entrée est validée et les spécificateurs tels que `%p`, `%x` ou `%s` ne sont **pas traités**  
- [ ] Oui — la fourniture de spécificateurs `%p` ou `%x` entraîne la réflexion d'adresses mémoire de la pile (stack) ou du tas (heap)  
- [ ] Oui — des données sensibles (ex: pointeurs, stack cookies) **peuvent** être exfiltrées via des spécificateurs de format  

### L'application peut-elle être plantée ou manipulée à l'aide du spécificateur `%n` ?
- [ ] Non — les spécificateurs permettant des écritures en mémoire sont **désactivés** ou bloqués par le compilateur/l'environnement  
- [ ] Oui — l'application plante (DoS) lorsque `%s%s%s%s` ou des chaînes similaires sont fournies  
- [ ] Oui — des écritures mémoire arbitraires via `%n` sont **possibles**, menant potentiellement à une Remote Code Execution (RCE)  

### Les protections binaires modernes (ASLR, DEP, Stack Canaries) sont-elles contournables via cette injection ?
- [ ] Non — l'environnement est durci et la fuite de mémoire **ne peut pas** être utilisée pour contourner les protections  
- [ ] Oui — l'injection de chaîne de format **peut** être utilisée pour divulguer des adresses de base, rendant le contournement de l'ASLR/DEP **possible**

---

## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Les vulnérabilités incubées, également appelées vulnérabilités persistantes ou de second ordre, surviennent lorsqu'un Payload malveillant est stocké par l'application dans un état « froid » (cold state), puis exécuté ou traité ultérieurement dans un contexte différent. Ce modèle d'attaque cible généralement les systèmes Back-end, les consoles d'administration ou les outils de reporting automatisés qui récupèrent des données à partir d'une base de données partagée ou d'un système de fichiers sans re-validation adéquate ou Output Encoding. Les Pentesters doivent tracer le cycle de vie de l'entrée utilisateur depuis le point d'entrée (l'« incubateur ») jusqu'à chaque emplacement où ces données sont finalement rendues ou utilisées par d'autres composants ou applications secondaires. Une exploitation réussie entraîne souvent des conséquences à fort impact, telles que le détournement de session administrative via un Stored XSS ou l'exfiltration de données par une Second-order SQL Injection dans des modules de reporting internes.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Outils :** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Existe-t-il des points de terminaison où les entrées contrôlées par l'utilisateur sont stockées pour une récupération ultérieure par d'autres utilisateurs ou processus ?
- [ ] Non — l'application ne stocke **pas** les entrées utilisateur pour une utilisation ultérieure  
- [ ] Oui — les entrées **sont** stockées dans des champs de base de données, des fichiers journaux ou des paramètres de configuration  

### Une validation des entrées ou une sanitisation est-elle appliquée avant que les données ne soient écrites dans la couche de stockage persistant ?
- [ ] Oui — une validation stricte par Allow-list **est appliquée** au point d'entrée  
- [ ] Oui — une sanitisation générique **est appliquée** mais un contournement **est possible** via l'encodage ou des caractères non standards  
- [ ] Non — l'entrée est stockée sous sa forme brute, non validée  

### Les données stockées sont-elles correctement encodées ou sanitisées lorsqu'elles sont rendues dans des interfaces administratives ou secondaires ?
- [ ] Oui — un Output Encoding sensible au contexte **est appliqué** partout où les données sont rendues  
- [ ] Oui — l'encodage **est appliqué** à certains endroits mais **manquant** à d'autres (par exemple, tableau de bord d'administration interne)  
- [ ] Non — les données sont rendues sans aucun encodage ni sanitisation, permettant l'exécution du Payload  

### Les payloads stockés dans la base de données peuvent-ils affecter les processus batch en Back-end ou les systèmes de surveillance Out-of-band ?
- [ ] Non — les processus Back-end utilisent des méthodes de parsing sûres ou des requêtes paramétrées  
- [ ] Oui — les payloads stockés **peuvent** déclencher des interactions dans la logique Back-end (par exemple, CSV Injection, Command Injection dans les logs)  
- [ ] Oui — les payloads stockés **peuvent** déclencher des requêtes Out-of-band (OOB) vers des serveurs contrôlés par l'attaquant

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

Les vulnérabilités de type HTTP Splitting et Smuggling surviennent suite à des divergences dans la manière dont les proxys frontend et les serveurs backend interprètent et traitent les limites des requêtes HTTP, plus particulièrement en ce qui concerne les en-têtes `Content-Length` et `Transfer-Encoding`. En concevant des requêtes ambiguës, un attaquant peut « smuggler » une requête cachée vers le backend ou injecter des séquences CRLF pour diviser une réponse, ce qui entraîne un Cache Poisoning, un Request Hijacking ou un contournement des contrôles de sécurité. Ces failles se manifestent généralement dans des environnements complexes utilisant des reverse proxies, des load balancers ou des CDNs qui possèdent des logiques de parsing incohérentes. Du point de vue d'un attaquant, cela permet la redirection du trafic utilisateur, le vol de jetons de session sensibles et l'exécution d'actions non autorisées dans le contexte des sessions d'autres utilisateurs.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Outils :** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### L'environnement est-il sensible au Request Smuggling via des divergences CL.TE ou TE.CL ?
- [ ] Non — les serveurs frontend et backend gèrent les limites de requêtes de manière cohérente  
- [ ] Oui — des divergences existent mais l'exploitation n'est **pas possible** en raison des mesures d'atténuation de l'infrastructure  
- [ ] Oui — le smuggling CL.TE ou TE.CL **est possible**, permettant à des requêtes cachées d'atteindre le backend *(Élevée)*  
- [ ] Oui — le TE.TE (double encodage/obfuscation) **est possible** pour contourner les filtres frontend *(Critique)*  

### Le HTTP Response Splitting peut-il être réalisé via une injection CRLF dans les en-têtes ?
- [ ] Non — l'application assainit correctement les séquences CRLF dans toutes les entrées d'en-tête  
- [ ] Oui — les séquences CRLF sont réfléchies dans les en-têtes mais le Response Splitting n'est **pas possible**  
- [ ] Oui — l'injection CRLF **est possible**, permettant l'injection d'en-tête ou le Cache Poisoning  

### Les contrôles de sécurité (WAF/ACLs) sont-ils contournés à l'aide de requêtes « smuggled » ?
- [ ] Non — les contrôles de sécurité s'appliquent à la fois aux requêtes externes et aux requêtes passées en fraude  
- [ ] Oui — les requêtes passées en fraude **peuvent** contourner les règles WAF du frontend ou les ACL basées sur l'IP  

### Est-il possible de détourner les sessions d'autres utilisateurs ou de rediriger leur trafic ?
- [ ] Non — les flux de requête/réponse sont isolés et ne peuvent pas être croisés  
- [ ] Oui — le Request Smuggling **est possible** et permet de capturer les requêtes d'autres utilisateurs *(Critique)*  
- [ ] Oui — le Response Splitting **est possible** et permet un Cache Poisoning côté navigateur ou une XSS

---

## WSTG-INPV-16 — HTTP Request Smuggling

L'HTTP Request Smuggling (HRS) se produit lorsqu'une chaîne de serveurs HTTP (tels qu'un équilibreur de charge et un serveur web back-end) interprète les limites d'une requête de manière différente, généralement en raison d'en-têtes `Content-Length` et `Transfer-Encoding` conflictuels. Un attaquant exploite cette désynchronisation pour « smuggler » (introduire clandestinement) une entrée dans le tampon de requête du serveur back-end, préfixant ainsi la requête de l'utilisateur légitime suivant avec un segment contrôlé par l'attaquant. Cette technique permet des attaques graves, notamment le session hijacking, le contournement des contrôles de sécurité et le cache poisoning. L'exploitation est généralement réalisée par une analyse basée sur le temps (timing-based analysis) ou en observant des réponses inattendues du back-end lorsque des requêtes ultérieures sont envoyées au serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Critique / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Outils :** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Les serveurs front-end et back-end gèrent-ils l'en-tête `Transfer-Encoding: chunked` de manière cohérente ?
- [ ] Oui — les deux serveurs gèrent le codage par blocs (chunked encoding) de manière identique et la désynchronisation est **impossible**  
- [ ] Non — les serveurs interprètent les en-têtes différemment mais un assainissement ou une normalisation **est appliqué(e)**  
- [ ] Non — la désynchronisation CL.TE (Content-Length/Transfer-Encoding) **est possible** *(Critique)*  
- [ ] Non — la désynchronisation TE.CL (Transfer-Encoding/Content-Length) **est possible** *(Critique)*  

### L'attaquant peut-il empoisonner le cache côté serveur ou côté client à l'aide d'une requête « smuggled » ?
- [ ] Non — la mise en cache est **désactivée** ou n'est pas susceptible d'être empoisonnée  
- [ ] Oui — les requêtes « smuggled » **peuvent** rediriger les utilisateurs vers des domaines malveillants ou injecter des scripts via le cache poisoning  

### Est-il possible de capturer les requêtes ou les jetons de session (session tokens) d'autres utilisateurs via la concaténation de requêtes ?
- [ ] Non — le smuggling ne **permet pas** l'exposition de données entre utilisateurs  
- [ ] Oui — le smuggling permet d'ajouter la requête de l'utilisateur suivant à un corps `POST` contrôlé par l'attaquant, rendant l'exfiltration **possible**  

### L'infrastructure utilise-t-elle HTTP/2, et est-elle sensible à la désynchronisation H2.CL ou H2.TE ?
- [ ] Non — l'application utilise uniquement HTTP/1.1 ou HTTP/2 est implémenté de manière sécurisée sans downgrade  
- [ ] Oui — des downgrades en texte clair de HTTP/2 vers HTTP/1.1 se produisent et la désynchronisation **est possible**  

### Les en-têtes malformés (par exemple, `Transfer-Encoding:  chunked` avec un espace) sont-ils gérés de manière sécurisée ?
- [ ] Oui — les en-têtes malformés sont rejetés ou normalisés sur tous les niveaux  
- [ ] Non — la désynchronisation TE.TE **est possible** en raison d'une gestion incohérente des en-têtes malformés ou masqués

---

## WSTG-INPV-17 — Testing for Host Header Injection

L'injection d'en-tête Host (Host Header Injection) se produit lorsqu'une application fait confiance à l'en-tête `Host` fourni par l'utilisateur et l'incorpore dans la logique côté serveur, telle que la génération de liens ou les redirections, sans validation suffisante. Les attaquants manipulent cet en-tête pour faciliter l'empoisonnement de cache web (Web Cache Poisoning), l'empoisonnement de réinitialisation de mot de passe (Password Reset Poisoning) en redirigeant des jetons sensibles vers un domaine externe, ou pour contourner les contrôles de sécurité reposant sur le routage basé sur les en-têtes. Une exploitation réussie permet à un attaquant d'exfiltrer des données de session, d'effectuer des prises de contrôle de compte (Account Takeover) ou de rediriger des utilisateurs peu méfiants vers une infrastructure malveillante. Cette vulnérabilité est plus couramment rencontrée dans les environnements avec équilibrage de charge (load-balanced) ou dans les applications qui génèrent dynamiquement des URLs absolues basées sur le contexte de la requête entrante.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si l'en-tête Host est utilisé pour générer des liens sensibles dans des e-mails, tels que les réinitialisations de mot de passe.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Outils :** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### L'application reflète-t-elle la valeur de l'en-tête `Host` dans les liens, redirections ou scripts ?
- [ ] Non — l'application utilise des URLs de base codées en dur (hardcoded) ou une configuration strictement validée  
- [ ] Oui — l'en-tête `Host` est reflété mais strictement validé par rapport à une liste blanche (whitelist)  
- [ ] Oui — l'en-tête `Host` est reflété et un contournement **est possible** via des valeurs malformées (ex: `expected.com:attacker.com`)  
- [ ] Oui — l'en-tête `Host` est reflété **sans** aucune validation  

### Des en-têtes alternatifs liés à l'hôte sont-ils traités par l'application ou l'infrastructure ?
- [ ] Non — les en-têtes tels que `X-Forwarded-Host`, `X-Host` ou `Forwarded` sont ignorés  
- [ ] Oui — les en-têtes alternatifs sont traités mais ne **surchargent pas** la logique interne  
- [ ] Oui — les en-têtes alternatifs **peuvent** être utilisés pour surcharger la valeur de l'en-tête `Host` principal  

### L'en-tête `Host` peut-il être manipulé pour empoisonner les e-mails générés par le système (ex: réinitialisations de mot de passe) ?
- [ ] Non — les liens dans les e-mails utilisent un domaine statique et de confiance provenant de la configuration du serveur  
- [ ] Oui — l'en-tête `Host` influence les liens des e-mails mais la validation **ne peut pas** être contournée  
- [ ] Oui — l'en-tête `Host` **peut** être manipulé pour rediriger les jetons de réinitialisation de mot de passe vers un domaine contrôlé par l'attaquant *(Critique)*  

### L'application est-elle susceptible au Web Cache Poisoning via l'en-tête `Host` ?
- [ ] Non — aucun mécanisme de mise en cache n'est présent ou l'en-tête `Host` fait partie de la clé de cache (cache key)  
- [ ] Oui — un cache est présent mais il valide strictement ou ignore l'en-tête `Host` pour les entrées hors clé (unkeyed inputs)  
- [ ] Oui — un cache existe et **peut** être empoisonné par un en-tête `Host` injecté pour servir du contenu malveillant à d'autres utilisateurs  

### Le serveur accepte-t-il des en-têtes `Host` arbitraires pour le routage d'hôtes virtuels (virtual host routing) ?
- [ ] Non — le serveur renvoie une erreur ou une page par défaut pour les en-têtes `Host` non reconnus  
- [ ] Oui — le serveur accepte n'importe quel en-tête `Host`, ce qui **est** utile pour la reconnaissance ou l'énumération de services internes

---

## WSTG-INPV-18 — Test d'Injection de Modèles Côté Serveur (SSTI)

L'Injection de modèles côté serveur (SSTI) se produit lorsqu'une application intègre des entrées contrôlées par l'utilisateur dans un moteur de template côté serveur sans assainissement ou échappement suffisant. Cela permet à un attaquant d'injecter des directives de template malveillantes, qui sont ensuite exécutées sur le serveur, pouvant potentiellement mener à un compromis complet du système via une exécution de code à distance (RCE). La SSTI se manifeste généralement dans des fonctionnalités telles que la génération dynamique d'e-mails, les pages de profil et les systèmes de notification utilisant des moteurs tels que Jinja2, Twig ou FreeMarker. Du point de vue d'un attaquant, cette vulnérabilité est souvent exploitée pour divulguer des variables d'environnement sensibles, lire des fichiers internes ou exécuter des commandes système en exploitant les objets et méthodes natifs du moteur de template.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **État du test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Outils :** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### L'application utilise-t-elle un moteur de template côté serveur pour traiter les entrées utilisateur ?
- [ ] Non — l'application n'utilise **pas** de modèles côté serveur pour le contenu dynamique  
- [ ] Oui — des modèles sont utilisés mais l'entrée utilisateur n'est **pas** intégrée dans les directives de template  
- [ ] Oui — l'entrée utilisateur est intégrée dans les modèles **sans** assainissement approprié  

### Des expressions arithmétiques simples sont-elles évaluées lorsqu'elles sont injectées dans les champs de saisie ?
- [ ] Non — les payloads comme `{{7*7}}` ou `${7*7}` sont reflétés comme des chaînes de caractères littérales  
- [ ] Oui — les expressions sont évaluées (par exemple, retournant `49`), confirmant que le moteur de template **est vulnérable**  

### Les objets ou méthodes intégrés du moteur de template peuvent-ils être consultés ?
- [ ] Non — l'accès aux objets, méthodes ou configurations dangereux est **restreint** ou isolé dans un bac à sable  
- [ ] Oui — les objets intégrés (par exemple, `self`, `config`, `__class__`) **peuvent** être consultés pour divulguer des données sensibles  
- [ ] Oui — l'exécution de code à distance (RCE) via des appels système ou des bibliothèques de l'OS **est possible**  

### Existe-t-il un mécanisme de bac à sable (sandbox) ou de liste d'autorisation (allowlist) empêchant l'exécution de directives dangereuses ?
- [ ] Oui — une liste d'autorisation stricte ou un bac à sable sécurisé est en place et aucun contournement n'est **possible**  
- [ ] Oui — un bac à sable est présent mais un contournement **est possible** via des payloads spécifiques dépendants du moteur  
- [ ] Non — aucun bac à sable ni aucune liste d'autorisation n'est **appliqué** au moteur de template

---

## WSTG-INPV-19 — Test de Server-Side Request Forgery (SSRF)

Le Server-Side Request Forgery (SSRF) survient lorsqu'un attaquant peut influencer une application web pour qu'elle effectue des requêtes non autorisées depuis le serveur vers des ressources internes ou externes. En manipulant des paramètres qui attendent des URL, des noms d'hôte ou des adresses IP, un attaquant peut contourner les contrôles au niveau du réseau tels que les pare-feu et les ACL pour sonder des segments de réseau interne, accéder à des services de métadonnées cloud (IMDS) ou interagir avec des API et des bases de données internes vulnérables. Cette vulnérabilité se manifeste généralement dans des fonctionnalités impliquant la récupération d'images, la génération de PDF, les webhooks ou les téléchargements de fichiers via URL. Du point de vue de l'attaquant, l'SSRF est une primitive puissante utilisée pour pivoter dans des environnements isolés, exfiltrer des données de configuration sensibles ou potentiellement réaliser une exécution de code à distance (Exploit RCE) via l'interaction avec des services internes comme Redis ou Memcached.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Outils :** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### L'application traite-t-elle des URL ou des noms d'hôte fournis par l'utilisateur ?
- [ ] Non — aucune fonctionnalité n'accepte d'URL ou de noms d'hôte externes  
- [ ] Oui — la fonctionnalité existe mais l'entrée **est strictement validée** par rapport à une liste d'autorisation (allow-list)  
- [ ] Oui — la fonctionnalité existe et accepte des URL fournies par l'utilisateur  

### Des contrôles de validation ou des listes de blocage sont-ils appliqués à la destination demandée ?
- [ ] Oui — une **liste d'autorisation** (allow-list) stricte de domaines/IP de confiance est appliquée  
- [ ] Oui — une **liste de blocage** (deny-list) est utilisée (ex: blocage de `127.0.0.1`, `169.254.169.254`)  
- [ ] Non — **aucune validation** ou filtrage n'est appliqué à l'entrée  

### Est-il possible de contourner les filtres via l'encodage, les redirections ou le DNS rebinding ?
- [ ] Non — les filtres sont robustes et gèrent les redirections et la résolution DNS de manière sécurisée  
- [ ] Oui — un contournement **est possible** en utilisant l'encodage URL ou des formats d'IP alternatifs (hexadécimal, octal)  
- [ ] Oui — un contournement **est possible** via des redirections HTTP 3xx vers des cibles internes  
- [ ] Oui — un contournement **est possible** via des attaques de DNS rebinding pour résoudre des IP internes  

### L'application peut-elle atteindre des services de métadonnées internes ou des interfaces locales ?
- [ ] Non — le réseau local et les points de terminaison de métadonnées cloud ne sont **pas accessibles**  
- [ ] Oui — l'accès à localhost (`127.0.0.1`) ou aux services de loopback **est possible**  
- [ ] Oui — l'accès aux services de métadonnées cloud (ex: AWS/GCP/Azure IMDS) **est possible** *(Critique)*  

### Quelle est la nature de la réponse SSRF ?
- [ ] Aveugle (Blind) — aucune réponse ni métadonnée n'est retournée à l'utilisateur  
- [ ] Partielle (Partial) — des métadonnées de réponse (en-têtes, taille, timing) sont retournées à l'utilisateur  
- [ ] Complète (Full) — le corps complet de la réponse de la ressource interne **est reflété** à l'utilisateur

---

## WSTG-INPV-20 — Mass Assignment

Le Mass Assignment, également connu sous le nom d'Overposting ou de désérialisation non sécurisée de paramètres, se produit lorsqu'une application web lie automatiquement les paramètres de requête HTTP entrante aux propriétés d'objets internes sans filtrage approprié des attributs. Cette vulnérabilité est fréquente dans les frameworks MVC modernes et les APIs RESTful où les données JSON, XML ou encodées par formulaire sont directement désérialisées dans les modèles de données côté application. Les attaquants exploitent ce comportement en injectant des paramètres supplémentaires non répertoriés dans une requête — tels que `isAdmin`, `role` ou `account_balance` — pour modifier des champs sensibles que les développeurs n'avaient pas l'intention d'exposer au contrôle de l'utilisateur. Une exploitation réussie entraîne généralement une élévation de privilèges (Privilege Escalation), une modification de données non autorisée ou le contournement de workflows critiques de la logique métier.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Haute / Moyenne* |

> *La sévérité devient Haute si des attributs administratifs, des niveaux de permission ou des champs de facturation peuvent être modifiés.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### L'application utilise-t-elle la liaison automatique (automatic binding) pour les paramètres de requête entrante ?
- [ ] Non — les paramètres sont mappés manuellement à des variables spécifiques ou à des objets sécurisés  
- [ ] Oui — la liaison automatique est utilisée mais des **listes blanches** (white-lists) strictes ou des DTO (Data Transfer Objects) sont appliqués  
- [ ] Oui — la liaison automatique est utilisée et des attributs internes sensibles **peuvent** être modifiés  

### Des attributs administratifs ou liés aux permissions peuvent-ils être injectés avec succès ?
- [ ] Non — les paramètres injectés sont ignorés, supprimés ou provoquent une erreur de validation  
- [ ] Oui — les paramètres supplémentaires sont acceptés mais n'affectent **pas** l'état de l'objet backend  
- [ ] Oui — l'injection de paramètres tels que `is_admin`, `role` ou `permissions` permet **avec succès** une élévation de privilèges *(Critique)*  

### Est-il possible de modifier la logique métier sensible ou des champs financiers via l'overposting ?
- [ ] Non — les champs tels que `price`, `balance` ou `status` sont protégés contre le Mass Assignment  
- [ ] Oui — les attributs métier sensibles **peuvent** être modifiés en les ajoutant au corps de la requête JSON ou du formulaire  

### L'application révèle-t-elle des structures d'objets internes facilitant la devinette de paramètres ?
- [ ] Non — les réponses incluent uniquement les champs publics prévus et la documentation est restreinte  
- [ ] Oui — les champs d'objets internes sont visibles dans les réponses GET, rendant la découverte de paramètres **possible**  
- [ ] Oui — la documentation de l'API (Swagger/OpenAPI) énumère explicitement les propriétés internes qui **peuvent** être affectées

---

## WSTG-ERRH-01 — Test de la gestion incorrecte des erreurs

Une gestion incorrecte des erreurs se produit lorsqu'une application web divulgue des détails techniques sensibles—tels que des traces de pile d'exécution (stack traces), des informations sur le schéma de la base de données ou des chemins de fichiers internes—à travers ses réponses d'erreur. Les attaquants déclenchent ces erreurs en soumettant des entrées malformées, en accédant à des ressources inexistantes ou en provoquant des exceptions côté serveur afin de cartographier l'architecture sous-jacente de l'application et d'identifier des vecteurs potentiels pour une exploitation ultérieure. Cette fuite d'informations sert souvent de précurseur à des attaques plus graves, notamment l'SQL Injection ou le Path Traversal, en fournissant à l'attaquant des spécifications précises sur l'environnement. Une implémentation sécurisée doit fournir des messages génériques conviviaux pour l'utilisateur tout en capturant des diagnostics détaillés uniquement dans des journaux (logs) sécurisés côté serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **État du Test** | Non effectué |
| **Sévérité** | Basse / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### L'application utilise-t-elle un gestionnaire d'erreurs global et générique pour les exceptions non gérées ?
- [ ] Oui — des pages d'erreur génériques sont **activées** et ne révèlent **aucun** détail technique  
- [ ] Oui — des pages d'erreur personnalisées sont utilisées mais une fuite **est possible** via des en-têtes spécifiques  
- [ ] Non — les pages d'erreur par défaut du serveur (ex: Tomcat, IIS, Nginx) sont **visibles**  

### Des informations techniques peuvent-elles être énumérées via des entrées malformées ou des cas limites ?
- [ ] Non — les erreurs sont gérées de manière appropriée avec des identifiants de référence uniques pour la journalisation  
- [ ] Oui — des traces de pile (stack traces) ou des requêtes de base de données **sont divulguées** dans les réponses  
- [ ] Oui — des chemins de fichiers internes, des variables d'environnement ou des chaînes de version de serveur **sont divulgués**  

### Les réponses d'API divulguent-elles des objets d'erreur verbeux ou des informations de débogage ?
- [ ] Non — les API renvoient des codes d'erreur standardisés et des messages JSON assainis  
- [ ] Oui — les API renvoient des objets de débogage verbeux ou des détails complets sur l'exception dans le corps de la réponse  
- [ ] Oui — des traces de pile (stack traces) sont incluses dans les champs `details` ou `exception` de la réponse JSON  

### L'application se comporte-t-elle différemment (basé sur le temps ou le contenu) lorsque des erreurs surviennent ?
- [ ] Non — les signatures de réponse et les délais sont cohérents quel que soit le type d'erreur  
- [ ] Oui — des réponses différenciées **peuvent** être utilisées pour énumérer des états valides vs invalides (ex: énumération d'utilisateurs / User Enumeration)

---

## WSTG-ERRH-02 — Test de Stack Traces

Les stack traces sont générées lorsqu'une application ne parvient pas à gérer une exception de manière appropriée, ce qui entraîne la divulgation de l'état d'exécution interne et de la hiérarchie d'appels à l'utilisateur final. Pour un testeur de pénétration, ces traces sont inestimables car elles révèlent le framework de l'application, les versions des bibliothèques, les chemins système internes et le flux logique, ce qui réduit considérablement l'effort requis pour la reconnaissance. L'exploitation consiste à déclencher intentionnellement des erreurs via des entrées malformées, des types de données inattendus ou des violations de limites pour forcer l'application dans un état instable et exfiltrer des informations sur la stack technologique sousjacente. Cette fuite fournit souvent le contexte nécessaire pour identifier des composants tiers vulnérables ou pour concevoir des exploits spécifiques pour des failles logiques découvertes dans les chemins de code divulgués.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **État du Test** | Non effectué |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité passe à Moyenne si la stack trace révèle des détails architecturaux sensibles, des requêtes de base de données ou des paramètres de configuration interne.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Est-ce que des stack traces complètes sont renvoyées au client lors d'erreurs de l'application ?
- [ ] Non — l'application renvoie des pages d'erreur génériques ou des messages d'erreur personnalisés  
- [ ] Oui — les stack traces sont **activées** mais uniquement pour des modules spécifiques non sensibles  
- [ ] Oui — les stack traces complètes sont **activées** et visibles dans le corps de la réponse HTTP  

### Est-ce que les messages d'erreur divulguent des informations environnementales sensibles ?
- [ ] Non — les messages d'erreur ne contiennent aucun détail technique ou environnemental  
- [ ] Oui — les messages révèlent des **chemins de fichiers internes** ou des **structures de répertoires** côté serveur  
- [ ] Oui — les messages révèlent des **schémas de base de données**, des **requêtes SQL** ou des **versions de bibliothèques tierces**  

### Est-ce que les stack traces peuvent être déclenchées en manipulant les paramètres d'entrée ?
- [ ] Non — les entrées malformées sont gérées de manière appropriée ou rejetées par la validation  
- [ ] Oui — les traces peuvent être déclenchées par des **types de données inattendus** (ex : soumission d'un tableau là où une chaîne de caractères est attendue)  
- [ ] Oui — les traces sont déclenchées par des **octets nuls**, des **caractères spéciaux** ou des **valeurs limites**  

### Un gestionnaire d'erreurs global est-il appliqué de manière cohérente sur l'ensemble de l'application ?
- [ ] Oui — un gestionnaire d'exceptions global est **appliqué de manière cohérente** sur tous les points de terminaison testés  
- [ ] Oui — un gestionnaire est présent mais **n'est pas appliqué** aux endpoints hérités, aux API ou aux endpoints nouvellement ajoutés  
- [ ] No — aucun mécanisme de gestion d'erreurs global n'est **détecté**, ce qui entraîne des erreurs de serveur par défaut

---

## WSTG-CRYP-01 — Test des faiblesses de la sécurité de la couche de transport

Le test des faiblesses de la sécurité de la couche de transport (Transport Layer Security) implique l'analyse de l'implémentation et de la configuration de SSL/TLS pour garantir la confidentialité et l'intégrité des données pendant le transit. Les attaquants ciblent les mauvaises configurations telles que les protocoles obsolètes (SSLv2, TLS 1.0), les suites de chiffrement (Cipher Suites) faibles (NULL, RC4 ou anonymes) et les certificats invalides ou faiblement signés pour mener des attaques de type Man-in-the-Middle (MitM). Ce test cible généralement les points de terminaison du serveur web de l'application ou de l'équilibreur de charge (Load Balancer) où la communication chiffrée est établie. Une exploitation réussie permet à un adversaire d'intercepter des données sensibles, de détourner des sessions utilisateur ou de rétrograder les connexions vers des états non sécurisés via un Downgrade de protocole ou le déchiffrement du trafic.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Moyenne* |

> *La sévérité est Élevée si des données sensibles (PII, identifiants, jetons de session) sont transmises ; Moyenne pour le trafic informatif général.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Les protocoles TLS/SSL obsolètes ou non sécurisés sont-ils supportés ?
- [ ] Non — seuls TLS 1.2 et TLS 1.3 sont **activés**  
- [ ] Oui — TLS 1.0 ou TLS 1.1 sont **activés**  
- [ ] Oui — SSLv2 ou SSLv3 sont **activés** *(Critique)*  

### Des suites de chiffrement (cipher suites) faibles ou non sécurisées sont-elles acceptées par le serveur ?
- [ ] Non — seuls des chiffrements forts et modernes (ex : AES-GCM, ChaCha20) sont **supportés**  
- [ ] Oui — des chiffrements avec un cryptage faible (ex : 3DES, RC4) ou une faible longueur de bits sont **supportés**  
- [ ] Oui — les chiffrements NULL, anonymes (ADH) ou d'exportation sont **supportés** *(Critique)*  

### Le certificat du serveur est-il valide et fiable ?
- [ ] Oui — le certificat est valide, correspond au domaine et est signé par une **CA de confiance**  
- [ ] Non — le certificat a **expiré** ou utilise un **algorithme de signature faible** (ex : SHA-1)  
- [ ] Non — le certificat est **auto-signé** ou un **mismatch** de nom de domaine se produit  

### Le serveur supporte-t-il la Secure Renegotiation et empêche-t-il les attaques de type Downgrade ?
- [ ] Oui — la Secure Renegotiation est **supportée** et TLS Fallback SCSV est **activé**  
- [ ] Non — la Secure Renegotiation n'est **pas supportée**, permettant potentiellement une injection MitM  
- [ ] Non — le serveur est vulnérable aux attaques **POODLE** ou **ROBOT**  

### La sécurité HSTS (HTTP Strict Transport Security) est-elle correctement implémentée ?

| En-tête | Présent | Manquant |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Oui — l'en-tête HSTS est présent avec un `max-age` long et `includeSubDomains` **activé**  
- [ ] Oui — l'en-tête HSTS est présent mais l'option `includeSubDomains` est manquante ou le `max-age` est **court**  
- [ ] Non — l'en-tête HSTS n'est **pas présent**

---

## WSTG-CRYP-02 — Test de Padding Oracle

Les vulnérabilités de type Padding Oracle surviennent lorsque le processus de déchiffrement d'une application divulgue des informations indiquant si le remplissage (padding) d'un texte chiffré (ciphertext) déchiffré est valide ou non. En observant ces réponses par canaux auxiliaires (side-channel)—telles que des codes d'état HTTP distincts, des messages d'erreur spécifiques ou des variations de temps de réponse—un attaquant peut déchiffrer des données sans connaître la clé de chiffrement et potentiellement ré-encrypter du texte clair (plaintext) arbitraire. Cette vulnérabilité se manifeste généralement dans les cookies de session chiffrés, les jetons d'authentification ou les paramètres d'URL utilisant des chiffrements par blocs en mode Cipher Block Chaining (CBC) avec un remplissage PKCS#7. L'exploitation permet une perte totale de confidentialité et peut conduire à un compromis d'intégrité par la construction de blocs chiffrés forgés.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Critique* |

> *La sévérité est Critique si la vulnérabilité réside dans la gestion de session, les jetons d'identité ou le chiffrement de données PII.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Un chiffrement par blocs est-il utilisé en mode CBC pour les données sensibles ?
- [ ] Non — l'application utilise un chiffrement authentifié (AES-GCM/ChaCha20-Poly1305)  
- [ ] Oui — l'application utilise des chiffrements par blocs mais le remplissage (padding) **n'est pas** requis (ex : chiffrements de flux ou mode CTR)  
- [ ] Oui — l'application utilise le mode CBC et un remplissage (padding) **est appliqué**  

### Le serveur divulgue-t-il la validité du remplissage via des canaux auxiliaires ?
- [ ] Non — le serveur renvoie des messages d'erreur uniformes et des temps de réponse cohérents  
- [ ] Oui — le serveur renvoie différents codes d'état HTTP (ex : 200 vs 500) pour les erreurs de remplissage  
- [ ] Oui — le serveur renvoie des messages d'erreur spécifiques (ex : "Invalid Padding", "Decryption failed")  
- [ ] Oui — le serveur présente des différences de temps mesurables lors d'un échec de déchiffrement  

### Le déchiffrement automatisé du texte chiffré est-il possible ?
- [ ] Non — les canaux auxiliaires (side-channels) ne sont **pas** assez stables pour l'exploitation  
- [ ] Oui — le texte chiffré (ciphertext) **peut** être partiellement déchiffré (derniers blocs)  
- [ ] Oui — la récupération complète du texte clair (plaintext) **est possible** en utilisant des outils comme `PadBuster`  

### L'attaquant peut-il effectuer du bit-flipping ou forger des textes chiffrés valides ?
- [ ] Non — les contrôles d'intégrité (HMAC) sont vérifiés **avant** le déchiffrement  
- [ ] Oui — aucun contrôle d'intégrité n'est présent, mais la construction du Payload **n'est pas** réalisable  
- [ ] Oui — la construction de texte chiffré arbitraire **est possible**, permettant une élévation de privilèges ou une injection de données

---

## WSTG-CRYP-03 — Test de transmission d'informations sensibles via des canaux non chiffrés

La transmission de données sensibles via des canaux non chiffrés, tels que le protocole HTTP en clair, expose les informations à l'interception et à la modification par des attaques de type Man-in-the-Middle (MITM). Cette vulnérabilité survient généralement lorsque les applications web ne parviennent pas à imposer le Transport Layer Security (TLS) sur tous les points de terminaison (endpoints), en particulier dans les composants hérités (legacy), les formulaires de connexion ou lors de la phase initiale de transition de l'HTTP vers l'HTTPS. Du point de vue d'un attaquant, cela permet le sniffing passif des identifiants d'authentification, des jetons de session (session tokens) et des informations personnelles identifiables (PII) sur des segments de réseau compromis ou non approuvés, tels que les réseaux Wi-Fi publics. S'assurer que toutes les données sensibles sont encapsulées dans un tunnel TLS robuste est fondamental pour maintenir la confidentialité et l'intégrité des interactions des utilisateurs et prévenir le détournement de session (session hijacking).


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Moyenne |

> *La sévérité devient Élevée si des identifiants d'authentification, des identifiants de session ou des PII sont transmis en clair.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### L'application autorise-t-elle la communication via HTTP non chiffré pour les points de terminaison sensibles ?
- [ ] Non — tout le trafic de l'application est strictement forcé via HTTPS *(Le plus sécurisé)*  
- [ ] Oui — les pages non sensibles sont accessibles via HTTP mais les points de terminaison sensibles **ne le sont pas**  
- [ ] Oui — les points de terminaison sensibles (connexion, paiement, profil) **sont** accessibles via HTTP non chiffré *(Risque élevé)*  

### Existe-t-il une redirection globale de HTTP vers HTTPS ?
- [ ] Oui — l'application effectue une redirection 301/302 vers HTTPS pour toutes les requêtes HTTP  
- [ ] Oui — la redirection est effectuée uniquement pour des points de terminaison sensibles spécifiques  
- [ ] Non — aucune redirection de HTTP vers HTTPS **n'est appliquée**  

### Le protocole HTTP Strict Transport Security (HSTS) est-il implémenté ?
- [ ] Oui — HSTS est **activé** avec une valeur `max-age` longue et inclut les directives `includeSubDomains` et `preload`  
- [ ] Oui — HSTS est **activé** mais les directives `includeSubDomains` ou `preload` sont manquantes  
- [ ] Non — HSTS n'est **pas activé** sur le serveur d'application  

### Les cookies sensibles sont-ils protégés contre la transmission en clair ?

| Nom du cookie | Flag Secure présent | Flag Secure manquant |
|---------------|:-------------------:|:--------------------:|
| `SessionID`   |                     |                      |
| `AuthToken`   |                     |                      |
| `CSRF-Token`  |                     |                      |

### L'application transmet-elle des données sensibles à des API tierces via des canaux non chiffrés ?
- [ ] Non — tous les appels API externes sont effectués via des tunnels chiffrés (HTTPS)  
- [ ] Oui — certaines données de télémétrie non sensibles sont envoyées via HTTP  
- [ ] Oui — des clés d'API, des PII ou des identifiants **sont** envoyés à des tiers via HTTP non chiffré

---

## WSTG-CRYP-04 — Test des primitives cryptographiques faibles

Les primitives cryptographiques faibles impliquent l'utilisation d'algorithmes obsolètes ou non sécurisés, tels que MD5, SHA-1, DES ou RC4, qui sont sensibles aux attaques par collision, à l'analyse fréquentielle ou au déchiffrement par Brute Force. Ces faiblesses surviennent généralement dans le hachage de mots de passe, le chiffrement des données au repos, la génération de jetons de session ou la vérification de signatures numériques au sein du backend de l'application. Du point de vue d'un attaquant, l'identification de ces primitives permet de compromettre l'intégrité et la confidentialité des données, par exemple en cassant des condensats (hashes) interceptés pour récupérer des mots de passe en clair ou en forgeant des jetons d'authentification. Les applications modernes devraient plutôt utiliser des primitives standards de l'industrie, résistantes aux collisions et à haute entropie, comme Argon2, Bcrypt ou AES-GCM pour garantir une protection robuste contre la cryptanalyse.

| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Statut du test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Des algorithmes de hachage dépréciés tels que MD5 ou SHA-1 sont-ils utilisés pour le stockage de données sensibles ?
- [ ] Non — l'application utilise des algorithmes modernes résistants aux collisions tels que Argon2 ou Bcrypt  
- [ ] Oui — des algorithmes dépréciés sont utilisés mais **uniquement** pour des vérifications d'intégrité non sensibles à la sécurité  
- [ ] Oui — des algorithmes dépréciés **sont utilisés** pour des mots de passe ou des jetons sensibles *(Haute)*  

### Des algorithmes de chiffrement symétrique faibles comme DES, 3DES ou RC4 sont-ils actuellement employés ?
- [ ] Non — AES (128 bits ou plus) dans un mode sécurisé tel que GCM ou CBC est utilisé  
- [ ] Oui — des algorithmes hérités (legacy) sont **activés** pour la compatibilité descendante mais ne sont **pas** ceux par défaut  
- [ ] Oui — des algorithmes hérités sont la méthode principale pour protéger les données au repos ou en transit  

### L'application implémente-t-elle une logique cryptographique "maison" (roll-your-own) ou non standard ?
- [ ] Non — l'application adhère strictement à des bibliothèques cryptographiques standards et examinées par les pairs  
- [ ] Oui — des wrappers personnalisés existent mais les primitives sous-jacentes **sont** des standards de l'industrie  
- [ ] Oui — des algorithmes cryptographiques propriétaires ou une logique personnalisée **sont appliqués** aux données sensibles *(Critique)*  

### Les sels (salts) cryptographiques et les vecteurs d'initialisation (IV) sont-ils gérés selon les meilleures pratiques ?
- [ ] Oui — les sels sont uniques par utilisateur et les IV sont imprévisibles pour chaque opération  
- [ ] Oui — des sels sont utilisés mais ne sont **pas** uniques pour l'ensemble des enregistrements  
- [ ] Non — les sels sont statiques, codés en dur ou absents, et les IV **sont réutilisés** sur plusieurs sessions  

### La longueur de bit des clés asymétriques (RSA, Diffie-Hellman) est-elle suffisante selon les standards modernes ?
- [ ] Oui — les clés RSA/DH font 2048 bits ou plus, ou l'Elliptic Curve (ECC) est utilisée  
- [ ] Non — les clés font moins de 2048 bits mais le contournement n'est **pas** trivial  
- [ ] Non — les clés font 1024 bits ou moins, ce qui les rend **vulnérables** à la factorisation ou à des attaques computationnelles

---

## WSTG-BUSL-01 — Test de validation des données de la logique métier

Les tests de validation des données de la logique métier se concentrent sur la capacité de l'application à identifier et à rejeter les données qui sont syntaxiquement correctes mais logiquement invalides dans le contexte du domaine métier. Les attaquants exploitent ces failles en soumettant des valeurs inattendues — telles que des quantités négatives dans un panier d'achat, des dates passées pour des événements futurs ou des entiers extrêmement grands — pour contourner les contraintes et manipuler l'état de l'application. Ces vulnérabilités résident souvent dans des workflows en plusieurs étapes, des processus de paiement et des modules de gestion de compte où la logique côté serveur ne parvient pas à vérifier l'intégrité contextuelle des données fournies par l'utilisateur. Une exploitation réussie peut entraîner des pertes financières, une compromission de l'intégrité et un accès non autorisé à des fonctionnalités ou des données restreintes.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Outils :** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### L'application impose-t-elle des limites de plage logique sur les entrées numériques ?
- [ ] Oui — l'application rejette correctement les valeurs négatives, nulles ou excessivement grandes lorsqu'elles sont inappropriées *(Le plus sécurisé)*  
- [ ] Oui — l'application rejette certaines plages invalides mais **est** vulnérable à un dépassement d'entier (integer overflow) ou à un contournement de limite (boundary bypass)  
- [ ] Non — l'application accepte n'importe quelle valeur numérique sans tenir compte du contexte métier *(Critique)*  

### Les contrôles de validation côté serveur sont-ils cohérents à travers toutes les couches de l'application ?
- [ ] Oui — la validation est appliquée de manière cohérente au niveau des couches API/service et base de données  
- [ ] Oui — la validation **est appliquée** au niveau de l'UI/Frontend mais un contournement via une requête API directe **est possible**  
- [ ] Non — la validation n'est **pas appliquée** ou est incohérente, permettant une corruption logique des données  

### La logique métier peut-elle être détournée en manipulant les données dans des workflows en plusieurs étapes ?
- [ ] Non — l'état est maintenu de manière sécurisée sur le serveur et les données des étapes précédentes **ne peuvent pas** être altérées  
- [ ] Oui — la validation a lieu lors des premières étapes, mais la soumission finale **ne vérifie pas** à nouveau l'intégrité des données  
- [ ] Oui — le contournement du workflow **est possible** en sautant des étapes ou en soumettant des données dans le désordre  

### L'application gère-t-elle correctement les données de type "cas limites" (edge cases) qui contournent les contraintes métier ?
- [ ] Oui — les contraintes sont robustes et gèrent les entrées inhabituelles mais syntaxiquement valides (ex. : années bissextiles, changements de fuseau horaire)  
- [ ] Oui — certains cas limites sont gérés, mais des combinaisons spécifiques **peuvent** entraîner un comportement inattendu  
- [ ] Non — les cas limites entraînent directement des échecs logiques, tels que des achats gratuits ou des changements de statut non autorisés

---

## WSTG-BUSL-02 — Tester la capacité à forger des requêtes

La falsification de requêtes dans un contexte de logique métier implique la manipulation de paramètres définis par l'application pour effectuer des actions en dehors du flux de travail ou du niveau d'autorisation prévu. Les attaquants ciblent les processus en plusieurs étapes, tels que les systèmes de paiement ou l'enregistrement de compte, où l'état est maintenu via des paramètres côté client que le serveur ne parvient pas à re-valider par rapport à une source de confiance côté backend. En modifiant des champs cachés, des valeurs JSON ou des paramètres REST tels que les prix, les quantités ou les indicateurs d'état internes, un attaquant peut contourner les exigences de paiement, réaliser une escalade de privilèges ou déclencher des changements d'état imprévus. Cette vulnérabilité se manifeste généralement dans les formulaires web avec état ou les API où le serveur accorde une confiance implicite à la représentation de l'état métier fournie par le client lors d'une transaction.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Critique* |

> *La sévérité devient Critique si la requête forgée permet une perte financière, des actions administratives non autorisées ou une prise de contrôle totale de compte (Account Takeover).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### L'application valide-t-elle les paramètres transactionnels par rapport à une « source de vérité » côté serveur ?
- [ ] Oui — tous les paramètres sensibles (prix, rôle, ID) sont validés par rapport à la base de données et le contournement est **impossible**  
- [ ] Oui — la validation est appliquée mais peut être contournée via une pollution de paramètres (Parameter Pollution) ou un encodage alternatif  
- [ ] Non — l'application fait confiance aux valeurs côté client pour déterminer le coût ou l'issue d'une transaction *(Critique)*  

### Les valeurs critiques pour le métier (ex : quantités, montants) peuvent-elles être modifiées en valeurs non autorisées ?
- [ ] Non — les valeurs négatives, les valeurs nulles ou les valeurs excessivement élevées sont rejetées par le serveur  
- [ ] Oui — le serveur accepte les valeurs modifiées mais seulement dans une plage limitée  
- [ ] Oui — des valeurs négatives ou nulles **peuvent** être soumises, entraînant des contournements financiers ou de logique  

### Des champs cachés ou des paramètres d'API sont-ils utilisés pour maintenir l'état au cours de processus en plusieurs étapes ?
- [ ] Non — l'état est maintenu de manière sécurisée via des sessions côté serveur ou des jetons (tokens) signés  
- [ ] Oui — l'état est transmis via le client mais les contrôles d'intégrité (HMAC/Signatures) sont **activés** et robustes  
- [ ] Oui — l'état est transmis via le client et les contrôles d'intégrité sont **désactivés** ou peuvent être supprimés  

### Est-il possible de forger des requêtes qui ignorent des étapes intermédiaires obligatoires dans un flux de travail (workflow) ?
- [ ] Non — le serveur impose une séquence stricte d'opérations pour chaque transaction  
- [ ] Oui — des étapes **peuvent** être ignorées mais l'action finale nécessite toujours des données valides provenant des étapes précédentes  
- [ ] Oui — la requête finale de « soumission » ou de « finalisation » **peut** être forgée directement pour contourner l'autorisation ou les étapes de paiement

---

## WSTG-BUSL-03 — Test des contrôles d'intégrité

Le test des contrôles d'intégrité se concentre sur la vérification que l'application garantit que les données restent inchangées et fiables lorsqu'elles circulent à travers divers processus métier ou entre le client et le serveur. Les attaquants ciblent cette logique en falsifiant des paramètres critiques—tels que les prix des produits, les quantités, les privilèges des utilisateurs ou les états de transaction—au sein des requêtes HTTP, des champs cachés ou des cookies. Si l'application manque de vérification côté serveur ou de contrôles d'intégrité robustes comme le Hashing ou les HMACs, un adversaire peut manipuler ces valeurs pour commettre une fraude, élever ses propres privilèges ou contourner les contraintes métier prévues. Ce test est essentiel pour toute application gérant des transactions financières, des transitions d'état sensibles ou des workflows en plusieurs étapes où la sortie d'une étape sert d'entrée de confiance pour la suivante.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si le défaut d'intégrité permet un vol financier, une élévation de privilèges non autorisée ou la modification d'enregistrements persistants.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Les paramètres métier sensibles sont-ils exposés à une falsification côté client ?
- [ ] Non — les valeurs sensibles sont gérées exclusivement côté serveur  
- [ ] Oui — des valeurs comme le prix, la quantité ou les indicateurs d'état **sont** exposées mais chiffrées  
- [ ] Oui — les valeurs **sont** exposées en texte clair dans des champs cachés, des paramètres URL ou des cookies  

### Un mécanisme d'intégrité (par ex., HMAC, Checksum) est-il appliqué aux paramètres contrôlés par le client ?
- [ ] Oui — un contrôle d'intégrité robuste **est appliqué** et vérifié à chaque requête  
- [ ] Oui — un contrôle d'intégrité **est appliqué** mais utilise un algorithme faible/prévisible ou un secret codé en dur  
- [ ] Non — aucun contrôle d'intégrité **n'est appliqué** aux paramètres sensibles  

### Le contrôle d'intégrité peut-il être contourné ou recalculé par un attaquant ?
- [ ] Non — la vérification de la signature est strictement appliquée et le contournement **n'est pas possible**  
- [ ] Oui — la vérification de la signature peut être contournée en omettant le paramètre de signature  
- [ ] Oui — la signature **peut** être recalculée car la clé secrète ou la logique est exposée/faible  

### L'application effectue-t-elle une revalidation backend des données par rapport à une source de confiance ?
- [ ] Oui — le serveur revalide toutes les valeurs par rapport à la base de données et le contournement **n'est pas possible**  
- [ ] Oui — le serveur effectue une validation partielle mais un contournement **est possible** via des cas particuliers (edge cases)  
- [ ] Non — le serveur fait confiance aux données fournies par le client sans vérification backend *(Critique)*  

### Est-il possible de falsifier l'état d'un processus en plusieurs étapes ?
- [ ] Non — l'état de la session est géré côté serveur et ne peut pas être manipulé  
- [ ] Oui — les informations d'état sont transmises via le client et une falsification **est possible** pour sauter des étapes ou répéter des actions

---

## WSTG-BUSL-04 — Test de temporisation des processus

Les attaques par analyse temporelle des processus (Process Timing) consistent à mesurer le temps mis par une application pour traiter des requêtes spécifiques afin d'en déduire des informations sensibles sur les états internes ou l'existence de données. En analysant les écarts de temps de réponse de l'ordre de la milliseconde — souvent causés par une logique conditionnelle à sortie anticipée (early-exit), des recherches en base de données ou des comparaisons cryptographiques qui ne s'exécutent pas en temps constant — les attaquants peuvent effectuer une analyse par canal auxiliaire (side-channel analysis) pour énumérer des noms d'utilisateurs valides, vérifier des fragments de mots de passe ou confirmer l'existence d'enregistrements spécifiques. Ces vulnérabilités surviennent généralement au niveau des points de terminaison d'authentification, des formulaires de réinitialisation de mot de passe et des filtres d'API gourmands en ressources où la logique backend bifurque en fonction de la validité de l'entrée. Du point de vue de l'attaquant, ces différences de timing servent d'« oracle booléen » qui révèle des vérités sur les données backend, même lorsque l'application renvoie des messages d'erreur génériques identiques à l'utilisateur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Outils :** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### L'application présente-t-elle des temps de réponse variables pour des requêtes de ressources valides ou invalides ?
- [ ] Non — les temps de réponse sont identiques quelle que soit la validité de l'entrée  
- [ ] Oui — des différences de timing négligeables existent mais ne sont **pas** statistiquement significatives  
- [ ] Oui — des différences de timing constantes sont **observables** et fournissent un oracle fiable  

### Des fonctions de comparaison en temps constant ou des délais artificiels sont-ils implémentés pour normaliser les temps de réponse ?
- [ ] Oui — des algorithmes en temps constant sont **appliqués** à toutes les opérations de comparaison sensibles *(Le plus sécurisé)*  
- [ ] Oui — des délais artificiels ou du jitter sont **activés**, mais la logique sous-jacente ne s'exécute pas en temps constant  
- [ ] Non — aucune mesure d'atténuation du timing n'est **appliquée**, permettant une mesure directe des branchements logiques  

### Un attaquant peut-il utiliser l'analyse statistique pour isoler le signal temporel du bruit réseau ?
- [ ] Non — le jitter réseau et le bruit applicatif rendent l'analyse temporelle **impossible**  
- [ ] Oui — avec un échantillonnage suffisant, le signal temporel **peut** être isolé du bruit  
- [ ] Oui — le delta de timing est suffisamment important pour qu'une normalisation statistique ne soit **pas** requise  

### L'énumération de comptes est-elle possible via la fonctionnalité de connexion ou de réinitialisation de mot de passe ?
- [ ] Non — l'existence d'un compte ne peut pas être déterminée par le timing  
- [ ] Oui — les écarts de timing permettent à un attaquant distant d'**énumérer** des noms d'utilisateurs ou des adresses e-mail valides  

### Des opérations gourmandes en ressources (ex : traitement de fichiers, recherches complexes) divulguent-elles l'existence de données via le timing ?
- [ ] Non — la consommation de ressources est uniforme, qu'un enregistrement soit trouvé ou non  
- [ ] Oui — l'existence d'enregistrements sensibles **peut** être déduite en mesurant la durée du traitement backend

---

## WSTG-BUSL-05 — Test des limites du nombre de fois qu'une fonction peut être utilisée

Ce test évalue si une application applique des contraintes de logique métier sur la fréquence et le nombre total de fois qu'une action ou fonction spécifique peut être exécutée par un utilisateur, une session ou une adresse IP unique. Le défaut de mise en œuvre de ces limites permet à des attaquants d'abuser de fonctions à haute valeur ajoutée — telles que l'envoi de SMS OTP, le déclenchement d'e-mails de réinitialisation de mot de passe, l'application de codes de réduction ou l'exécution d'exports de données volumineux — entraînant des pertes financières, l'épuisement des ressources ou des dommages à la réputation. Ces vulnérabilités se trouvent généralement dans les points de terminaison transactionnels ou les fonctionnalités de communication et sont exploitées via des scripts automatisés qui répètent l'action bien au-delà des seuils métiers prévus. Du point de vue d'un attaquant, il s'agit d'une cible privilégiée pour le SMS pumping, le mail bombing ou le Brute Force de workflows manquant de Rate Limiting explicite ou de régulation basée sur un compteur.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si la fonction implique des transactions financières, des coûts de SMS ou impacte la disponibilité globale du système.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### L'application définit-elle et applique-t-elle des limites sur les fonctions métier sensibles ?
- [ ] Oui — les limites sont strictement appliquées et un contournement n'est **pas possible**  
- [ ] Oui — les limites sont appliquées mais sont trop élevées pour empêcher l'abus  
- [ ] Non — aucune limite n'est appliquée à l'exécution de la fonction  

### Les limites de débit (Rate Limiting) et les compteurs d'utilisation sont-ils appliqués côté serveur ?
- [ ] Oui — l'application est strictement effectuée côté serveur et **ne peut pas** être manipulée  
- [ ] Non — l'application repose sur une logique côté client (ex: JavaScript ou champs cachés)  
- [ ] Non — aucune application n'existe à quelque niveau que ce soit  

### Les limites d'utilisation peuvent-elles être contournées par manipulation d'en-tête ou de session ?
- [ ] Non — le contournement via `X-Forwarded-For`, `Origin`, ou rotation de session n'est **pas possible**  
- [ ] Oui — les limites **peuvent** être contournées en usurpant les en-têtes IP ou en supprimant les cookies  
- [ ] Oui — les limites **peuvent** être contournées en créant plusieurs comptes ou sessions  

### Le dépassement de la limite déclenche-t-il une réponse défensive appropriée ?
- [ ] Oui — l'application renvoie `429 Too Many Requests` et journalise l'événement *(Le plus sécurisé)*  
- [ ] Oui — l'application renvoie une erreur mais ne journalise **pas** l'abus potentiel  
- [ ] Non — l'application continue de traiter les requêtes mais ignore la sortie  
- [ ] Non — l'application ne fournit aucune réponse ou plante sous la charge  

### L'impact de l'abus de la fonction est-il significatif pour l'entreprise ?
- [ ] Non — l'abus de la fonction a un coût ou un impact sur les ressources négligeable  
- [ ] Oui — l'abus **peut** entraîner une consommation mineure de ressources ou une gêne pour l'utilisateur  
- [ ] Oui — l'abus **peut** entraîner des coûts financiers importants (ex: frais de SMS) ou un déni de service *(Critique)*

---

## WSTG-BUSL-06 — Test de contournement des flux de travail (Workflow Circumvention)

Le contournement de flux de travail consiste à manipuler la logique d'une application pour outrepasser les séquences ou les prérequis prévus au sein de processus à étapes multiples. Les attaquants identifient des points de terminaison (endpoints) sensibles—tels que les confirmations de paiement, les approbations administratives ou les écrans de validation MFA—et tentent d'y accéder directement, en sautant les étapes intermédiaires obligatoires. Cette vulnérabilité survient lorsque la logique côté serveur ne parvient pas à maintenir une machine d'état (state machine) robuste, s'appuyant plutôt sur l'hypothèse que l'utilisateur suit le chemin guidé par l'interface utilisateur (UI). Une exploitation réussie peut entraîner des défaillances critiques de la logique métier (Business Logic), notamment des transactions non autorisées, une élévation de privilèges (Privilege Escalation) et le contournement des mécanismes de vérification d'identité.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si le contournement permet des transactions financières non autorisées, une élévation de privilèges ou un accès administratif.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Outils :** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### L'application contient-elle des processus métier à étapes multiples ?
- [ ] Non — l'application ne consiste qu'en des actions à requête unique  
- [ ] Oui — des processus à étapes multiples (ex : paniers d'achat, formulaires d'assistant, inscription) **sont** présents  

### La séquence des étapes est-elle strictement imposée par le serveur ?
- [ ] Oui — le suivi d'état côté serveur garantit que les étapes **ne peuvent pas** être sautées  
- [ ] Oui — les contrôles côté client suggèrent une séquence, mais le serveur **ne valide pas** la progression  
- [ ] Non — l'application **n'impose aucun** ordre spécifique des opérations  

### Un attaquant peut-il atteindre l'étape finale d'un processus en demandant directement l'URL finale ou le point de terminaison ?
- [ ] Non — l'accès direct aux étapes finales **n'est pas possible** sans complétion préalable  
- [ ] Oui — les étapes finales (ex : `/api/v1/checkout/complete`) **peuvent** être atteintes par requête directe  

### L'application vérifie-t-elle que les données prérequises des étapes précédentes sont présentes et valides ?
- [ ] Oui — le serveur valide toutes les données des étapes précédentes avant de continuer  
- [ ] Non — le serveur **est** vulnérable au "saut" d'étapes où des données sont attendues mais non vérifiées  

### Les contrôles de sécurité tels que le MFA ou la vérification d'identité peuvent-ils être contournés en manipulant le flux de travail ?
- [ ] Non — les contrôles critiques **ne peuvent pas** être contournés via une manipulation de flux  
- [ ] Oui — le contournement du MFA ou des vérifications d'identité **est possible** en sautant directement à l'étape post-vérification

---

## WSTG-BUSL-07 — Tester les défenses contre l'utilisation abusive de l'application

Les défenses contre l'utilisation abusive de l'application évaluent la capacité de l'application à identifier, journaliser (log) et répondre aux comportements anormaux des utilisateurs qui s'écartent de la logique métier (Business Logic) prévue. Contrairement à la sécurité traditionnelle basée sur les signatures, ces défenses se concentrent sur la détection de modèles tels que les requêtes en rafale (rapid-fire requests), le Credential Stuffing, ou l'énumération séquentielle de ressources qui signalent un abus automatisé. Du point de vue d'un attaquant, ce test détermine la résilience de l'application contre l'automatisation et les attaques « low and slow » conçues pour exfiltrer des données ou épuiser les ressources sans déclencher les alertes de sécurité standard. L'absence de mise en œuvre de ces défenses entraîne souvent du scraping massif de données, des prises de contrôle de comptes (Account Takeover) ou des dénis de service (DoS) via des fonctionnalités applicatives légitimes mais gourmandes en ressources.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Des mécanismes de Rate Limiting ou de throttling sont-ils appliqués sur les points de terminaison (endpoints) sensibles ?
- [ ] Oui — un Rate Limiting strict **est appliqué** et imposé sur tous les points de terminaison sensibles *(Le plus sécurisé)*  
- [ ] Oui — le Rate Limiting **est appliqué** mais peut être contourné via la rotation d'adresses IP ou la manipulation d'en-têtes (headers)  
- [ ] Oui — le Rate Limiting **est appliqué** uniquement aux points de terminaison d'authentification, laissant les autres exposés  
- [ ] Non — aucun Rate Limiting ou throttling **n'est appliqué** aux fonctionnalités de l'application  

### L'application détecte-t-elle et bloque-t-elle les modèles d'interaction automatisés ?
- [ ] Oui — l'analyse comportementale ou les CAPTCHAs **sont activés** et bloquent efficacement les bots  
- [ ] Oui — l'anti-automatisation **est activée** mais un contournement **est possible** en utilisant des navigateurs sans interface (headless browsers) ou la rotation de sessions  
- [ ] Non — l'application **ne peut pas** distinguer un utilisateur humain d'un script automatisé  

### Les comportements anormaux sont-ils journalisés et suivis d'une réponse défensive ?
- [ ] Oui — l'utilisation abusive déclenche un blocage automatisé et alerte l'équipe de sécurité  
- [ ] Oui — l'utilisation abusive est journalisée pour l'audit mais **aucune** action défensive automatisée n'est prise  
- [ ] Non — l'application **ne journalise pas** et ne répond pas aux modèles d'activité inhabituels  

### Les défenses peuvent-elles être contournées en manipulant les identifiants côté client ou les en-têtes (headers) ?
- [ ] Non — les défenses reposent sur l'état côté serveur et une télémétrie vérifiée  
- [ ] Oui — les contrôles **peuvent** être contournés en effaçant les cookies ou en effectuant une rotation des chaînes `User-Agent`  
- [ ] Oui — les contrôles **peuvent** être contournés en injectant des en-têtes tels que `X-Forwarded-For` ou `X-Real-IP`

---

## WSTG-BUSL-08 — Test de téléchargement de types de fichiers inattendus

Le téléchargement de types de fichiers inattendus permet aux attaquants de contourner les contraintes de la logique métier en soumettant des fichiers que l'application n'a pas l'intention de traiter, ce qui peut potentiellement mener à une exécution de code à distance (Remote Code Execution), à du Cross-Site Scripting (XSS) ou à un déni de service (Denial of Service). Les attaquants sondent ces vulnérabilités en modifiant les extensions de fichiers, les types MIME et les Magic Bytes pour tromper la logique de validation côté serveur et l'inciter à accepter du contenu malveillant. Cela se produit généralement lors du téléchargement de photos de profil, dans les systèmes de gestion de documents ou les pièces jointes de tickets de support où la validation côté back-end est insuffisante. Une exploitation réussie peut compromettre le serveur hôte si le fichier téléchargé est exécuté ou mener à des attaques côté client si le fichier est renvoyé à d'autres utilisateurs avec un Content-Type incorrect.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Outils :** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### L'application restreint-elle le téléchargement de fichiers à un ensemble spécifique d'extensions ?
- [ ] Oui — une liste blanche (Whitelist) stricte d'extensions est appliquée et un contournement n'est **pas possible**  
- [ ] Oui — une liste blanche d'extensions est présente mais un contournement **est possible** via des doubles extensions ou des octets nuls (Null Bytes)  
- [ ] Non — n'importe quelle extension de fichier **peut** être téléchargée  

### Le contenu du fichier est-il validé au-delà de l'extension du fichier ?
- [ ] Oui — la logique côté serveur vérifie les Magic Bytes et effectue une inspection approfondie du contenu  
- [ ] Oui — la logique côté serveur vérifie le type MIME via l'en-tête `Content-Type` uniquement  
- [ ] Non — aucune validation de contenu n'est **appliquée** après la vérification de l'extension  

### Un attaquant peut-il exécuter du code en téléchargeant un Web Shell ou un script ?
- [ ] Non — les fichiers téléchargés sont stockés dans un répertoire non exécutable ou un compartiment de stockage (S3/Azure Blob)  
- [ ] Oui — les fichiers sont stockés dans un répertoire accessible par le Web, mais l'exécution **est désactivée** via la configuration du serveur  
- [ ] Oui — les scripts malveillants téléchargés **peuvent** être exécutés directement via un chemin URL prévisible *(Critique)*  

### Des attaques côté client comme le XSS sont-elles possibles via les types de fichiers téléchargés ?
- [ ] Non — les fichiers sont servis avec `Content-Disposition: attachment` et `X-Content-Type-Options: nosniff`  
- [ ] Oui — des fichiers SVG ou HTML **peuvent** être téléchargés et rendus dans le navigateur, menant à un XSS  
- [ ] Oui — les métadonnées d'image (EXIF) **sont traitées** et reflétées dans l'interface utilisateur sans assainissement (Sanitization)

---

## WSTG-BUSL-09 — Test de téléchargement de fichiers malveillants (Upload of Malicious Files)

La fonctionnalité de téléchargement de fichiers permet aux utilisateurs de soumettre des données au serveur, offrant ainsi un vecteur direct pour l'introduction de contenu malveillant dans l'environnement de l'application. Les attaquants exploitent des logiques de validation faibles pour télécharger des Web Shells, des malwares ou des fichiers spécialement conçus — tels que SVG ou HTML — afin de réaliser une exécution de code à distance (Remote Code Execution (RCE)) ou du Cross-Site Scripting (XSS). Cette vulnérabilité se retrouve généralement dans des fonctionnalités telles que les paramètres de profil, les systèmes de gestion documentaire et les gestionnaires de pièces jointes, où le serveur ne parvient pas à vérifier correctement les types de fichiers, leur contenu et les permissions d'exécution. Une exploitation réussie conduit souvent à une compromission totale du système, à l'exfiltration de données ou à l'utilisation du serveur comme point de pivot pour des attaques sur le réseau interne.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **État du test** | Non effectué |
| **Sévérité** | Haute / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Outils :** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### L'application propose-t-elle une fonctionnalité de téléchargement de fichiers ?
- [ ] Non — aucune fonctionnalité de téléchargement de fichiers n'existe  
- [ ] Oui — la fonctionnalité existe pour les utilisateurs authentifiés  
- [ ] Oui — la fonctionnalité est disponible pour les utilisateurs **non authentifiés** *(Critique)*  

### La validation de l'extension de fichier est-elle appliquée côté serveur ?
- [ ] Oui — une allowlist (liste d'autorisation) stricte d'extensions est **appliquée** et le contournement est **impossible**  
- [ ] Oui — une allowlist est utilisée mais un contournement **est possible** via la sensibilité à la casse ou les doubles extensions  
- [ ] Oui — une blocklist (liste de blocage) est utilisée, laquelle **peut** être contournée avec des extensions alternatives (ex : `.phtml`, `.asa`)  
- [ ] Non — aucune validation d'extension n'est **appliquée** côté serveur  

### Le contenu du fichier est-il validé au-delà de l'extension ou du type MIME ?
- [ ] Oui — l'application vérifie les magic bytes et effectue une inspection approfondie du contenu  
- [ ] Oui — l'application vérifie uniquement l'en-tête `Content-Type`, qui **est** facilement falsifiable  
- [ ] Non — l'application se repose entièrement sur l'extension du fichier pour la validation  

### Les fichiers téléchargés sont-ils stockés dans un répertoire disposant de permissions d'exécution ?
- [ ] Non — les fichiers sont stockés sur un service de stockage dédié (ex : S3) ou dans un répertoire non exécutable  
- [ ] Oui — les fichiers sont stockés sur le serveur web mais l'exécution est **désactivée** via la configuration  
- [ ] Oui — les fichiers sont stockés dans un répertoire accessible par le web et l'exécution **est activée** *(Critique)*  

### Les filtres de sécurité peuvent-ils être contournés via la manipulation du nom de fichier ?
- [ ] Non — les noms de fichiers sont assainis (sanitized) ou régénérés aléatoirement par le serveur  
- [ ] Oui — les filtres **peuvent** être contournés en utilisant des null bytes (ex : `shell.php%00.jpg`)  
- [ ] Oui — des caractères de Path Traversal (ex : `../../`) **peuvent** être utilisés pour télécharger des fichiers dans des répertoires arbitraires

---

## WSTG-BUSL-10 — Test des fonctionnalités de paiement

Le test des fonctionnalités de paiement consiste à identifier les failles de logique métier qui permettent à un attaquant de contourner les passerelles de paiement, de manipuler les totaux des commandes ou d'usurper des signaux de transaction réussie. Ces vulnérabilités résident généralement dans la communication entre l'application, le navigateur client et les processeurs de paiement tiers tels que Stripe, PayPal, ou les systèmes de comptabilité internes. Un attaquant peut tenter de modifier les paramètres de prix en transit, de soumettre des quantités négatives pour réduire le coût total, ou de rejouer les rappels (callbacks) de transaction réussie pour honorer des commandes sans paiement réel. Une exploitation réussie entraîne une perte financière directe, des écarts d'inventaire et un compromis potentiel de l'intégrité du e-commerce de l'application.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Le montant de la transaction est-il validé côté serveur avant le traitement ?
- [ ] Oui — le montant est strictement validé par rapport à la base de données produit côté serveur *(Le plus sécurisé)*  
- [ ] Oui — le montant est validé mais un contournement de la logique **est possible** via la pollution de paramètres (parameter pollution) ou le changement de devise  
- [ ] Non — le montant de la transaction **peut** être modifié dans la requête côté client et est accepté par le backend  

### La quantité d'articles peut-elle être manipulée pour aboutir à un total nul ou négatif ?
- [ ] Non — les quantités négatives ou nulles sont rejetées par la logique de validation de l'application  
- [ ] Oui — les quantités négatives sont acceptées dans le panier mais le total est corrigé lors du paiement  
- [ ] Oui — des quantités négatives **peuvent** être utilisées pour réduire le montant total de la commande ou obtenir un crédit *(Critique)*  

### L'application gère-t-elle de manière sécurisée les rappels (callbacks) ou les webhooks des passerelles de paiement ?
- [ ] Oui — les rappels nécessitent une signature cryptographique valide qui **ne peut pas** être usurpée  
- [ ] Oui — les signatures sont vérifiées mais le secret est faible ou la vérification **peut** être contournée  
- [ ] Non — l'application s'appuie sur des rappels non authentifiés ou non vérifiés pour confirmer le statut du paiement  

### L'application est-elle vulnérable aux conditions de concurrence (race conditions) pendant la phase de paiement ou de validation de commande ?
- [ ] Non — l'intégrité transactionnelle et les mécanismes de verrouillage de base de données sont **activés**  
- [ ] Oui — des conditions de concurrence **sont possibles** mais n'impactent pas le prix final ou l'exécution de la commande  
- [ ] Oui — des requêtes concurrentes **peuvent** mener à plusieurs exécutions de commande pour un seul paiement ou à une "double dépense" de cartes-cadeaux  

### Les codes de réduction ou les points de fidélité sont-ils sujets à manipulation ou réutilisation ?
- [ ] Non — les codes sont validés pour un usage unique et les contraintes logiques sont **appliquées**  
- [ ] Oui — les codes peuvent être réutilisés ou appliqués à des articles non éligibles, mais l'impact est limité  
- [ ] Oui — les attaquants **peuvent** appliquer plusieurs codes conflictuels ou contourner les limites d'expiration et d'utilisation

---

## WSTG-CLNT-01 — Test du Cross-Site Scripting basé sur le DOM

Le Cross-Site Scripting basé sur le DOM (DOM XSS) se produit lorsqu'une application contient du JavaScript côté client qui traite des données provenant d'une source non fiable de manière non sécurisée, généralement en réinjectant les données dans le Document Object Model (DOM) via un sink dangereux. Contrairement au XSS réfléchi ou stocké, la vulnérabilité réside entièrement dans le code côté client ; le Payload n'est souvent pas envoyé au serveur, car il est traité par des sinks tels que `innerHTML`, `eval()`, ou `document.write()`. Les attaquants exploitent cela en créant des URL avec des fragments ou des paramètres malveillants qui, lorsqu'ils sont chargés par le navigateur d'une victime, exécutent du JavaScript arbitraire dans le contexte de la session de l'utilisateur. Cela peut mener à l'exfiltration de jetons de session (Session Tokens), au vol de données sensibles et à des actions non autorisées effectuées au nom de l'utilisateur authentifié.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Outils :** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Des sources non fiables sont-elles utilisées pour capturer des données contrôlées par l'utilisateur en JavaScript ?
- [ ] Non — JavaScript n'utilise pas `location.hash`, `location.search`, ou `document.referrer`  
- [ ] Oui — des sources non fiables sont utilisées mais les données ne sont transmises à aucun sink d'exécution  
- [ ] Oui — des sources non fiables sont utilisées et les données circulent directement dans la logique côté client  

### Les données contrôlées par l'utilisateur sont-elles transmises à des sinks DOM dangereux ?
- [ ] Non — les sinks tels que `innerHTML`, `outerHTML`, `document.write()`, ou `eval()` ne sont pas utilisés  
- [ ] Oui — des sinks dangereux sont présents mais les données sont strictement assainies (Sanitized) à l'aide d'une bibliothèque sécurisée comme `DOMPurify`  
- [ ] Oui — des sinks dangereux sont présents et les données sont traitées avec un filtrage par expressions régulières (Regex) insuffisant ou personnalisé  
- [ ] Oui — des sinks dangereux sont présents et les données sont transmises sans aucun assainissement ni encodage *(Critique)*  

### Le sink d'exécution peut-il être atteint via un Payload basé sur une URL ?
- [ ] Non — le flux de la source vers le sink ne peut pas être déclenché via une entrée externe  
- [ ] Oui — le flux est possible mais nécessite une interaction complexe de l'utilisateur  
- [ ] Oui — le flux est possible et peut être déclenché via un simple fragment d'URL ou un paramètre de requête (Query Parameter)  

### Des en-têtes de sécurité modernes ou des mesures d'atténuation basées sur le navigateur neutralisent-ils le risque ?
- [ ] Oui — une `Content-Security-Policy` (CSP) stricte avec `script-src` et `trusted-types` est activée et empêche l'exécution  
- [ ] Oui — une CSP est présente mais `unsafe-inline` ou des hachages (Hashes) faibles rendent un contournement (Bypass) possible  
- [ ] Non — aucune CSP n'est présente pour atténuer l'exécution basée sur le DOM  

### L'application utilise-t-elle des frameworks côté client avec des protections intégrées ?
- [ ] Oui — le framework (ex: Angular, React) encode automatiquement les données et aucun contournement n'est possible  
- [ ] Oui — le framework est utilisé mais des "escape hatches" comme `dangerouslySetInnerHTML` sont activés  
- [ ] Non — l'application utilise du JavaScript natif (Vanilla JS) ou des bibliothèques obsolètes (ex: anciennes versions de jQuery) sans auto-échappement (Auto-escaping)

---

## WSTG-CLNT-02 — Test de l'exécution de JavaScript

Le test d'exécution JavaScript se concentre sur l'identification des cas où une application traite de manière incorrecte les données fournies par l'utilisateur, entraînant l'exécution de scripts non autorisés dans le contexte du navigateur de la victime. Cette vulnérabilité se traduit généralement par un Cross-Site Scripting (XSS), qui permet aux attaquants d'exfiltrer des cookies de session, de manipuler le Document Object Model (DOM) ou d'effectuer des actions au nom de l'utilisateur. Les testeurs d'intrusion examinent les points de sortie (sinks) tels que `innerHTML`, `document.write()` et `eval()` où des données non assainies provenant de sources telles que les fragments d'URL, le `localStorage` ou `window.name` peuvent être traitées. Une exploitation réussie compromet l'intégrité et la confidentialité de la session de l'utilisateur et peut servir de pivot pour des attaques côté client plus complexes.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Outils :** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### L'application traite-t-elle des données fournies par l'utilisateur dans des points de sortie (sinks) JavaScript dangereux ?
- [ ] Non — toutes les données sont assainies ou utilisées dans des points de sortie sécurisés comme `textContent`  
- [ ] Oui — les données sont traitées dans des points de sortie dangereux mais l'assainissement ou l'encodage **est appliqué**  
- [ ] Oui — les données sont traitées dans des points de sortie dangereux et l'assainissement **n'est pas appliqué**  

### Une Content Security Policy (CSP) est-elle présente pour atténuer l'exécution de scripts ?
- [ ] Oui — une CSP restrictive est **activée** et aucun contournement **n'est possible**  
- [ ] Oui — une CSP est **activée** mais un contournement **est possible** via `unsafe-inline` ou des CDN non sécurisés  
- [ ] Non — aucune CSP n'est **activée**  

### Le JavaScript peut-il être exécuté via des paramètres d'URL ou des fragments (DOM-based XSS) ?
- [ ] Non — l'entrée est correctement encodée et l'exécution **n'est pas possible**  
- [ ] Oui — l'entrée est reflétée dans le DOM mais l'exécution **est empêchée** par les protections des frameworks modernes  
- [ ] Oui — l'entrée est directement exécutée dans le navigateur et l'exploitation **est possible**  

### Les gestionnaires d'événements (event handlers) ou les schémas URI sont-ils vulnérables à l'injection de scripts ?
- [ ] Non — l'entrée est assainie pour supprimer les attributs d'événement et les schémas `javascript:`  
- [ ] Oui — les gestionnaires d'événements **peuvent** être injectés mais des filtres limitent la complexité du payload  
- [ ] Oui — l'exécution de JavaScript arbitraire via les attributs `onerror`, `onload` ou `href` **est possible**

---

## WSTG-CLNT-03 — Injection HTML

L'Injection HTML (HTML Injection) se produit lorsqu'une application ne parvient pas à assainir les entrées fournies par l'utilisateur avant de les restituer dans le navigateur, permettant à un attaquant d'injecter des balises HTML arbitraires dans le Document Object Model (DOM) de la page web. Bien qu'elle soit similaire au Cross-Site Scripting (XSS), l'objectif principal de l'Injection HTML est de manipuler la présentation visuelle de la page ou de faciliter des attaques d'hameçonnage (Phishing) en injectant des formulaires malveillants et du contenu trompeur. Les attaquants ciblent les paramètres qui sont réfléchis dans l'UI (User Interface), tels que les termes de recherche, les champs de profil utilisateur ou les messages d'erreur, pour inciter les victimes à divulguer des identifiants sensibles ou à interagir avec des liens tiers non autorisés. Cette vulnérabilité représente un risque significatif pour l'intégrité de l'interface utilisateur de l'application et peut être le précurseur d'attaques d'ingénierie sociale (Social Engineering) ou de sessions plus complexes.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Statut du Test** | Non effectué |
| **Gravité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Les entrées contrôlées par l'utilisateur sont-elles réfléchies dans le DOM sans un encodage HTML approprié ?
- [ ] Non — toutes les entrées réfléchies sont strictement encodées en HTML avant d'être rendues  
- [ ] Oui — certains caractères **sont** encodés mais des contournements (bypasses) pour certaines balises **sont possibles**  
- [ ] Oui — l'entrée est réfléchie brute, et l'injection HTML **est possible**  

### Des éléments HTML structurels peuvent-ils être injectés pour modifier la mise en page ?
- [ ] Non — l'application filtre ou supprime les balises structurelles comme `<div>`, `<table>`, ou `<iframe>`  
- [ ] Oui — des éléments structurels **peuvent** être injectés mais n'ont **pas** d'impact significatif sur l'UI  
- [ ] Oui — une défiguration (defacement) significative de l'UI **est possible** via les balises injectées  

### Est-il possible d'injecter des éléments d'hameçonnage fonctionnels comme des formulaires ou des liens malveillants ?
- [ ] Non — les balises `<form>`, `<input>`, et `<a>` sont efficacement bloquées ou assainies  
- [ ] Oui — des liens malveillants **peuvent** être injectés pour rediriger les utilisateurs vers des sites externes  
- [ ] Oui — des éléments `<form>` fonctionnels **peuvent** être injectés pour collecter des identifiants *(Moyenne)*  

### Les en-têtes de sécurité ou les Content Security Policies (CSP) atténuent-ils l'impact de l'injection ?
- [ ] Oui — une CSP restrictive est en place et `form-action` ou `frame-src` **est appliqué**  
- [ ] No — des en-têtes de sécurité sont présents mais la CSP **est désactivée** ou contient des directives unsafe-inline/wildcards  
- [ ] No — aucune CSP ou en-tête de sécurité pertinent **n'est activé**

---

## WSTG-CLNT-04 — Test de redirection d'URL côté client (Client-Side URL Redirect)

Une redirection d'URL côté client se produit lorsqu'une application web accepte une entrée contrôlée par l'utilisateur—généralement via des paramètres d'URL ou des fragments de hachage—et l'utilise au sein d'un récepteur (sink) de redirection côté client tel que `window.location` ou `document.location.href`. Cette vulnérabilité est principalement exploitée par des attaquants pour mener des campagnes de phishing sophistiquées, car la victime fait initialement confiance au domaine légitime avant d'être redirigée silencieusement vers un site malveillant. Au-delà du phishing, les redirections côté client peuvent souvent être combinées pour exfiltrer des données sensibles telles que des jetons d'accès OAuth (OAuth access tokens) ou des identifiants de session si la redirection se produit alors que ces valeurs sont présentes dans l'URL ou dans l'en-tête `Referer`. Dans les Single Page Applications (SPA) modernes, cette vulnérabilité apparaît fréquemment dans la logique de routage, les paramètres "return to" après l'authentification ou les fonctionnalités de changement de langue.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **État du Test** | Non effectué |
| **Sévérité** | Moyenne* |

> *La sévérité devient Élevée si la redirection peut être utilisée pour exfiltrer des jetons OAuth, des identifiants de session, ou pour contourner des contrôles de sécurité dans un flux d'authentification.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Outils :** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### L'application utilise-t-elle des scripts côté client pour gérer les redirections basées sur les entrées utilisateur ?
- [ ] Non — la redirection est gérée exclusivement côté serveur via des codes d'état HTTP `3xx`  
- [ ] Oui — l'application utilise des récepteurs (sinks) JavaScript tels que `window.location` pour naviguer en fonction des paramètres d'URL  
- [ ] Oui — l'application utilise un routeur de framework côté client (ex: React Router, Vue Router) qui traite des chemins fournis par l'utilisateur  

### Des contrôles de validation d'entrée ou de liste blanche (whitelisting) sont-ils implémentés pour l'URL de destination ?
- [ ] Oui — une **liste blanche** (whitelist) stricte de domaines/chemins autorisés est appliquée et le contournement (bypass) n'est **pas possible**  
- [ ] Oui — une validation est présente mais repose sur des regex faibles ou des listes noires (blacklists), et le contournement **est possible**  
- [ ] Non — l'application accepte n'importe quelle chaîne arbitraire comme cible de redirection  

### La logique de redirection peut-elle être contournée en utilisant des techniques d'offuscation courantes ?
- [ ] Non — les contrôles gèrent correctement les caractères encodés, les URL relatives au protocole (`//`) et les barres obliques inverses (backslashes)  
- [ ] Oui — le contournement est **possible** via l'encodage URL ou le double-encodage  
- [ ] Oui — le contournement est **possible** en utilisant des URL relatives au protocole ou des caractères `@` (ex: `https://expected.com@attacker.com`)  
- [ ] Oui — le contournement est **possible** en exploitant des comportements spécifiques au navigateur ou une injection d'octet nul (null byte injection)  

### Le processus de redirection expose-t-il des données sensibles au site de destination ?
- [ ] Non — aucune donnée sensible n'est présente dans l'URL ni transmise via les en-têtes pendant la redirection  
- [ ] Oui — des données sensibles (ex: jetons de session, clés API) **sont divulguées** via l'en-tête `Referer` au site externe  
- [ ] Oui — les données sensibles présentes dans le fragment d'URL (`#`) **sont accessibles** aux scripts du site de destination  

### Est-il possible d'exécuter du JavaScript via le récepteur de redirection (DOM-based XSS) ?
- [ ] Non — le récepteur (sink) autorise uniquement la navigation vers les protocoles `http` ou `https`  
- [ ] Oui — le récepteur de redirection autorise les URI `javascript:` ou `data:`, rendant une vulnérabilité Cross-Site Scripting (XSS) basée sur le DOM **possible**

---

## WSTG-CLNT-05 — Test d'Injection CSS

L'Injection CSS (CSS Injection) se produit lorsqu'une application permet à une entrée fournie par l'utilisateur d'influencer les feuilles de style en cascade (CSS) d'une page web sans assainissement ou échappement approprié. Bien que souvent perçue comme un problème cosmétique, les attaquants peuvent exploiter l'injection CSS pour exfiltrer des données sensibles, telles que des jetons CSRF ou des identifiants de session, en utilisant des sélecteurs d'attributs et des propriétés de type background-image pour déclencher des requêtes externes. Cette vulnérabilité survient généralement dans les points de terminaison où les préférences de l'utilisateur (par exemple, thèmes personnalisés, couleurs de police) sont reflétées à l'intérieur de blocs `<style>` ou d'attributs `style` en ligne. Du point de vue d'un attaquant, une exploitation réussie peut mener à un détournement d'interface utilisateur (UI Redressing), à du phishing par modification non autorisée de la mise en page, ou à une exfiltration furtive de données dans des environnements où des politiques de sécurité de contenu (CSP) strictes pourraient autrement bloquer les attaques basées sur JavaScript.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si le point d'injection permet l'exfiltration d'attributs sensibles comme des jetons CSRF ou s'il peut être utilisé pour du UI Redressing dans des flux de travail sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### L'application reflète-t-elle les entrées contrôlées par l'utilisateur dans les contextes CSS ?
- [ ] Non — l'entrée n'est **pas** reflétée dans les balises de style, les attributs ou les fichiers CSS  
- [ ] Oui — l'entrée est reflétée mais strictement limitée à des valeurs alphanumériques sûres  
- [ ] Oui — l'entrée est reflétée à l'intérieur d'un attribut `style` ou d'un bloc `<style>`  

### Les métacaractères et fonctions spécifiques au CSS sont-ils correctement assainis ?
- [ ] Oui — les caractères tels que `{`, `}`, `:` et les fonctions comme `url()` sont **correctement échappés**  
- [ ] Oui — l'assainissement est en place mais un contournement **est possible** via l'encodage ou les commentaires CSS  
- [ ] Non — aucun assainissement n'est appliqué aux métacaractères CSS  

### L'exfiltration de données est-elle possible en utilisant des sélecteurs CSS ?
- [ ] Non — les sélecteurs **ne peuvent pas** déclencher de requêtes externes ou fuiter des données  
- [ ] Oui — une exfiltration partielle de données **est possible** via les sélecteurs d'attributs et `background-image`  
- [ ] Oui — une exfiltration complète de jetons sensibles **est possible** en utilisant des techniques automatisées de Brute Force  

### Une politique de sécurité de contenu (CSP) atténue-t-elle le risque d'injection CSS ?
- [ ] Oui — `style-src` est restreint à 'self' et `img-src` ou `connect-src` **est restreint**  
- [ ] Oui — une CSP est présente mais utilise 'unsafe-inline' ou autorise des domaines externes génériques  
- [ ] Non — aucune CSP n'est implémentée pour empêcher le chargement de ressources externes via CSS  

### La vulnérabilité peut-elle être utilisée pour effectuer du UI Redressing ou du Phishing ?
- [ ] Non — la portée de l'injection est trop limitée pour modifier la mise en page de manière significative  
- [ ] Oui — la modification de l'interface utilisateur (UI) **est possible**, permettant la superposition d'éléments malveillants  
- [ ] Oui — un contrôle total de la mise en page est **possible**, facilitant des attaques de phishing hautement convaincantes

---

## WSTG-CLNT-06 — Test de manipulation de ressources côté client (Client-Side Resource Manipulation)

La manipulation de ressources côté client survient lorsqu'une application permet à une entrée contrôlée par l'utilisateur d'influencer la source ou le chemin des ressources chargées par le navigateur, telles que des scripts, des feuilles de style ou des iframes. En manipulant des fragments d'URI, des paramètres de requête (query parameters) ou d'autres sources DOM, un attaquant peut contraindre l'application à charger du contenu externe malveillant ou à exécuter du code non autorisé. Cette vulnérabilité se trouve généralement dans la logique JavaScript qui alimente dynamiquement les attributs `src` ou `href` d'éléments HTML sans validation suffisante par rapport à une liste d'autorisation (allow-list) de confiance. Du point de vue d'un attaquant, cela est exploité en forgeant une URL qui redirige le chargement de ressources vers un serveur contrôlé par l'attaquant, ce qui peut entraîner un Cross-Site Scripting (XSS) basé sur le DOM, une exfiltration de données sensibles ou un détournement d'interface utilisateur (UI redressing).


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Outils :** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### L'application utilise-t-elle des récepteurs (sinks) côté client pour charger ou intégrer dynamiquement des ressources ?
- [ ] Non — les ressources sont chargées uniquement via du HTML statique ou une logique côté serveur  
- [ ] Oui — le JavaScript côté client alimente dynamiquement des attributs de ressources tels que `src`, `href` ou `data`  

### Des contrôles de validation ou des listes d'autorisation (allow-lists) sont-ils implémentés pour les sources d'URI des ressources ?
- [ ] Oui — des listes d'autorisation strictes et une validation d'origine **sont appliquées** à toutes les ressources dynamiques  
- [ ] Oui — une validation est présente mais repose sur des listes d'exclusion (blacklists) faibles ou des regex facilement contournables  
- [ ] Non — aucun contrôle de validation n'est appliqué aux chemins de ressources contrôlés par l'utilisateur  

### Est-il possible de contourner la validation d'URI en utilisant des protocoles alternatifs ou du codage ?
- [ ] Non — le contournement des filtres de ressources n'est **pas possible**  
- [ ] Oui — le contournement **est possible** en utilisant différents protocoles tels que `data:`, `vbscript:` ou `javascript:`  
- [ ] Oui — le contournement **est possible** en utilisant des caractères encodés ou des octets nuls (null bytes) pour contourner les correspondances de chaînes simples  

### Un attaquant peut-il rediriger le chargement de ressources vers un domaine externe non fiable ?
- [ ] Non — les chemins de ressources sont restreints à la même origine ou à un sous-répertoire fixe  
- [ ] Oui — l'application **peut** être contrainte de charger des scripts ou des styles à partir d'un domaine externe arbitraire  

### Quel est l'impact maximal de la manipulation de ressources ?
- [ ] Faible — la manipulation n'affecte que des éléments non exécutables comme des images ou du texte non sensible  
- [ ] Moyen — la manipulation permet l'injection de CSS ou un détournement d'interface (UI redressing) significatif  
- [ ] Élevé — la manipulation conduit à un XSS basé sur le DOM ou à l'exécution de JavaScript arbitraire *(Critique)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Le Cross-Origin Resource Sharing (CORS) est un mécanisme basé sur le navigateur qui permet à un serveur d'autoriser explicitement l'accès à ses ressources à partir de différentes origines, assouplissant ainsi la Same-Origin Policy (SOP). Des mauvaises configurations (misconfigurations) surviennent lorsqu'une application fait une confiance excessive à des origines arbitraires, souvent en reflétant l'en-tête `Origin` dans l'en-tête de réponse `Access-Control-Allow-Origin` ou en utilisant des listes blanches (whitelists) par expressions régulières (regex) mal implémentées. Du point de vue d'un attaquant, une politique CORS permissive peut être exploitée en hébergeant un script malveillant sur un domaine contrôlé qui effectue des requêtes authentifiées vers l'application vulnérable, permettant à l'attaquant d'exfiltrer des données sensibles, telles que des jetons CSRF (CSRF tokens) ou des PII, directement depuis le corps de la réponse (response body). Cette vulnérabilité est particulièrement critique sur les points de terminaison d'API (API endpoints) qui traitent des sessions authentifiées et renvoient des informations non publiques.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Outils :** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### L'application implémente-t-elle des en-têtes CORS sur les points de terminaison authentifiés ?
- [ ] Non — le CORS n'est **pas activé** ; le navigateur utilise par défaut la Same-Origin Policy stricte  
- [ ] Oui — les en-têtes CORS sont présents et restreints à une **whitelist** stricte  
- [ ] Oui — les en-têtes CORS sont présents mais configurés de manière **permissive**  

### Comment le serveur gère-t-il l'en-tête `Access-Control-Allow-Origin` (ACAO) ?
- [ ] L'ACAO est défini sur un domaine statique spécifique et **de confiance**  
- [ ] L'ACAO est défini sur `*` (wildcard) et les identifiants (credentials) **ne peuvent pas** être envoyés  
- [ ] L'ACAO est défini sur `*` (wildcard) et les identifiants (credentials) **peuvent** être envoyés *(Note : La plupart des navigateurs bloquent cette configuration)*  
- [ ] L'ACAO **reflète dynamiquement** la valeur de l'en-tête `Origin` provenant de la requête  

### L'en-tête `Access-Control-Allow-Credentials` (ACAC) est-il défini sur `true` ?
- [ ] Non — l'ACAC n'est **pas présent** ou est défini sur `false`  
- [ ] Oui — l'ACAC est défini sur `true`, mais l'ACAO est **restreint** à des origines de confiance  
- [ ] Oui — l'ACAC est défini sur `true` et l'ACAO **reflète** des origines arbitraires *(Critique)*  

### La whitelist d'origines peut-elle être contournée via une manipulation de sous-domaine ou de l'origine null ?
- [ ] Non — la whitelist est strictement appliquée et **ne peut pas** être contournée  
- [ ] Oui — la whitelist accepte **n'importe quel** sous-domaine de la cible (ex: `attacker.target.com`)  
- [ ] Oui — la whitelist accepte l'origine `null` via des iframes sandboxées  
- [ ] Oui — la whitelist est vulnérable aux contournements par regex (ex: `target.com.attacker.com` ou `target.com-safe.com`)  

### La configuration CORS permet-elle l'exfiltration de données sensibles ?
- [ ] Non — les points de terminaison exposés ne contiennent **pas** de données sensibles ou spécifiques à la session  
- [ ] Oui — les points de terminaison authentifiés renvoient des PII, des identifiants (credentials) ou des jetons CSRF (CSRF tokens) qui **peuvent** être lus par une origine non autorisée

---

## WSTG-CLNT-08 — Test de Cross Site Flashing

Le Cross-Site Flashing (XSF) est une vulnérabilité côté client qui survient lorsqu'un fichier Flash (SWF) gère de manière incorrecte des entrées contrôlées par l'utilisateur, permettant à un attaquant d'exécuter du code ActionScript malveillant ou de créer un pont vers l'environnement JavaScript du navigateur. En manipulant des paramètres tels que `FlashVars` ou des chaînes de requête URL transmises à des puits (sinks) comme `ExternalInterface.call`, `getURL` ou `loadMovie`, un attaquant peut effectuer des actions dans le contexte du domaine vulnérable, incluant le détournement de session (session hijacking) et l'exfiltration de données. Cette vulnérabilité se retrouve principalement dans les applications d'entreprise héritées (legacy) qui hébergent encore des fichiers SWF, où une validation insuffisante des entrées permet au film Flash d'être détourné comme vecteur de Cross-Site Scripting (XSS) ou pour contourner la Same-Origin Policy (SOP) via des configurations cross-domain permissives.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Des fichiers Adobe Flash (.swf) hérités sont-ils hébergés sur l'application ?
- [ ] Non — aucun fichier Flash n'est hébergé ou référencé dans le périmètre de l'application  
- [ ] Oui — des fichiers SWF sont présents mais ne servent que du contenu statique  
- [ ] Oui — des fichiers SWF sont présents et acceptent des paramètres dynamiques via `FlashVars` ou des chaînes de requête URL  

### Les entrées transmises aux puits (sinks) ActionScript sont-elles assainies ?
- [ ] Oui — toutes les entrées sont strictement validées et les puits ActionScript **ne peuvent pas** être manipulés  
- [ ] Oui — une validation est appliquée mais un contournement (bypass) **est possible** via des techniques d'encodage spécifiques  
- [ ] Non — l'entrée est transmise directement à des puits sensibles comme `ExternalInterface.call` ou `getURL` *(Critique)*  

### Le fichier Flash peut-il être utilisé pour exécuter du JavaScript arbitraire (XSS) ?
- [ ] Non — `ExternalInterface` est **désactivé** ou le paramètre `allowScriptAccess` est défini sur `never`  
- [ ] Oui — `allowScriptAccess` est défini sur `sameDomain` mais le SWF est hébergé sur le domaine cible  
- [ ] Oui — `allowScriptAccess` est défini sur `always`, permettant un XSS depuis n'importe quel domaine  

### La politique `crossdomain.xml` empêche-t-elle les requêtes cross-origin non autorisées ?
- [ ] Oui — `crossdomain.xml` est restrictif et n'autorise que des origines spécifiques et de confiance  
- [ ] Non — le fichier `crossdomain.xml` **est manquant**  
- [ ] Oui — la politique est excessivement permissive (ex: `<allow-access-from domain="*" />`)  

### Le fichier SWF peut-il être forcé de charger des animations externes contrôlées par un attaquant ?
- [ ] Non — l'application **ne peut pas** être forcée de charger des fichiers SWF externes  
- [ ] Oui — les fonctions `loadMovie` ou `loadMovieNum` acceptent des URL externes non validées

---

## WSTG-CLNT-09 — Test du Clickjacking

Le Clickjacking (également connu sous le nom de UI redressing) se produit lorsqu'un attaquant utilise des couches transparentes ou opaques pour tromper un utilisateur et l'inciter à cliquer sur un bouton ou un lien sur une autre page, alors que celui-ci pensait cliquer sur la page de premier niveau. Cette vulnérabilité compromet l'intégrité des actions de l'utilisateur, pouvant entraîner une modification non autorisée de données, des changements de compte ou des transferts financiers effectués au nom de la victime. Les attaquants exploitent généralement cela en intégrant le site web cible dans une `<iframe>` sur un site malveillant contrôlé et en le superposant avec un contenu trompeur ou des incitations attrayantes. Cela se produit le plus fréquemment sur les pages effectuant des actions sensibles modifiant l'état, telles que les boutons « Supprimer le compte », les formulaires de réinitialisation de mot de passe ou les panneaux de configuration administrative.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si des actions sensibles (ex : transferts de fonds, suppression de compte ou élévation de privilèges) peuvent être déclenchées via clickjacking.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Outils :** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### L'application implémente-t-elle des en-têtes côté serveur pour empêcher le cadrage (framing) non autorisé ?
- [ ] Oui — `X-Frame-Options` ou `Content-Security-Policy` **sont appliqués** et empêchent le cadrage *(Le plus sécurisé)*  
- [ ] Oui — les en-têtes sont présents mais **mal configurés** (ex : `X-Frame-Options: ALLOW-FROM` qui manque d'un support large par les navigateurs)  
- [ ] Non — aucun en-tête de protection contre le cadrage **n'est appliqué**  

### La directive `frame-ancestors` de la `Content-Security-Policy` (CSP) est-elle implémentée ?
- [ ] Oui — `frame-ancestors 'none'` ou `'self'` **est appliqué**  
- [ ] Oui — `frame-ancestors` est présent mais **autorise** des origines non fiables ou trop larges  
- [ ] Non — la directive est **manquante** ou la CSP n'est pas implémentée  

### L'en-tête `X-Frame-Options` (XFO) est-il correctement configuré pour le support des navigateurs anciens ?
- [ ] Oui — `DENY` ou `SAMEORIGIN` **est appliqué**  
- [ ] Oui — XFO est présent mais **ignoré** par les navigateurs modernes car une directive CSP `frame-ancestors` existe  
- [ ] Non — l'en-tête XFO est **manquant** ou défini sur une valeur non sécurisée  

### Les protections contre le cadrage peuvent-elles être contournées à l'aide de techniques connues ?
- [ ] Non — contournement **impossible** via les techniques standards (double-framing, etc.)  
- [ ] Oui — la protection **peut** être contournée en raison d'une regex ou d'une logique faible dans la liste blanche (white-list) de `frame-ancestors`  
- [ ] Oui — la protection **peut** être contournée car l'application s'appuie uniquement sur du frame-busting basé sur JavaScript  

### Une action sensible est-elle exploitable via une preuve de concept (PoC) de clickjacking ?
- [ ] Non — le cadrage est bloqué ou aucune action sensible n'est accessible  
- [ ] Oui — le UI redressing **est possible** mais nécessite une interaction utilisateur complexe  
- [ ] Oui — le UI redressing **est possible** et permet des actions immédiates de modification d'état *(Critique)*

---

## WSTG-CLNT-10 — Test des WebSockets

Le test des WebSockets se concentre sur le canal de communication full-duplex établi entre le client et le serveur, qui contourne les modèles traditionnels de requête-réponse HTTP. Les pentesteurs évaluent la sécurité du processus de handshake, la présence de protections contre le Cross-Site WebSocket Hijacking (CSWSH), et la rigueur de la validation des entrées appliquée aux payloads des messages. L'exploitation implique généralement le détournement (hijacking) d'une session active pour exfiltrer des données en temps réel ou l'injection de payloads malveillants déclenchant des vulnérabilités côté serveur telles que l'injection de commandes (Command Injection) ou SQLi via le socket persistant. Étant donné que de nombreux pare-feu d'applications Web (WAF) traditionnels n'inspectent pas efficacement le trafic WebSocket, ce protocole constitue souvent un vecteur furtif pour contourner les défenses périmétriques.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Statut du test** | Non effectué |
| **Sévérité** | Haute / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Outils :** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### La connexion WebSocket est-elle établie via un canal sécurisé ?
- [ ] Oui — `wss://` (WebSocket Secure) est imposé pour toutes les connexions  
- [ ] Non — `ws://` est utilisé, permettant l'interception par attaque de l'homme du milieu (Person-in-the-Middle - PITM)  

### L'application est-elle protégée contre le Cross-Site WebSocket Hijacking (CSWSH) ?
- [ ] Non — le serveur valide strictement l'en-tête `Origin` lors du handshake  
- [ ] Oui — la validation de l'en-tête `Origin` est faible ou **peut** être contournée via des origines nulles ou usurpées (spoofed)  
- [ ] Oui — aucune validation de l'en-tête `Origin` n'est effectuée, permettant à des attaquants cross-site d'initier une connexion *(Critique)*  

### La validation et l'assainissement (sanitization) des entrées sont-ils appliqués aux payloads des messages WebSocket ?
- [ ] Oui — tous les messages entrants sont assainis et validés par rapport à un schéma  
- [ ] Oui — une certaine validation existe mais un contournement **est possible** en utilisant des payloads encodés ou non standard  
- [ ] Non — les messages sont traités directement, permettant l'injection (XSS, SQLi, RCE) ou la manipulation logique  

### Des contrôles d'authentification et d'autorisation sont-ils effectués sur chaque message WebSocket ?
- [ ] Oui — la session est vérifiée pour chaque message ou le protocole est sans état (stateless) avec des jetons (tokens)  
- [ ] Oui — l'authentification a lieu lors du handshake, mais l'autorisation pour des actions spécifiques **n'est pas** appliquée par message  
- [ ] Non — l'authentification n'est vérifiée que lors du handshake, permettant au détournement de session d'accorder un accès complet  

### Le serveur implémente-t-il une limitation de débit (rate limiting) ou des contraintes de ressources sur le WebSocket ?
- [ ] Oui — la limitation de débit (rate limiting) et les tailles maximales de message sont **activées** et appliquées  
- [ ] Non — aucune limite n'existe, permettant un déni de service (DoS) via l'inondation de messages (message flooding) ou des payloads de grande taille

---

## WSTG-CLNT-11 — Test du Web Messaging

Le Web Messaging, ou `postMessage`, permet une communication cross-origin entre des objets window, tels qu'une page parente et une fenêtre contextuelle (popup) générée ou une iframe intégrée. Ce mécanisme est essentiel pour les fonctionnalités web modernes, mais il introduit un risque significatif si l'application réceptrice ne vérifie pas l'identité de l'expéditeur ou traite de manière incorrecte le payload du message. Les attaquants exploitent ces vulnérabilités en hébergeant un site malveillant qui intègre l'application cible et envoie des messages forgés pour déclencher des actions imprévues, exfiltrer des données sensibles ou exécuter un Cross-Site Scripting (XSS) basé sur le DOM. Une exploitation réussie se produit lorsque les écouteurs de messages (message listeners) ne valident pas strictement la propriété `origin` ou transmettent `message.data` directement dans des puits d'exécution (sinks) dangereux.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyen / Élevé |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Outils :** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### L'application utilise-t-elle l'API `postMessage` pour la communication entre documents ?
- [ ] Non — les écouteurs `postMessage` ne sont **pas** présents dans le code côté client  
- [ ] Oui — `postMessage` est utilisé pour la communication entre composants internes  
- [ ] Oui — `postMessage` est utilisé pour communiquer avec des domaines ou des widgets tiers  

### L'origine des messages entrants est-elle strictement validée ?
- [ ] Oui — l'origine est vérifiée par rapport à une liste blanche (whitelist) stricte en utilisant `===` ou une comparaison identique *(Le plus sécurisé)*  
- [ ] Oui — l'origine est validée à l'aide d'une regex, mais la logique **est** susceptible d'être contournée (ex: `^https://trusted.com`)  
- [ ] Non — la propriété `origin` n'est **pas** vérifiée, permettant à n'importe quel domaine d'envoyer des messages  
- [ ] Non — un caractère générique `*` (wildcard) est utilisé dans le `targetOrigin` de l'expéditeur, fuyant potentiellement des données vers des écouteurs malveillants  

### Le payload du message est-il assaini avant d'être traité par l'application ?
- [ ] Oui — les données sont traitées comme du texte brut et aucun puits (sink) dangereux n'est utilisé  
- [ ] Oui — les données sont analysées (ex: `JSON.parse`) et validées avant utilisation, et aucun contournement n'est **possible**  
- [ ] Non — les données du message sont transmises directement dans des puits (sinks) dangereux tels que `innerHTML`, `eval()` ou `setTimeout()`  
- [ ] Non — les données du message sont utilisées pour naviguer dans la fenêtre ou modifier le `location.href` sans validation  

### Un attaquant peut-il déclencher des actions non autorisées ou exfiltrer des données via un message malveillant ?
- [ ] Non — même avec un message falsifié, aucune action ou donnée sensible ne peut être accédée  
- [ ] Oui — un message forgé **peut** déclencher des changements d'état sensibles (ex: changement de mot de passe, mise à jour du profil)  
- [ ] Oui — un message forgé **peut** entraîner un XSS basé sur le DOM ou l'exfiltration de jetons CSRF/données de session

---

## WSTG-CLNT-12 — Test Browser Storage

Le test du stockage du navigateur consiste à analyser comment une application utilise les mécanismes côté client tels que le LocalStorage, SessionStorage, IndexedDB et WebSQL pour la persistance des données. Les informations sensibles stockées de manière non sécurisée — y compris les PII, les jetons d'authentification ou les identifiants de session — peuvent être compromises si un attaquant parvient à exploiter une vulnérabilité de Cross-Site Scripting (XSS) ou obtient un accès physique à l'appareil de l'utilisateur. Les Pentesters doivent déterminer si les données sont stockées en clair, si elles restent accessibles après la déconnexion et si le cycle de vie du stockage est géré de manière appropriée pour prévenir les fuites de données. Du point de vue d'un attaquant, ces mécanismes de stockage sont des cibles lucratives pour l'exfiltration d'informations de maintien d'état permettant le détournement de session (session hijacking) ou une persistance de compte à long terme.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des jetons de session ou des PII sensibles sont stockés dans le LocalStorage et sont accessibles via XSS.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### L'application stocke-t-elle des informations sensibles dans le stockage du navigateur ?
- [ ] Non — l'application n'utilise pas le stockage du navigateur ou ne stocke que l'état de l'interface utilisateur (UI) non sensible  
- [ ] Oui — les données sensibles sont stockées mais sont chiffrées en utilisant une cryptographie côté client robuste  
- [ ] Oui — des données sensibles (jetons, PII, secrets) sont stockées en clair *(Moyenne)*  

### Les données stockées sont-elles accessibles à des scripts non autorisés (risque XSS) ?
- [ ] Non — les données sont stockées dans des cookies HttpOnly (pas dans le stockage du navigateur) ou le stockage n'est pas utilisé  
- [ ] Oui — LocalStorage/SessionStorage est utilisé, rendant les données complètement accessibles via n'importe quel Payload XSS *(Haute)*  

### Le stockage du navigateur est-il correctement vidé lors de la déconnexion de l'utilisateur ?
- [ ] Oui — tout le stockage lié à l'application est explicitement vidé pendant le processus de déconnexion  
- [ ] Non — le stockage persiste après la déconnexion, mais ne contient pas de données de session sensibles  
- [ ] Non — les jetons d'authentification ou les PII persistent dans le stockage après la fin de la session *(Moyenne)*  

### L'application utilise-t-elle IndexedDB ou WebSQL pour des jeux de données volumineux ou sensibles ?
- [ ] Non — ces mécanismes de stockage ne sont pas utilisés  
- [ ] Oui — les données sont stockées et correctement assainies avant d'être rendues dans le DOM  
- [ ] Oui — des jeux de données sensibles sont stockés en clair au sein des structures IndexedDB/WebSQL  

### L'intégrité des données stockées peut-elle être manipulée pour affecter la logique de l'application ?
- [ ] Non — l'application valide ou signe les données avant de les traiter à partir du stockage  
- [ ] Oui — la modification des valeurs de stockage (ex : rôles d'utilisateur, drapeaux) est possible et entraîne une altération du comportement de l'application

---

## WSTG-CLNT-13 — Test de Cross Site Script Inclusion (XSSI)

L'inclusion de scripts intersites (Cross-Site Script Inclusion - XSSI) se produit lorsqu'une application web exporte des données sensibles au sein d'un fichier JavaScript généré dynamiquement ou d'une réponse JSONP, qu'un site contrôlé par un attaquant peut ensuite inclure via une balise `<script>`. Étant donné que l'inclusion de script contourne la Same-Origin Policy (SOP), un attaquant peut exécuter le script dans son propre contexte et surcharger des objets ou variables globaux pour exfiltrer les informations sensibles. Cette vulnérabilité se manifeste généralement dans des points de terminaison authentifiés qui renvoient des données spécifiques à l'utilisateur telles que des adresses e-mail, des clés API ou des métadonnées de session au format script. Du point de vue de l'attaquant, l'exploitation consiste à concevoir une page malveillante qui référence le script vulnérable et capture les changements de variables globales ou les appels de fonction qui en résultent.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute |

> *La sévérité devient Haute si les données fuitées incluent des jetons CSRF, des identifiants de session ou des PII (Données d'identification personnelle) hautement sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Des points de terminaison authentifiés renvoient-ils du JavaScript dynamique ou du JSONP contenant des informations sensibles ?
- [ ] Non — tous les scripts sont statiques et **ne peuvent pas** contenir de données spécifiques à l'utilisateur  
- [ ] Oui — des scripts dynamiques existent mais **ne contiennent pas** d'informations sensibles  
- [ ] Oui — des scripts dynamiques contiennent des PII ou des jetons et **sont** accessibles entre origines (cross-origin)  

### L'en-tête `X-Content-Type-Options: nosniff` est-il implémenté sur les réponses de scripts sensibles ?
- [ ] Oui — l'en-tête est **appliqué** et empêche le sniffing de type MIME  
- [ ] Non — l'en-tête est **manquant**, permettant potentiellement à du contenu non script d'être exécuté comme un script  

### Les scripts sensibles sont-ils protégés par des mécanismes anti-XSSI tels que des préfixes non exécutables ou des en-têtes personnalisés ?
- [ ] Oui — les scripts nécessitent un en-tête personnalisé ou un jeton qui **ne peut pas** être envoyé via une balise `<script>` standard  
- [ ] Oui — les scripts utilisent des préfixes non exécutables (ex: `)]}'`) qui **ne peuvent pas** être facilement contournés  
- [ ] Non — les scripts sont directement accessibles via des requêtes GET utilisant des identifiants ambiants *(Critique)*  

### L'exfiltration de données sensibles est-elle possible en surchargeant des variables globales ou des prototypes ?
- [ ] Non — les données sensibles ont une portée locale ou sont protégées via des fonctionnalités modernes du navigateur  
- [ ] Oui — les données sensibles sont assignées à des variables globales et **peuvent** être capturées  
- [ ] Oui — les données fuitent via des rappels (callbacks) JSONP qui **peuvent** être interceptés

---

## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Le Reverse Tabnabbing est une vulnérabilité côté client où une page ouverte via `target="_blank"` conserve une référence fonctionnelle vers la page parente à travers l'objet `window.opener`. Cela permet à un site de destination contrôlé par un attaquant de rediriger par programmation l'onglet original de confiance vers une URL malveillante, généralement une page de Phishing conçue pour dérober des identifiants. L'attaque exploite la confiance inhérente de l'utilisateur dans l'onglet original, car il est peu probable qu'il remarque que la page qu'il vient de quitter a navigué vers un domaine différent. Les pratiques de sécurité modernes exigent l'utilisation des attributs `rel="noopener"` ou `rel="noreferrer"` sur les balises d'ancrage, ou la définition de la propriété `opener` à null en JavaScript, afin d'empêcher cette communication entre fenêtres.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Basse / Moyenne* |

> *La sévérité est Basse dans la plupart des navigateurs modernes car ils définissent désormais par défaut `target="_blank"` sur `rel="noopener"`. La sévérité devient Moyenne si l'application cible des navigateurs obsolètes ou utilise `window.open()` sans définir la propriété `opener` à null.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Des liens s'ouvrant dans une nouvelle fenêtre ou un nouvel onglet sont-ils présents ?
- [ ] Non — aucun lien n'utilise `target="_blank"` ou une redirection équivalente basée sur JavaScript  
- [ ] Oui — `target="_blank"` est utilisé exclusivement sur des liens internes de confiance  
- [ ] Oui — `target="_blank"` est utilisé sur des liens externes ou des URL fournies par l'utilisateur  

### La relation `window.opener` est-elle restreinte sur les balises d'ancrage ?
- [ ] Oui — `rel="noopener"` ou `rel="noreferrer"` est **appliqué de manière cohérente** à tous les liens avec `target="_blank"` *(Le plus sécurisé)*  
- [ ] Oui — les attributs sont appliqués mais sont **manquants** sur des liens à haut risque ou générés par l'utilisateur  
- [ ] Non — les attributs ne sont **pas appliqués**, permettant à la page enfant d'accéder à `window.opener`  

### L'application est-elle vulnérable au Reverse Tabnabbing via `window.open()` en JavaScript ?
- [ ] Non — `window.open()` n'est pas utilisé ou définit explicitement la propriété `opener` à null  
- [ ] Oui — `window.open()` est utilisé et la référence `opener` **reste accessible** pour la fenêtre enfant  

### Un attaquant peut-il contrôler la destination d'un lien qui s'ouvre dans un nouvel onglet ?
- [ ] Non — toutes les destinations de liens sont codées en dur et pointent vers des domaines de confiance  
- [ ] Oui — les destinations des liens sont fournies par l'utilisateur (ex: profils de réseaux sociaux, liens de biographie) et ne sont **pas** assainies pour les protections contre le tabnabbing

---

## WSTG-APIT-01 — Reconnaissance d'API

La reconnaissance d'API est le processus systématique consistant à identifier et cartographier la surface d'attaque de l'API afin de découvrir les points de terminaison (endpoints), les méthodes supportées et les structures de données sous-jacentes. Les attaquants réalisent cette phase pour découvrir des API non documentées (shadow APIs), des versions obsolètes présentant des vulnérabilités héritées, et des fichiers de documentation exposés tels que les définitions Swagger ou OpenAPI qui révèlent la logique métier interne. En analysant les modèles d'URL prévisibles, les fichiers JavaScript côté client et les résultats du brute-force de répertoires, un adversaire peut construire une cartographie complète de l'API pour identifier des cibles pour des attaques de type Broken Object Level Authorization (BOLA) ou Mass Assignment. Cette reconnaissance cible généralement des chemins communs tels que `/api/v1/`, `/swagger.json` ou `/graphql` pour obtenir une feuille de route des fonctionnalités backend de l'application.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **État du test** | Non effectué |
| **Sévérité** | Informationnel / Faible |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Outils :** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Les fichiers de documentation d'API (Swagger, OpenAPI, WSDL) sont-ils accessibles publiquement ?
- [ ] Non — La documentation de l'API **n'est pas** accessible ou est restreinte par une authentification  
- [ ] Oui — La documentation est accessible mais nécessite des identifiants valides  
- [ ] Oui — La documentation de l'API (par exemple, `/swagger-ui.html`) est **accessible publiquement** sans authentification  

### Des points de terminaison d'API non documentés ou « shadow » peuvent-ils être découverts via fuzzing ?
- [ ] Non — Le fuzzing de répertoires et de points de terminaison n'a révélé aucun chemin non documenté  
- [ ] Oui — Des points de terminaison non documentés ont été trouvés mais ils **ne peuvent pas** être accédés sans autorisation  
- [ ] Oui — Des points de terminaison non documentés ont été trouvés et **peuvent** être accédés sans autorisation valide  

### L'application révèle-t-elle plusieurs versions d'API (par exemple, /v1/, /v2/, /beta/) ?
- [ ] Non — Seule la version actuelle et sécurisée de l'API est accessible  
- [ ] Oui — Des versions héritées existent mais implémentent les **mêmes** contrôles de sécurité que la version actuelle  
- [ ] Oui — Des versions héritées (par exemple, `/v1/`) sont accessibles et **manquent** de contrôles de sécurité par rapport aux versions plus récentes  

### Les ressources côté client (JavaScript / Applications mobiles) divulguent-elles des structures de points de terminaison d'API ou des clés ?
- [ ] Non — Le code côté client ne contient **pas** de chemins d'API ou de clés sensibles codés en dur  
- [ ] Oui — Le code côté client contient des cartographies de points de terminaison d'API mais **aucune** clé sensible  
- [ ] Oui — Des clés d'API, des secrets ou des URL de points de terminaison internes **sont** codés en dur dans les ressources côté client  

### Des paramètres ou des en-têtes cachés sont-ils découvrables sur les points de terminaison connus ?
- [ ] Non — Le fuzzing de paramètres (par exemple, via `Arjun`) n'a **pas** révélé d'entrées cachées  
- [ ] Oui — Des paramètres cachés (par exemple, `debug=true`, `admin=1`) ont été découverts mais ne sont **pas** fonctionnels  
- [ ] Oui — Des paramètres cachés ont été découverts et **peuvent** être utilisés pour modifier le comportement de l'application

---

## WSTG-APIT-02 — API Broken Object Level Authorization

Le Broken Object Level Authorization (BOLA), également connu sous le nom de Insecure Direct Object Reference (IDOR), survient lorsqu'une API ne parvient pas à valider si un utilisateur dispose des autorisations appropriées pour accéder à une ressource spécifique identifiée par un ID ou pour la manipuler. Les attaquants exploitent cette faille en énumérant systématiquement ou en devinant les identifiants — tels que des ID numériques ou des UUID — dans les chemins de requête, les paramètres de requête (query parameters) ou les corps JSON afin d'accéder à des données appartenant à d'autres utilisateurs. Cette vulnérabilité est le problème le plus courant et le plus impactant dans la sécurité des API modernes, pouvant mener à une exfiltration massive de données, à des modifications non autorisées ou à une prise de contrôle totale de compte. Du point de vue de l'attaquant, l'objectif est d'identifier les endpoints qui acceptent des identifiants d'objet et de tester si la logique d'autorisation côté serveur est absente ou peut être contournée en manipulant ces ID.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Outils :** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Les identifiants d'objet sont-ils prévisibles ou énumérables au sein des requêtes API ?
- [ ] Non — les identifiants sont longs, aléatoires et sécurisés cryptographiquement (ex: UUIDv4)  
- [ ] Oui — les identifiants sont des entiers séquentiels (ex: `101`, `102`)  
- [ ] Oui — les identifiants suivent un modèle prévisible ou sont dérivés d'informations publiques  

### L'API effectue-t-elle une validation côté serveur de la propriété de l'objet pour chaque requête ?
- [ ] Oui — les contrôles d'autorisation sont appliqués pour chaque requête et aucun contournement n'est **possible**  
- [ ] Oui — les contrôles sont appliqués mais un contournement **est possible** via Parameter Pollution ou Method Tunneling  
- [ ] Non — l'application se repose uniquement sur le fait que l'utilisateur soit authentifié sans vérifier la propriété spécifique de la ressource  

### Est-il possible d'accéder à la ressource d'un autre utilisateur ou de la modifier en changeant l'identifiant ?
- [ ] Non — l'accès non autorisé aux ressources d'autres utilisateurs n'est **pas possible**  
- [ ] Oui — l'accès non autorisé en **lecture** (BOLA horizontal) **est possible**  
- [ ] Oui — la **modification** ou la **suppression** non autorisée (BOLA horizontal) **est possible**  

### L'API permet-elle d'accéder à des objets de niveau administratif ou système en substituant les ID ?
- [ ] Non — les ressources administratives sont protégées par des couches d'autorisation secondaires  
- [ ] Oui — l'accès ou la modification de ressources de niveau système (BOLA vertical) **est possible**  

### Le contrôle d'autorisation peut-il être contourné en déplaçant l'identifiant vers une autre partie de la requête ?
- [ ] Non — la logique d'autorisation est cohérente quel que soit l'emplacement du paramètre  
- [ ] Oui — un contournement **est possible** lorsque l'ID est déplacé du chemin URL vers le corps JSON ou les en-têtes  
- [ ] Oui — un contournement **est possible** lorsque plusieurs ID sont fournis et que le serveur traite celui qui n'est pas autorisé

---

## WSTG-APIT-99 — Testing GraphQL

GraphQL est un langage de requête pour les API qui permet aux clients de demander des structures de données spécifiques, mais les mauvaises configurations entraînent souvent des risques de sécurité importants, notamment la divulgation d'informations et l'épuisement des ressources. Les vulnérabilités proviennent généralement de l'activation de l'introspection, de l'absence de limites de profondeur ou de complexité des requêtes, et du Broken Object-Level Authorization (BOLA) au sein des fonctions de résolution (resolvers). Les attaquants exploitent ces points de terminaison (endpoints) en utilisant l'introspection pour cartographier l'ensemble du schéma, en exploitant des fragments circulaires pour déclencher un Denial of Service (DoS), ou en contournant l'autorisation par l'accès à des champs non autorisés via des requêtes imbriquées. Les tests se concentrent sur les endpoints `/graphql`, `/v1/graphql`, ou `/graphiql` afin de s'assurer que l'implémentation impose des contrôles d'accès et une gestion des ressources stricts.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée / Critique* |

> *La sévérité devient Critique si le Broken Object-Level Authorization (BOLA) permet un accès non autorisé à des PII ou à des mutations administratives.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Outils :** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Le système d'introspection GraphQL est-il activé ?
- [ ] Non — l'introspection est **désactivée** et le schéma ne peut pas être cartographié  
- [ ] Oui — l'introspection est **activée** mais nécessite une authentification administrative  
- [ ] Oui — l'introspection est **activée** et accessible publiquement *(Élevée)*  

### Des limites de ressources (profondeur et complexité) sont-elles imposées sur les requêtes ?
- [ ] Oui — des limites strictes de profondeur et de complexité de requête sont **appliquées** et imposées  
- [ ] Oui — des limites sont **appliquées** mais peuvent être contournées en utilisant des alias ou des fragments  
- [ ] Non — aucune limite n'est **appliquée**, permettant des requêtes récursives et des DoS  

### Le Broken Object-Level Authorization (BOLA) est-il présent dans les résolveurs ?
- [ ] Non — l'autorisation est **appliquée** au niveau des champs et des objets pour chaque requête  
- [ ] Oui — l'autorisation est **appliquée** à certains champs mais des objets sensibles sont exposés  
- [ ] Oui — l'autorisation n'est **pas appliquée**, permettant l'accès à n'importe quel objet via la manipulation d'ID  

### Les mutations GraphQL sont-elles correctement protégées contre une utilisation non autorisée ?
- [ ] Non — les mutations ne sont **pas** présentes dans le schéma  
- [ ] Oui — les mutations nécessitent une authentification valide et des contrôles d'autorisation stricts  
- [ ] Oui — les mutations sont **possibles** pour des utilisateurs non authentifiés ou manquent d'autorisation  

### Les messages d'erreur divulguent-ils des détails sensibles sur l'implémentation ou le schéma ?
- [ ] Non — les messages d'erreur sont génériques et ne **révèlent pas** de logique interne  
- [ ] Oui — les messages d'erreur révèlent les types de base de données sous-jacents ou des suggestions via « Did you mean...? »  
- [ ] Oui — des traces d'appels (stack traces) complètes et des données de configuration sensibles sont **révélées** dans les réponses