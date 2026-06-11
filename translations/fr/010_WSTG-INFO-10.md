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