## WSTG-CONF-14 — Tester les mauvaises configurations des en-têtes de sécurité HTTP

Le test des mauvaises configurations des en-têtes de sécurité HTTP consiste à vérifier la présence et la mise en œuvre correcte des en-têtes défensifs qui fournissent une protection essentielle contre les vecteurs d'attaque web courants. Bien que ces en-têtes ne corrigent pas les vulnérabilités de code sous-jacentes, leur absence augmente considérablement le risque d'attaques de l'homme du milieu (MITM), d'exploitation du MIME-sniffing et de fuites de données involontaires entre origines via les référents (referrers). Du point de vue d'un attaquant, l'absence d'en-têtes de sécurité est un indicateur très visible d'un environnement mal durci, facilitant souvent des chaînes d'exploitation plus complexes comme le détournement de session (session hijacking) ou l'exfiltration d'informations. Ces configurations sont généralement évaluées au niveau du serveur et doivent être appliquées de manière cohérente à tous les types de réponses, y compris les points de terminaison API et les ressources statiques.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **État du test** | Non effectué |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit de présence des en-têtes de sécurité
| En-tête | Présent | Manquant | Configuration recommandée |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` ou `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Spécifique aux fonctionnalités, ex: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Comment les en-têtes de sécurité sont-ils appliqués sur l'ensemble du périmètre de l'application ?
- [ ] Oui — les en-têtes sont appliqués globalement via un répartiteur de charge (load balancer) ou un proxy inverse (reverse proxy) centralisé et **ne peuvent pas** être surchargés  
- [ ] Oui — les en-têtes sont appliqués aux réponses des documents principaux mais sont **manquants** sur les réponses API ou les ressources statiques  
- [ ] Non — les en-têtes sont appliqués de manière incohérente ou sont **manquants** sur l'ensemble du domaine de l'application  

### L'en-tête Strict-Transport-Security (HSTS) est-il correctement configuré ?
- [ ] Oui — `max-age` est défini sur une longue durée (ex: 2 ans) et `includeSubDomains` est **activé**  
- [ ] Oui — `max-age` est défini mais `includeSubDomains` est **désactivé**, laissant les sous-domaines vulnérables  
- [ ] No — l'en-tête HSTS n'est **pas présent** ou `max-age` est défini sur 0, permettant des attaques par déclassement de protocole (protocol downgrade)  

### La politique Referrer-Policy empêche-t-elle la fuite de paramètres d'URL sensibles ?
- [ ] Oui — la politique est définie sur `no-referrer` ou `same-origin` *(Le plus sécurisé)*  
- [ ] Oui — la politique est définie sur `strict-origin-when-cross-origin`  
- [ ] Non — la politique n'est **pas définie** ou est définie sur `unsafe-url`, risquant de divulguer des jetons ou des PII sensibles dans les en-têtes referer  

### L'en-tête X-Content-Type-Options empêche-t-il le MIME-sniffing ?
- [ ] Oui — la directive `nosniff` est **présente** et correctement implémentée  
- [ ] Non — l'en-tête est **manquant**, permettant potentiellement à un navigateur d'exécuter des fichiers non exécutables (ex: images) comme des scripts  

---