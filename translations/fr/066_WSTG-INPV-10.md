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