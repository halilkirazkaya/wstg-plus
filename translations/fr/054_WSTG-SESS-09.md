## WSTG-SESS-09 — Testing for Session Hijacking

Le Session Hijacking (détournement de session) se produit lorsqu'un attaquant capture, prédit ou fixe un identifiant de session (SID) valide pour obtenir un accès non autorisé à la session active d'un utilisateur. Cette vulnérabilité se manifeste généralement par une sécurité insuffisante de la couche de transport, une absence de flags de sécurité sur les cookies ou des algorithmes de génération de SID prévisibles qui permettent à un attaquant de contourner entièrement l'authentification. Du point de vue de l'attaquant, une exploitation réussie permet une usurpation d'identité (impersonation) complète de la victime sans nécessiter ses identifiants (credentials), ce qui est souvent réalisé par sniffing réseau, Cross-Site Scripting (XSS) ou des attaques de Session Fixation.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Les identifiants de session sont-ils protégés pendant le transit ?
- [ ] Oui — Le flag `Secure` est **activé** et HSTS est strictement appliqué  
- [ ] Oui — Le flag `Secure` est **activé** mais HSTS est **désactivé**  
- [ ] Non — Le flag `Secure` n'est **pas appliqué**, permettant l'interception du SID sur des canaux non chiffrés *(Élevée)*  

### L'identifiant de session est-il protégé contre l'accès par script côté client ?
- [ ] Oui — Le flag `HttpOnly` est **appliqué** à tous les cookies liés à la session  
- [ ] Non — Le flag `HttpOnly` n'est **pas appliqué**, permettant l'exfiltration du SID via XSS *(Critique)*  

### L'application implémente-t-elle une liaison de session (session binding) aux attributs du client ?
- [ ] Oui — La session est liée aux attributs du client (IP/User-Agent) et la réutilisation n'est **pas possible**  
- [ ] Oui — La liaison de session est présente mais un contournement **est possible** via le spoofing d'en-tête (header spoofing)  
- [ ] No — Aucune liaison de session n'existe ; le SID **est valide** lorsqu'il est utilisé depuis n'importe quelle source  

### L'identifiant de session est-il suffisamment aléatoire pour empêcher la prédiction ?
- [ ] Oui — Le SID utilise un générateur de nombres pseudo-aléatoires cryptographiquement sûr (CSPRNG)  
- [ ] Non — La longueur du SID est insuffisante ou suit une séquence/un motif **prévisible**  

### Les sessions concurrentes sont-elles gérées pour limiter la fenêtre d'attaque ?
- [ ] Non — L'application impose strictement une seule session active par utilisateur  
- [ ] Oui — Plusieurs sessions concurrentes sont **autorisées** mais surveillées  
- [ ] Oui — Des sessions concurrentes illimitées **sont possibles** à travers différents appareils/emplacements  

---