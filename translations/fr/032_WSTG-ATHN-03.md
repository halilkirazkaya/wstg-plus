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