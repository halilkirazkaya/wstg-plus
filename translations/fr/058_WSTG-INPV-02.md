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