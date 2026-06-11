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