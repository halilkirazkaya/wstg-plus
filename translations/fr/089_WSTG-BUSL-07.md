## WSTG-BUSL-07 — Tester les défenses contre l'utilisation abusive de l'application

Les défenses contre l'utilisation abusive de l'application évaluent la capacité de l'application à identifier, journaliser (log) et répondre aux comportements anormaux des utilisateurs qui s'écartent de la logique métier (Business Logic) prévue. Contrairement à la sécurité traditionnelle basée sur les signatures, ces défenses se concentrent sur la détection de modèles tels que les requêtes en rafale (rapid-fire requests), le Credential Stuffing, ou l'énumération séquentielle de ressources qui signalent un abus automatisé. Du point de vue d'un attaquant, ce test détermine la résilience de l'application contre l'automatisation et les attaques « low and slow » conçues pour exfiltrer des données ou épuiser les ressources sans déclencher les alertes de sécurité standard. L'absence de mise en œuvre de ces défenses entraîne souvent du scraping massif de données, des prises de contrôle de comptes (Account Takeover) ou des dénis de service (DoS) via des fonctionnalités applicatives légitimes mais gourmandes en ressources.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Des mécanismes de Rate Limiting ou de throttling sont-ils appliqués sur les points de terminaison (endpoints) sensibles ?
- [ ] Oui — un Rate Limiting strict **est appliqué** et imposé sur tous les points de terminaison sensibles *(Le plus sécurisé)*  
- [ ] Oui — le Rate Limiting **est appliqué** mais peut être contourné via la rotation d'adresses IP ou la manipulation d'en-têtes (headers)  
- [ ] Oui — le Rate Limiting **est appliqué** uniquement aux points de terminaison d'authentification, laissant les autres exposés  
- [ ] Non — aucun Rate Limiting ou throttling **n'est appliqué** aux fonctionnalités de l'application  

### L'application détecte-t-elle et bloque-t-elle les modèles d'interaction automatisés ?
- [ ] Oui — l'analyse comportementale ou les CAPTCHAs **sont activés** et bloquent efficacement les bots  
- [ ] Oui — l'anti-automatisation **est activée** mais un contournement **est possible** en utilisant des navigateurs sans interface (headless browsers) ou la rotation de sessions  
- [ ] Non — l'application **ne peut pas** distinguer un utilisateur humain d'un script automatisé  

### Les comportements anormaux sont-ils journalisés et suivis d'une réponse défensive ?
- [ ] Oui — l'utilisation abusive déclenche un blocage automatisé et alerte l'équipe de sécurité  
- [ ] Oui — l'utilisation abusive est journalisée pour l'audit mais **aucune** action défensive automatisée n'est prise  
- [ ] Non — l'application **ne journalise pas** et ne répond pas aux modèles d'activité inhabituels  

### Les défenses peuvent-elles être contournées en manipulant les identifiants côté client ou les en-têtes (headers) ?
- [ ] Non — les défenses reposent sur l'état côté serveur et une télémétrie vérifiée  
- [ ] Oui — les contrôles **peuvent** être contournés en effaçant les cookies ou en effectuant une rotation des chaînes `User-Agent`  
- [ ] Oui — les contrôles **peuvent** être contournés en injectant des en-têtes tels que `X-Forwarded-For` ou `X-Real-IP`  

---