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