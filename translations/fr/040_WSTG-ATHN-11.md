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