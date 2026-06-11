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