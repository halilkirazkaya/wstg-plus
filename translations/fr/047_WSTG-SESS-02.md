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