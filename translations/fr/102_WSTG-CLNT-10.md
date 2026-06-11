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